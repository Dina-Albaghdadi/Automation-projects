# U6 — CONNECT IT TO THE WORLD

## Integration Thinking

> Connecting tools is a means. The goal is a correct, understandable, and safe result across system boundaries.

In this unit, I learned how to design the relationship between independent systems so they work together toward one result: which system owns which responsibility, how data moves between them, with which permissions and in which order, and how to prove the result really happened.

**🌐 English** · [العربية](#-النسخة-العربية)

---

## 🎯 Capability

**Integration Thinking**

The ability to analyze and design the connection between an app, a database, an automation platform, and an AI model, so they exchange the right data and work together reliably.

---

## 🧠 What I Learned

- **System Boundaries:** define each system's responsibility.
- **Webhooks & APIs:** choose the right way to connect.
- **Data Contract:** define the shape of requests and responses between systems.
- **Asynchronous Processing:** respond immediately, process in the background, and track the status.
- **Structured AI Output:** make AI output storable and displayable.
- **Idempotency:** prevent the same operation from running twice.
- **End-to-End Verification:** prove the result was saved and delivered, not only that a request was sent.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** PlanCraft AI — AI Project Planner

- **Problem:** Turning an idea into a clear plan takes time and experience, and one app alone cannot do it well.
- **Systems:** Lovable → n8n Webhook → AI Agent (Gemini) → Supabase → Lovable
- **Integration Focus:** a small data contract (`request_id`), asynchronous processing with status tracking, structured AI output, and duplicate prevention.
- **Tools:** Lovable, n8n, Google Gemini, Supabase

[Explore the Project](./Course-Project/)

> This unit has no independent project.

---

## 🔍 Engineering Focus

1. Which systems take part, and what is each one responsible for?
2. What data moves between them, and in what shape?
3. What happens if a step fails in the middle?
4. What happens if the same request arrives twice?
5. How do I prove the result was saved and reached the user?

---

## 📌 Key Takeaway

**A request that was sent is not a result that was delivered.**

---

## 🔗 Related Projects

- [Guided Project — PlanCraft AI](./Course-Project/)

---

**Next:** [U7 — MAKE IT RELIABLE](../07-Reliability%20%26%20Production%20Thinking/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U6 — اربطه بالعالم

> ربط الأدوات وسيلة؛ الهدف نتيجة صحيحة ومفهومة وآمنة عبر حدود الأنظمة.

في هذه الوحدة، تعلمت كيف أصمم العلاقة بين أنظمة مستقلة حتى تعمل معًا لتحقيق نتيجة واحدة: من يتحمل أي مسؤولية، وكيف تنتقل البيانات، وبأي صلاحيات، وبأي ترتيب، وكيف أثبت أن النتيجة تحققت فعلًا.

### 🧠 ماذا تعلمت

- **حدود الأنظمة:** تحديد مسؤولية كل نظام.
- **الـWebhooks والـAPIs:** اختيار طريقة الاتصال المناسبة.
- **عقد البيانات:** تحديد شكل الطلب والرد بين الأنظمة.
- **المعالجة غير المتزامنة:** الرد الفوري ثم المعالجة في الخلفية مع تتبع الحالة.
- **مخرجات AI منظمة:** جعل مخرجات الـAI قابلة للحفظ والعرض.
- **منع التكرار:** عدم تنفيذ العملية مرتين.
- **التحقق من البداية للنهاية:** إثبات أن النتيجة حُفظت ووصلت، وليس فقط أن الطلب أُرسل.

### 🛠️ المشاريع

- **مشروع المحاضرة — PlanCraft AI:** نظام يحوّل فكرة مشروع إلى خطة منظمة عبر تطبيق وWorkflow ونموذج AI وقاعدة بيانات.

> لا يوجد مشروع مستقل لهذه الوحدة.

### 📌 الخلاصة

نجاح الإرسال لا يعني نجاح العملية؛ الدليل الحقيقي هو أن النتيجة حُفظت وظهرت للمستخدم.

</div>
