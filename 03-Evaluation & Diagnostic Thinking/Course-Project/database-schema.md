# TaskBridge — Database Schema

> The schema implemented in the Supabase project (from [`supabase/migrations`](https://github.com/Dina-Albaghdadi/taskbridge-plans/tree/main/supabase/migrations)), verified by testing the running app with two accounts.

---

## 1. Relationships

```text
auth.users ──< plans ──< plan_tasks
   (1)          (N)         (N)

plans.user_id       → auth.users.id   (ON DELETE CASCADE)
plan_tasks.user_id  → auth.users.id   (ON DELETE CASCADE)
(plan_tasks.plan_id, plan_tasks.user_id) → (plans.id, plans.user_id)   (ON DELETE CASCADE)
```

- A user owns many plans; a plan contains many tasks.
- The **composite foreign key** makes it impossible for a task to belong to a plan owned by someone else.
- Deleting a plan deletes its tasks; deleting a user deletes their plans and tasks.

---

## 2. Tables

### `plans`

| Column | Type | Rules |
|---|---|---|
| `id` | uuid | Primary key, `gen_random_uuid()` |
| `user_id` | uuid | Not null, default `auth.uid()`, FK → `auth.users(id)` cascade |
| `title` | text | Not null |
| `description` | text | Not null |
| `goal` | text | Not null |
| `deadline` | date | Nullable |
| `deliverables` | jsonb | Not null, default `[]` |
| `next_step` | text | Nullable |
| `created_at` | timestamptz | Not null, default `now()` |
| `updated_at` | timestamptz | Not null, default `now()`, refreshed by trigger |

Constraint: `UNIQUE (id, user_id)` — the target of the tasks' composite foreign key.

### `plan_tasks`

| Column | Type | Rules |
|---|---|---|
| `id` | uuid | Primary key, `gen_random_uuid()` |
| `plan_id` | uuid | Not null, part of composite FK |
| `user_id` | uuid | Not null, default `auth.uid()`, FK → `auth.users(id)` cascade |
| `title` | text | Not null |
| `description` | text | Nullable |
| `priority` | text | Not null, default `Medium`, CHECK ∈ `High`, `Medium`, `Low` |
| `status` | text | Not null, default `To Do`, CHECK ∈ `To Do`, `In Progress`, `Done` |
| `sort_order` | integer | Not null, default `0` |
| `created_at` | timestamptz | Not null, default `now()` |

Index: `plan_tasks_plan_id_idx` on `plan_id` (tasks are always loaded per plan).

**Progress is not stored.** It is calculated as `Done tasks ÷ total tasks × 100`, and `0%` when a plan has no tasks.

---

## 3. Row Level Security

RLS is **enabled on both tables**. Only the `authenticated` role has table privileges; anonymous visitors have no access.

| Table | Operation | Policy |
|---|---|---|
| `plans` | SELECT | `user_id = auth.uid()` |
| `plans` | INSERT | `WITH CHECK user_id = auth.uid()` |
| `plans` | UPDATE | `USING` and `WITH CHECK user_id = auth.uid()` (ownership before and after) |
| `plans` | DELETE | `user_id = auth.uid()` |
| `plan_tasks` | SELECT | `user_id = auth.uid()` |
| `plan_tasks` | INSERT | `user_id = auth.uid()` **and** the plan belongs to `auth.uid()` |
| `plan_tasks` | UPDATE | Own task before; after the update still own task **and** own plan |
| `plan_tasks` | DELETE | `user_id = auth.uid()` |

The app uses the public (`anon`) client key; the `service_role` key is not used in the frontend.

---

## 4. Verification (from the running app)

| Rule | How it was verified | Result |
|---|---|---|
| Users see only their own plans | User B's *My Plans* showed none of User A's plans, and the reverse | ✅ |
| Direct access is blocked by the database | User B opened User A's plan URL → *Plan not found*, no data | ✅ |
| Cascade delete | Deleted a plan with two tasks → both rows gone from `plan_tasks` in Supabase | ✅ |
| Allowed values only | Priority and status can only be chosen from the allowed lists | ✅ |
| Progress calculation | 20%, 17%, 25%, 0 of 3 = 0%, 67%, 33%, empty plan = 0% | ✅ |

---

## 5. SQL (as applied)

```sql
CREATE TABLE public.plans (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL DEFAULT auth.uid() REFERENCES auth.users(id) ON DELETE CASCADE,
  title text NOT NULL,
  description text NOT NULL,
  goal text NOT NULL,
  deadline date,
  deliverables jsonb NOT NULL DEFAULT '[]'::jsonb,
  next_step text,
  created_at timestamptz NOT NULL DEFAULT now(),
  updated_at timestamptz NOT NULL DEFAULT now(),
  CONSTRAINT plans_id_user_id_key UNIQUE (id, user_id)
);
GRANT SELECT, INSERT, UPDATE, DELETE ON public.plans TO authenticated;
GRANT ALL ON public.plans TO service_role;
ALTER TABLE public.plans ENABLE ROW LEVEL SECURITY;
CREATE POLICY "plans_select_own" ON public.plans FOR SELECT TO authenticated USING (user_id = auth.uid());
CREATE POLICY "plans_insert_own" ON public.plans FOR INSERT TO authenticated WITH CHECK (user_id = auth.uid());
CREATE POLICY "plans_update_own" ON public.plans FOR UPDATE TO authenticated USING (user_id = auth.uid()) WITH CHECK (user_id = auth.uid());
CREATE POLICY "plans_delete_own" ON public.plans FOR DELETE TO authenticated USING (user_id = auth.uid());

CREATE OR REPLACE FUNCTION public.set_updated_at() RETURNS trigger LANGUAGE plpgsql SET search_path = public AS $$
BEGIN NEW.updated_at = now(); RETURN NEW; END; $$;
CREATE TRIGGER plans_set_updated_at BEFORE UPDATE ON public.plans FOR EACH ROW EXECUTE FUNCTION public.set_updated_at();

CREATE TABLE public.plan_tasks (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  plan_id uuid NOT NULL,
  user_id uuid NOT NULL DEFAULT auth.uid() REFERENCES auth.users(id) ON DELETE CASCADE,
  title text NOT NULL,
  description text,
  priority text NOT NULL DEFAULT 'Medium' CHECK (priority IN ('High','Medium','Low')),
  status text NOT NULL DEFAULT 'To Do' CHECK (status IN ('To Do','In Progress','Done')),
  sort_order integer NOT NULL DEFAULT 0,
  created_at timestamptz NOT NULL DEFAULT now(),
  CONSTRAINT plan_tasks_plan_owner_fkey FOREIGN KEY (plan_id, user_id) REFERENCES public.plans(id, user_id) ON DELETE CASCADE
);
CREATE INDEX plan_tasks_plan_id_idx ON public.plan_tasks(plan_id);
GRANT SELECT, INSERT, UPDATE, DELETE ON public.plan_tasks TO authenticated;
GRANT ALL ON public.plan_tasks TO service_role;
ALTER TABLE public.plan_tasks ENABLE ROW LEVEL SECURITY;
CREATE POLICY "tasks_select_own" ON public.plan_tasks FOR SELECT TO authenticated USING (user_id = auth.uid());
CREATE POLICY "tasks_insert_own" ON public.plan_tasks FOR INSERT TO authenticated WITH CHECK (
  user_id = auth.uid() AND EXISTS (SELECT 1 FROM public.plans p WHERE p.id = plan_id AND p.user_id = auth.uid()));
CREATE POLICY "tasks_update_own" ON public.plan_tasks FOR UPDATE TO authenticated USING (user_id = auth.uid()) WITH CHECK (
  user_id = auth.uid() AND EXISTS (SELECT 1 FROM public.plans p WHERE p.id = plan_id AND p.user_id = auth.uid()));
CREATE POLICY "tasks_delete_own" ON public.plan_tasks FOR DELETE TO authenticated USING (user_id = auth.uid());
```
