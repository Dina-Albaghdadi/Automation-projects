# JobTrack — Job Application Tracker

> Web App يتابع فيه الباحث عن عمل طلبات التوظيف الخاصة به (الشركة، الوظيفة، التاريخ، الحالة)، بحساب خاص لا يرى فيه أحد بياناته غيره.

---

## 🎯 Problem

### The Context

الباحث عن عمل، وخاصة الخريج الجديد، يقدّم على عدد كبير من الوظائف عبر مصادر مختلفة: LinkedIn، ومواقع الشركات، والبريد الإلكتروني.
غالبًا يتابع هذه الطلبات في ذاكرته، أو في ملاحظات متفرقة.

### The Problem

بعد أسابيع ينسى أين قدّم، ومتى، وإلى أي مرحلة وصل كل طلب.
فيقدّم على نفس الوظيفة مرتين، أو يفوّت متابعة مقابلة، ولا يعرف كم طلبًا وصل إلى مرحلة متقدمة.

### Why It Matters

البحث عن عمل عملية طويلة ومُرهقة. سجل واحد واضح يوفّر الوقت، ويمنع الأخطاء المحرجة (مثل التقديم المكرر)، ويعطي صورة واقعية عن التقدّم.
ولأن البيانات شخصية، يجب أن تبقى خاصة بصاحبها فقط.

---

## 💡 Solution

Web App مبني بـ **Lovable** ومتصل بـ **Supabase** لقاعدة البيانات وتسجيل الدخول.

What does the system do?

1. يسمح للمستخدم بإنشاء حساب (Sign Up) مع تأكيد البريد، ثم Login وLogout.
2. يضيف المستخدم طلب توظيف، والنظام يتحقق من المدخلات (حقول مطلوبة، رابط بصيغة صحيحة، تاريخ غير مستقبلي).
3. يمنع إضافة نفس الشركة ونفس الوظيفة مرتين، حتى لو اختلفت الأحرف الكبيرة والصغيرة أو المسافات.
4. يعرض قائمة الطلبات وملخصًا بعددها حسب الحالة (`Applied`، `Interview`، `Offer`، `Rejected`)، ويسمح بتعديل الحالة والحذف.
5. يضمن أن كل مستخدم يرى ويعدّل **طلباته فقط** عبر Row Level Security في Supabase.

---

## 👤 Users

**Primary Users:**  
خريج أو باحث عن عمل يتابع طلبات التوظيف الخاصة به.

**User Need:**  
أن يسجّل كل طلب في مكان واحد، ويعرف حالته الحالية، ويتجنب التكرار، مع ضمان أن بياناته لا يراها غيره.

---

## 🧠 How I Designed the Solution

بدأت بالتصميم قبل البناء: حددت الجدول وحقوله، وقواعد التحقق، وقواعد الصلاحيات، والاختبارات التي سأثبت بها أن النظام يعمل.

ثم قسمت المسؤولية:
- **Supabase** مسؤول عن البيانات والحماية: الجدول والقيود (`CHECK`، `UNIQUE`) والـRLS كُتبت بـSQL مباشرة.
- **Lovable** مسؤول عن الواجهة: الصفحات، والفورم، ورسائل الخطأ المفهومة للمستخدم.

التحقق موجود في طبقتين: الواجهة تعطي رسالة واضحة، وقاعدة البيانات هي الحماية الأخيرة إذا تم تجاوز الواجهة.

---

## ⚙️ How It Works

```text
المستخدم يسجّل الدخول (Supabase Auth)
  ↓
يملأ فورم طلب التوظيف
  ↓
Logic (الواجهة): تحقق من الحقول وإزالة المسافات
  ↓
Logic (قاعدة البيانات): CHECK + UNIQUE + RLS (auth.uid() = user_id)
  ↓
حفظ الطلب، أو رسالة خطأ مفهومة (مثل: You already added this job at this company.)
  ↓
Output: قائمة الطلبات + ملخص الحالات للمستخدم نفسه فقط
```

---

## 🤖 Role of AI

What does AI do in this solution?

- **Lovable (AI)** ولّد واجهة التطبيق كاملة من Prompt واحد مفصّل: الصفحات، والفورم، والتحقق، ورسائل الخطأ.
- ربط الواجهة بجدول Supabase الموجود، وحوّل أخطاء قاعدة البيانات (مثل كود التكرار `23505`) إلى رسائل مفهومة.

