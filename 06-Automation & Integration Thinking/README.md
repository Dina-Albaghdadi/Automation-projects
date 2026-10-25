# U6 — CONNECT IT TO THE WORLD

## Integration Thinking

> Connecting tools is a means. The goal is a correct, understandable, and safe result across system boundaries.

في هذه الوحدة، أتعلم كيف أصمم علاقة بين أنظمة مستقلة حتى تعمل معًا لتحقيق نتيجة محددة: من يتحمل أي مسؤولية، وكيف تنتقل البيانات، وبأي صلاحيات، وبأي ترتيب، وكيف أثبت أن النتيجة تحققت فعلًا.

---

## 🎯 Capability

**Integration Thinking**

القدرة على تحليل وتصميم الاتصال بين تطبيق وقاعدة بيانات ومنصة أتمتة ونموذج AI، بحيث تتبادل البيانات الصحيحة وتعمل معًا بشكل موثوق.

---

## 🧠 What I Learned

- **System Boundaries:** تحديد مسؤولية كل نظام.
- **Webhooks & APIs:** اختيار طريقة الاتصال المناسبة.
- **Data Contract:** تحديد شكل الطلب والرد بين الأنظمة.
- **Asynchronous Processing:** الرد الفوري ثم المعالجة في الخلفية مع تتبع الحالة.
- **Structured AI Output:** جعل مخرجات الـAI قابلة للحفظ والعرض.
- **Idempotency:** منع تكرار العملية عند إرسال الطلب أكثر من مرة.
- **End-to-End Verification:** إثبات أن النتيجة حُفظت ووصلت، وليس فقط أن الطلب أُرسل.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** PlanCraft AI — AI Project Planner

نظام متكامل يحوّل فكرة مشروع إلى خطة منظمة عبر تطبيق وWorkflow ونموذج AI وقاعدة بيانات.

- **Problem:** تحويل فكرة إلى خطة واضحة يحتاج وقتًا وخبرة، ولا يكفيه تطبيق واحد.
- **Systems:** Lovable ← n8n Webhook ← AI Agent (Gemini) ← Supabase ← Lovable
- **Integration Focus:** عقد بيانات صغير (`request_id`)، ومعالجة غير متزامنة مع تتبع الحالة، ومخرجات AI منظمة، ومنع تكرار الخطة.
- **Tools:** Lovable، n8n، Google Gemini، Supabase

[Explore the Project](./Course-Project/)

> لا يوجد مشروع مستقل (Independent Project) لهذه الوحدة.

---

## 🔍 Engineering Focus

1. ما الأنظمة المشاركة، وما مسؤولية كل منها؟
2. ما شكل البيانات التي تنتقل بينها؟
3. ماذا يحدث إذا فشلت خطوة في المنتصف؟
4. ماذا يحدث إذا وصل نفس الطلب مرتين؟
5. كيف أثبت أن النتيجة حُفظت ووصلت للمستخدم؟

---

## 📌 Key Takeaway

**A request that was sent is not a result that was delivered.**

نجاح الإرسال لا يعني نجاح العملية؛ الدليل الحقيقي هو أن النتيجة حُفظت وظهرت للمستخدم.

---

## 🔗 Related Projects

- [Guided Project](./Course-Project/)
