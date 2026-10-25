# U5 — CONTROL ITS AGENCY

## Agent & Decision Thinking

> An AI agent is not just a chatbot connected to tools. It decides, uses tools, and must operate within controlled boundaries.

In this unit, I learned how to design an AI agent that makes decisions and uses tools to reach a goal, while controlling its autonomy: when to ask, when to wait, when to act, and how to verify the result.

**🌐 English** · [العربية](#-النسخة-العربية)

---

## 🎯 Capability

**Agent & Decision Thinking**

The ability to design an agent that collects, validates, and confirms information before it executes an action, within clear boundaries.

---

## 🧠 What I Learned

- **Agent vs. Workflow:** when a solution really needs decisions.
- **Tools & Memory:** connect an agent to tools and to conversation memory.
- **Collect → Validate → Confirm → Execute:** never act before confirmation.
- **Boundaries:** what the agent may and may not do.
- **Idempotency:** prevent the same action from running twice.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** XYZ Real Estate — AI Agent

- **Problem:** Customer requirements arrive scattered and must be recorded correctly and confirmed.
- **Solution:** A Telegram agent that collects nine fields, applies business rules, and saves to Google Sheets only after confirmation.
- **Tools:** n8n, Telegram, OpenRouter, Simple Memory, Google Sheets

[Explore the Project](./Course-Project/)

### 02 — Independent Project

**Project:** My Daily Progress — Telegram AI Agent *(in progress)*

- **Problem:** It is hard to see honestly what I did and what I skipped from my daily routine.
- **Solution:** A bot that logs tasks from free-text messages after confirmation and sends an end-of-day report.
- **Tools:** n8n, Telegram, Google Sheets

[Explore the Project](./Independent-Project/)

---

## 🔍 Engineering Focus

1. Does this problem need an agent, or is a fixed workflow enough?
2. What information must the agent collect before acting?
3. When should it ask instead of guessing?
4. What prevents it from acting before confirmation?
5. How do we know the action really succeeded?

---

## 📌 Key Takeaway

**Complete information is not the same as approved information.**

---

## 🔗 Related Projects

- [Guided Project — XYZ Real Estate](./Course-Project/)
- [Independent Project — My Daily Progress](./Independent-Project/)

---

**Next:** [U6 — CONNECT IT TO THE WORLD](../06-Automation%20%26%20Integration%20Thinking/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U5 — تحكّم باستقلاليته

> الـAI Agent ليس مجرد Chatbot مربوط بأدوات؛ إنه يقرر ويستخدم الأدوات، ويجب أن يعمل ضمن حدود واضحة.

في هذه الوحدة، تعلمت كيف أصمم Agent يتخذ القرارات ويستخدم الأدوات لتحقيق هدف محدد، مع التحكم في استقلاليته: متى يسأل، ومتى ينتظر، ومتى ينفّذ، وكيف يتحقق من النتيجة.

### 🧠 ماذا تعلمت

- **الـAgent مقابل الـWorkflow:** متى يحتاج الحل إلى اتخاذ قرارات.
- **الأدوات والذاكرة:** ربط الـAgent بأدوات وذاكرة محادثة.
- **اجمع ← تحقق ← أكّد ← نفّذ:** عدم التنفيذ قبل التأكيد.
- **الحدود:** ما يُسمح للـAgent بفعله وما لا يُسمح.
- **منع التكرار:** عدم تنفيذ نفس العملية مرتين.

### 🛠️ المشاريع

- **مشروع المحاضرة — XYZ Real Estate:** Agent على تيليجرام يجمع تسعة حقول ويطبق قواعد العمل ويحفظ في Google Sheets بعد التأكيد فقط.
- **المشروع المستقل — My Daily Progress** *(قيد التنفيذ)*: بوت يسجّل مهام الروتين اليومي من رسائل حرة بعد التأكيد، ويرسل تقريرًا يوميًا.

### 📌 الخلاصة

اكتمال المعلومات لا يعني الموافقة عليها.

</div>