التطبيق نفسه لا يستخدم AI أثناء التشغيل؛ هو نظام بيانات بقواعد ثابتة.

### What AI Should NOT Do

- لا يُنشئ أو يعدّل قاعدة البيانات والـRLS تلقائيًا. طلبت من Lovable صراحة ألا يغيّر قاعدة البيانات، لأن الحماية يجب أن تكون مكتوبة ومراجَعة.
- لا يُعتمد على قوله إن الشيء "يعمل". كل وظيفة أُثبتت باختبار ودليل (Screenshot).

---

## 🔄 Automation

What parts of the process are automated?

- إرسال بريد تأكيد الحساب عند التسجيل (Supabase Auth).
- ربط كل طلب بصاحبه تلقائيًا (`user_id` قيمته الافتراضية `auth.uid()`).
- التحقق من المدخلات ومنع التكرار ورفض البيانات غير الصالحة.
- حساب ملخص الحالات في الـDashboard.
- عزل بيانات كل مستخدم تلقائيًا عبر RLS.
- النشر (Deploy) على رابط عام عبر Lovable.

---

## 🗂️ Data

### Input

- بيانات الحساب: البريد الإلكتروني وكلمة المرور.
- بيانات الطلب: Company، Job title، Job link (اختياري)، Applied date، Status، Notes (اختياري).

### Output

- صف في جدول `job_applications` مرتبط بالمستخدم.
- قائمة الطلبات مع إمكانية التصفية حسب الحالة.
- ملخص: Total، Applied، Interview، Offer، Rejected.
- رسائل تحقق وأخطاء واضحة عند الإدخال غير الصحيح.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Lovable | بناء الواجهة والصفحات والوظائف، والنشر |
| Supabase Database | جدول `job_applications` والقيود (`CHECK`، `UNIQUE INDEX`) |
| Supabase Auth | Sign Up مع تأكيد البريد، وLogin، وLogout |
| Supabase RLS | كل مستخدم يقرأ ويعدّل بياناته فقط (4 Policies) |
| PageSpeed Insights | اختبار الأداء وإمكانية الوصول (Mobile) |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:**  
إنشاء قاعدة البيانات والقيود والـRLS بـSQL مباشرة في Supabase، وLovable يبني الواجهة فقط.

**Why:**  
الحماية والقواعد يجب أن تكون دقيقة ومراجَعة، لا أن يولّدها AI. كما أن رصيد Lovable المجاني محدود (5 credits)، وكتابة SQL في Supabase لا تستهلكه. النتيجة: التطبيق كاملًا بُني برسالة واحدة في Lovable.

### Decision 2

**Decision:**  
التحقق في طبقتين: الواجهة (رسائل واضحة) + قاعدة البيانات (`CHECK` و`UNIQUE` وRLS).

**Why:**  
تحقق الواجهة وحده يمكن تجاوزه بطلب مباشر إلى Supabase. قاعدة البيانات هي الحماية الأخيرة. ومنع التكرار يعتمد على `lower(trim(...))` حتى يُكتشف `GOOGLE` و`Google` كنفس الشركة.

### Decision 3

**Decision:**  
إبقاء **Confirm email** مفعّلًا، واختبار الحسابين ببريدين حقيقيين.

**Why:**  
هذا أقرب لتطبيق حقيقي وأكثر أمانًا، ويثبت أن تدفق التسجيل يعمل كاملًا.

---

## 🧪 Testing

### Test 1 — Positive

**Input:**  
User A يضيف طلبًا صحيحًا: `Google` / `Junior Automation Specialist` / `https://careers.google.com/jobs/123` / اليوم / `Applied`.

**Expected Result:**  
رسالة نجاح، والطلب يظهر في القائمة، والـDashboard: Total = 1 و Applied = 1، وصف جديد في Supabase بـ`user_id` معبأ.

**Actual Result:**  
ظهرت رسالة **Application added**، وظهر الطلب في القائمة، والـDashboard: Total = 1 و Applied = 1، وSupabase: 1 record و`user_id` معبأ.

**Status:** ✅

---

### Test 2 — Negative

**Input:**  
Company فارغ، Job title = `Data Analyst`، Job link = `linkedin.com/jobs/456` (بدون `https://`).

