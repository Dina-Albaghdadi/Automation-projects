# U3 — PROVE IT WORKS

## Quality Thinking

> Building a solution is not the same as proving it works.

In this unit, I moved from building a solution to verifying its quality: designing suitable tests, comparing expected and actual results, and finding the cases the system does not handle correctly.

The goal is to move from the feeling that the solution works to having evidence that supports it.

**🌐 English** · [العربية](#-النسخة-العربية)

---

## 🎯 Capability

**Quality Thinking**

The ability to verify a system's behavior and output quality using tests, evidence, and systematic failure analysis.

---

## 🧠 What I Learned

- **Testing Strategy:** decide what needs to be tested.
- **Test Cases:** design cases that represent different uses.
- **Expected vs. Actual Results:** compare what should happen with what did happen.
- **Edge Cases:** test unusual cases and the limits of expected behavior.
- **Failure Analysis:** find the cause of a failure.
- **Iteration:** improve the solution based on test results.
- **Evidence-Based Evaluation:** document the evidence behind every verdict.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** TaskBridge — Work Planning Web Application

- **System Under Test:** A plans-and-tasks web app (Lovable + Supabase).
- **Testing Focus:** Authentication, CRUD, progress calculation, data persistence, user data isolation with RLS (two accounts), and cascade delete — 14 tests.
- **Tools:** Lovable, Supabase Auth, PostgreSQL, RLS

[Explore the Project](./Course-Project/)

### 02 — Independent Project

**Project:** JobTrack — Job Application Tracker

- **System Under Test:** A personal job-application tracker (Lovable + Supabase).
- **Testing Focus:** Positive, negative, edge-case, data access (two accounts), and performance (PageSpeed) tests.
- **Tools:** Lovable, Supabase (Database, Auth, RLS), PageSpeed Insights

[Explore the Project](./Independent-Project/)

---

## 🔍 Engineering Focus

1. What behavior should the system achieve?
2. How can I verify that behavior?
3. In which cases could the solution fail?
4. Does the actual result match the expected result?
5. What caused the failure I found?
6. Did the change lead to an improvement I can prove?

---

## 📌 Key Takeaway

**A working demonstration is useful. Repeatable tests and documented evidence provide stronger confidence.**

---

## 📈 Capability Progress

- [x] Define the testing scope.
- [x] Prepare test cases.
- [x] Define expected results.
- [x] Document actual results.
- [x] Test unusual cases.
- [x] Analyze failures.
- [x] Document improvements and re-test results.

---

## 🔗 Related Projects

- [Guided Project — TaskBridge](./Course-Project/)
- [Independent Project — JobTrack](./Independent-Project/)

---

**Next:** [U4 — GROUND ITS KNOWLEDGE](../04-Grounding%20Thinking/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U3 — أثبت أنه يعمل

> بناء الحل لا يعني إثبات أنه يعمل.

في هذه الوحدة، انتقلت من بناء الحل إلى التحقق من جودته: تصميم اختبارات مناسبة، ومقارنة النتائج المتوقعة بالفعلية، واكتشاف الحالات التي لا يتعامل معها النظام بشكل صحيح. الهدف هو الانتقال من الانطباع بأن الحل يعمل إلى امتلاك أدلة تدعم هذا الاستنتاج.

### 🧠 ماذا تعلمت

- **استراتيجية الاختبار:** تحديد ما يجب اختباره.
- **حالات الاختبار:** تصميم حالات تمثل استخدامات مختلفة.
- **المتوقع مقابل الفعلي:** مقارنة ما يجب أن يحدث بما حدث فعلًا.
- **الحالات الحدّية:** اختبار الحالات غير المعتادة.
- **تحليل الفشل:** معرفة سبب الخطأ.
- **التحسين:** تعديل الحل بناءً على نتائج الاختبار.
- **التقييم بالأدلة:** توثيق الدليل وراء كل حكم.

### 🛠️ المشاريع

- **مشروع المحاضرة — TaskBridge:** 14 اختبارًا للمصادقة، وعمليات CRUD، وحساب التقدم، وحفظ البيانات، وعزل المستخدمين بحسابين، والحذف المتسلسل.
- **المشروع المستقل — JobTrack:** اختبارات إيجابية وسلبية وحدّية، واختبار الوصول للبيانات بحسابين، واختبار الأداء.

### 📌 الخلاصة

العرض الناجح مفيد، لكن الاختبارات القابلة للتكرار والأدلة الموثقة تعطي ثقة أكبر.

</div>
