# TaskBridge — Lovable Build Prompt

> The prompt used to build TaskBridge in Lovable, connected to a Supabase project.

## How it was used

1. Created a new Lovable project and connected it to a new Supabase project.
2. Sent the prompt below to Lovable.
3. Lovable created the tables, constraints, RLS policies, and pages, and reported what it could and could not verify (it could not test signed-in flows).
4. Completed the verification on the published app: configured the Supabase site URL, then tested every flow with two accounts — see the test results in the [README](./README.md#-testing) and the implemented schema in [database-schema.md](./database-schema.md).

## The prompt

```text
Build a complete, minimal, production-conscious web application
called TaskBridge using the current Lovable project and its
connected Supabase project.

IMPORTANT:
This is a new application. Build it from scratch in the current
project without adding unnecessary features.

Do not claim the application is fully functional until the
relevant implementation has been checked.

==================================================
1. PRODUCT DEFINITION
==================================================

Product name: TaskBridge

Tagline:
Turn unclear requests into clear, actionable plans.

Target users:
Freelancers and early-career professionals.

Problem:
Users often receive work requests without a clear breakdown
of the tasks, priorities, and expected deliverables.

Solution:
TaskBridge lets users create work plans, organize tasks,
track progress, and return to their saved plans.

Build a small, functional MVP.

The application must use Supabase for authentication and
persistent database storage.

Do not use mock data as the actual application data source.

==================================================
2. TECHNOLOGY AND PROJECT RULES
==================================================

- Use the existing Lovable application stack.
- Use the already connected Supabase project.
- Use Supabase Auth for authentication.
- Use Supabase PostgreSQL for application data.
- Reuse the existing Supabase connection.
- Do not create a second backend.
- Do not recreate existing tables unnecessarily.
- Do not delete existing database data.
- Do not expose service-role keys or private secrets.
- Do not add unnecessary libraries or complex architecture.
- Do not add features outside the scope of this prompt.

If Supabase is not connected, stop and tell me exactly
what I need to do to connect it. Do not build a fake backend.

==================================================
3. APPLICATION PAGES
==================================================

Create these pages:

A. LANDING PAGE

Display:
- TaskBridge logo or simple text wordmark.
- Headline: Turn unclear requests into clear, actionable plans.
- A short description of the product.
- Get Started button.
- Sign In button.

Keep the page professional, simple, and responsive.

B. AUTHENTICATION

Implement:
- Sign up with email and password.
- Login.
- Logout.
- Session persistence using Supabase Auth.
- Protected application pages.
- Clear loading and error states.

Use Supabase Auth. Do not create a custom authentication
system or store passwords in application tables.

Respect the configured email-confirmation settings.
Do not assume sign-up automatically creates an authenticated
session if email confirmation is required.

After successful login, redirect to My Plans.

C. MY PLANS

Display only the authenticated user's saved plans.

Each plan card should show:
- Plan title.
- Goal.
- Deadline, if provided.
- Total task count.
- Completed task count.
- Progress percentage.

Include:
- Create New Plan.
- Open Plan.
- Delete Plan, with confirmation.

Provide useful loading, empty, and error states.

Do not display hardcoded sample plans.

D. CREATE PLAN

Create a form with:

- Plan Title: required.
- Task Description: required.
- Desired Outcome: required.
- Deadline: optional.

Validate required fields.

For this MVP, create the plan using the supplied information.
Allow the user to add tasks manually after creating the plan.

Do not implement AI generation in this version.
Do not display fake AI-generated content.

E. PLAN DETAILS

Display:
- Plan title.
- Task description.
- Goal.
- Desired deliverables.
- Next step.
- Deadline.
- Tasks.
- Overall progress.

Allow the owner to:
- Edit the plan.
- Add a task.
- Edit a task.
- Delete a task.
- Change task priority.
- Change task status.
- Delete the plan after confirmation.

Task statuses:
- To Do
- In Progress
- Done

Task priorities:
- High
- Medium
- Low

Calculate progress from completed tasks divided by total tasks.

If a plan has no tasks, display 0% progress.

Do not store a manually entered progress percentage.

==================================================
4. DATABASE SCHEMA
==================================================

Create the following schema in the connected Supabase project.

Before creating anything, inspect the existing schema.
Reuse compatible existing tables where appropriate.
Do not drop tables or delete user data.

TABLE: plans

Fields:

- id: UUID, primary key, generated automatically.
- user_id: UUID, not null, references auth.users(id)
  with ON DELETE CASCADE.
- title: text, not null.
- description: text, not null.
- goal: text, not null.
- deadline: date, nullable.
- deliverables: JSONB, not null, default empty JSON array.
- next_step: text, nullable.
- created_at: timestamptz, not null, default now().
- updated_at: timestamptz, not null, default now().

Add a unique constraint on (id, user_id) so tasks can
reference their parent plan and owner together.

Update updated_at automatically when a plan is modified,
using a database trigger if appropriate.

TABLE: plan_tasks

Fields:

- id: UUID, primary key, generated automatically.
- plan_id: UUID, not null.
- user_id: UUID, not null, default auth.uid(),
  references auth.users(id) with ON DELETE CASCADE.
- title: text, not null.
- description: text, nullable.
- priority: text, not null, default 'Medium'.
- status: text, not null, default 'To Do'.
- sort_order: integer, not null, default 0.
- created_at: timestamptz, not null, default now().

Add database constraints allowing only these values:

priority:
- High
- Medium
- Low

status:
- To Do
- In Progress
- Done

Create a composite foreign key:
(plan_id, user_id) references plans(id, user_id)
with ON DELETE CASCADE.

This must ensure that a task's user_id matches the owner
of the associated plan.

Add an index on plan_tasks(plan_id) if useful.

Use migrations or the supported Supabase schema-management
workflow to apply the changes.

If a migration fails, inspect the actual error and fix
the specific cause. Do not create duplicate tables or
silently ignore failed migrations.

==================================================
5. ROW LEVEL SECURITY
==================================================

Enable Row Level Security on both tables.

For plans, implement policies that enforce:

SELECT:
A user can read only rows where user_id = auth.uid().

INSERT:
A user can insert only rows where user_id = auth.uid().

UPDATE:
A user can update only their own rows.
Enforce ownership both before and after the update.

DELETE:
A user can delete only their own rows.

For plan_tasks, implement policies that enforce:

SELECT:
A user can read only their own tasks.

INSERT:
A user can insert a task only when:
- task.user_id = auth.uid()
- the associated plan belongs to auth.uid()

UPDATE:
A user can update only their own tasks.
Enforce ownership before and after the update.
Ensure the task remains associated with a plan owned
by the authenticated user.

DELETE:
A user can delete only their own tasks.

Use the composite foreign key to ensure that the task
owner and parent plan owner match.

Do not rely only on frontend checks for security.
Use database-level RLS policies.

Do not grant anonymous users access to private plans or tasks.

Do not expose the Supabase service-role key in frontend code.

==================================================
6. DATA OPERATIONS
==================================================

Connect every relevant interface action to Supabase.

Implement:

- Create plan.
- Fetch the current user's plans.
- Fetch a selected plan and its tasks.
- Update plan.
- Delete plan.
- Create task.
- Update task.
- Delete task.
- Update task status.
- Update task priority.

Requirements:

- Data must persist after a browser refresh.
- Use the authenticated user's session.
- Never trust a user_id supplied by the client without
  enforcing ownership at the database level.
- Do not report success before a database operation succeeds.
- Prevent duplicate submissions while a request is processing.
- Display clear error messages when an operation fails.
- Update the UI after successful database operations.
- Handle empty results without displaying fake data.

Deleting a plan must also delete its associated tasks through
the configured database relationship.

If a database operation fails, preserve the user's input
where practical and explain the error.

==================================================
7. UI AND DESIGN
==================================================

Create a polished, professional, mobile-first web application.

Design requirements:

- Responsive on mobile and desktop.
- Clear visual hierarchy.
- Readable typography.
- Consistent spacing.
- Simple, professional cards.
- Restrained color palette.
- Accessible form labels.
- Clear buttons and navigation.
- Visible loading, empty, success, and error states.

Keep the navigation minimal:
- My Plans.
- Create Plan.
- Account / Logout.

Do not add excessive animations or decorative elements.

The application should look like a real productivity product,
not a generic template.

==================================================
8. STRICT SCOPE
==================================================

Do not implement:

- AI plan generation.
- AI chat.
- Fake AI responses.
- Team collaboration.
- Admin dashboards.
- Payments.
- Notifications.
- External integrations.
- Job boards.
- Unnecessary profile pages.
- Unnecessary settings pages.
- Unrequested features.

Prioritize a complete, working MVP over feature quantity.

==================================================
9. VERIFICATION
==================================================

After implementation, inspect the actual implementation.

Check the following where possible:

1. Supabase connection is configured.
2. Required tables and constraints exist.
3. RLS is enabled on both tables.
4. Policies enforce user ownership.
5. Task ownership matches parent-plan ownership.
6. Authentication and protected pages are implemented.
7. Create, read, update, and delete operations use Supabase.
8. Data is loaded from the database after refresh.
9. Task progress is calculated correctly.
10. Errors are handled without false success messages.
11. No private service-role key is exposed in frontend code.
12. No fake data is used as the persistent data source.

Do not claim a test passed unless it was actually run
and its result was observed.

If you cannot verify something, explicitly identify it
as unverified.

Do not rebuild working features just to improve their styling.

If an issue is found, fix only the confirmed issue.

==================================================
10. FINAL SUMMARY
==================================================

When finished, provide a concise report containing:

- Pages and features implemented.
- Supabase tables created or reused.
- Database constraints and relationships configured.
- RLS policies configured.
- Authentication configuration requirements.
- Tests actually performed and their observed results.
- Any remaining setup steps or unresolved errors.

Do not claim the application is production-ready merely
because implementation is complete.
```