**Expected Result:**  
`Company is required.` و`Job link must start with http:// or https://`، ولا يُحفظ شيء.

**Actual Result:**  
ظهرت الرسالتان بالنص المتوقع، وبقيت قاعدة البيانات 1 record.

**Status:** ✅

---

### Test 3 — Edge Case

**Input:**  
نفس الطلب بكتابة مختلفة: Company = `  GOOGLE  ` (أحرف كبيرة ومسافات)، Job title = `junior automation specialist`.

**Expected Result:**  
`You already added this job at this company.` ولا يُحفظ صف جديد.

**Actual Result:**  
ظهرت الرسالة، وأُزيلت المسافات، وبقيت قاعدة البيانات 1 record.

**Status:** ✅

---

### Test 4 — Data / Access

**Input:**  
User B (حساب ثانٍ ببريد مختلف) يسجّل الدخول، ثم يفتح رابط تعديل طلب User A مباشرة:  
`/applications/5f8d3bd7-12f2-4639-9549-74db3353c3de/edit`

**Expected Result:**  
User B يرى Dashboard فارغًا، ولا تظهر بيانات User A حتى عبر الرابط المباشر.

**Actual Result:**  
Dashboard الخاص بـUser B: Total = 0. الرابط المباشر أظهر `Application not found.` دون أي بيانات لـUser A. RLS منع البيانات من قاعدة البيانات نفسها.

**Status:** ✅

---

### Test 5 — Performance (PageSpeed Insights, Mobile)

**Input:**  
`https://jobtrack-dina.lovable.app/`

**Expected Result:**  
نتائج في النطاق الأخضر (90–100)، وتسجيل أي ملاحظات.

**Actual Result:**  
Performance **98**، Accessibility **95**، Best Practices **100**، SEO **100**. FCP 1.1 s، LCP 2.3 s، TBT 0 ms، CLS 0.  
ملاحظة: PageSpeed يزور الموقع بدون تسجيل دخول، فالنتيجة تقيس صفحة Login فقط.

**Status:** ✅ (مع ملاحظات Accessibility في قسم Challenges)

---

## 📸 Evidence

### Final Result

![Final Result — Application added](./Screenshots/EV-07-t1-application-added.png)

### Workflow

![Workflow — RLS enabled with 4 policies](./Screenshots/EV-04-rls-enabled-4-policies.png)

### All Evidence

> البريد الإلكتروني مغطى في كل الصور.

| # | ماذا يثبت | الصورة |
|---|---|---|
| EV-01 | مشروع Supabase `jobtrack` يعمل | [EV-01](./Screenshots/EV-01-supabase-project.png) |
| EV-02 | Lovable مربوط بمشروع Supabase | [EV-02](./Screenshots/EV-02-lovable-supabase-connected.png) |
| EV-03 | إنشاء الجدول بـSQL نجح | [EV-03](./Screenshots/EV-03-sql-table-created.png) |
| EV-04 | RLS مفعّل و4 Policies | [EV-04](./Screenshots/EV-04-rls-enabled-4-policies.png) |
| EV-05 | Sign Up + تأكيد البريد + Login نجحت | [EV-05](./Screenshots/EV-05-login-success-dashboard.png) |
| EV-06 | Test 1: الفورم قبل الحفظ | [EV-06](./Screenshots/EV-06-t1-add-form.png) |
| EV-07 | Test 1: رسالة النجاح والطلب في القائمة | [EV-07](./Screenshots/EV-07-t1-application-added.png) |
| EV-08 | Test 1: عدادات الـDashboard | [EV-08](./Screenshots/EV-08-t1-dashboard-count.png) |
| EV-09 | Test 1: الصف في Supabase مع `user_id` | [EV-09](./Screenshots/EV-09-t1-supabase-row.png) |
| EV-10 | Test 2: رسائل الخطأ | [EV-10](./Screenshots/EV-10-t2-validation-errors.png) |
| EV-11 | Test 2: قاعدة البيانات ما زالت 1 record | [EV-11](./Screenshots/EV-11-t2-supabase-still-1-record.png) |
| EV-12 | Test 3: رسالة التكرار | [EV-12](./Screenshots/EV-12-t3-duplicate-message.png) |
| EV-13 | Test 3: قاعدة البيانات ما زالت 1 record | [EV-13](./Screenshots/EV-13-t3-supabase-still-1-record.png) |
| EV-14 | Test 4: بريد تأكيد حساب User B | [EV-14](./Screenshots/EV-14-t4-user-b-confirm-email.png) |
| EV-15 | Test 4: Dashboard الخاص بـUser B فارغ | [EV-15](./Screenshots/EV-15-t4-user-b-sees-nothing.png) |
| EV-16 | Test 4: الرابط المباشر لطلب User A → `Application not found.` | [EV-16](./Screenshots/EV-16-t4-direct-url-blocked.png) |
| EV-17 | Test 5: نتائج PageSpeed (Mobile) | [EV-17](./Screenshots/EV-17-pagespeed-mobile-scores.png) |
| EV-18 | Test 5: مقاييس الأداء | [EV-18](./Screenshots/EV-18-pagespeed-performance-metrics.png) |
| EV-19 | Test 5: Diagnostics | [EV-19](./Screenshots/EV-19-pagespeed-diagnostics.png) |

---

## ⚠️ Challenges & Failures

What didn't work as expected?

- **رصيد Lovable محدود:** 5 credits فقط، ولا مجال لإعادة البناء عدة مرات.
- **محاولة اختبار غير صحيحة:** في أول محاولة لـTest 4 أُلصق صف كامل من الجدول بدل `id` فقط، فظهرت صفحة 404 بسبب رابط خاطئ، وليس بسبب الحماية.
- **Accessibility = 95:** PageSpeed وجد مشكلتين: تباين ألوان ضعيف في تبويب **Sign up** غير المختار، وعدم وجود عنصر `<main>` في الصفحة.
- **التسجيل ببريد مسجّل مسبقًا:** يظهر `Check your email to confirm your account.` بدل رسالة `Email already registered`.

How did I solve it?

- كتبت قاعدة البيانات والـRLS بـSQL في Supabase، وكتبت Prompt واحدًا مفصّلًا للواجهة، فبُني التطبيق برسالة واحدة.
- لم أعتمد صفحة 404 كدليل، وأعدت الاختبار بالرابط الصحيح، فظهر `Application not found.` (EV-16).
- مشاكل Accessibility ورسالة التسجيل: **سُجّلت كـBugs، والإصلاح قيد التنفيذ** عبر ربط المشروع بـGitHub وتعديل الكود، ثم إعادة الاختبار (Re-test).

---

## 📚 What I Learned

### Technical Lesson

الحماية الحقيقية للبيانات تكون في قاعدة البيانات (RLS)، وليس في إخفاء الأزرار في الواجهة. أثبت ذلك عندما فتح User B رابط طلب User A مباشرة ولم يحصل على أي بيانات.

### Problem-Solving Lesson

"لا تفترض أنه يعمل، اختبره". حتى نتيجة تبدو ناجحة (صفحة 404) قد تكون لسبب خاطئ. يجب التحقق من أن الاختبار نفسه صحيح قبل اعتماد النتيجة.

### AI Lesson

AI سريع جدًا في بناء الواجهة عندما يكون الـPrompt دقيقًا ومحددًا، لكنه قد يترك تفاصيل (مثل تباين الألوان، أو رسالة البريد المسجل). المراجعة والاختبار مسؤوليتي أنا.

---

## 🔮 Future Improvements

If I continued developing this project, I would:

- إصلاح مشاكل Accessibility ورسالة التسجيل المكرر، وإعادة الاختبار حتى Accessibility = 100.
- إضافة تذكير بالبريد لمتابعة الطلبات التي لم تتغير حالتها منذ مدة.
- إضافة بحث في الطلبات، وتصدير القائمة إلى CSV.
- إضافة مرحلة `Assessment` أو `Withdrawn` إلى الحالات.

---

## 🔗 Project Links

**Live Demo:** [jobtrack-dina.lovable.app](https://jobtrack-dina.lovable.app/)

**Workflow:** —

**Repository:** _سيُضاف بعد ربط مشروع Lovable بـGitHub_

---

## 👩‍💻 Author

**Dina Albaghdadi**

[LinkedIn](https://www.linkedin.com/in/dina-albaghdadi-79bb9a25a/) · [GitHub](https://github.com/Dina-Albaghdadi)
