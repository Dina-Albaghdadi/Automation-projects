# U7 — MAKE IT RELIABLE

## Reliability & Production Thinking

> Understand the problem. Choose the architecture. Build. Test. Improve.

In this unit, I moved from **building** AI solutions to **engineering** them: analyzing the problem before choosing tools, choosing the right architecture, testing the system in normal and abnormal cases, and diagnosing and fixing failures.

**🌐 English** · [العربية](#-النسخة-العربية)

---

## 🎯 Capability

**Reliability & Production Thinking**

The ability to choose the architecture that fits the problem (workflow, AI workflow, RAG, or AI agent), and build a solution that can be relied on, with a clear view of where it can fail.

---

## 🧠 What I Learned

- **Architecture Choice:** when a problem needs a workflow, RAG, or an AI agent.
- **Knowledge Pipelines:** turn documents into embeddings stored in a vector database.
- **Retrieval as a Tool:** let an agent search the knowledge base as a tool.
- **Knowledge vs. Memory:** permanent knowledge versus conversation context.
- **Retrieval vs. Generation:** locate the source of a wrong answer.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** RAG with n8n — Knowledge Pipeline & RAG AI Agent

- **Problem:** In Dify the RAG steps were hidden, so they were hard to understand or reuse.
- **Solution:** A pipeline that turns PDFs into embeddings in Supabase, and an agent that answers after searching them.
- **Tools:** n8n, Google Drive, Supabase (pgvector), OpenAI Embeddings, Google Gemini

[Explore the Project](./Course-Project/)

> This unit has no independent project.

---

## 🔍 Engineering Focus

1. What architecture does this problem really need?
2. Where does the knowledge come from, and how is it prepared?
3. How does the system find the right information for each question?
4. When an answer is wrong, is the cause the knowledge or the answer step?

---

## 📌 Key Takeaway

**Choose the architecture from the problem, not from the tool.**

---

## 🔗 Related Projects

- [Guided Project — RAG with n8n](./Course-Project/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U7 — اجعله موثوقًا

> افهم المشكلة، اختر المعمارية، ابنِ، اختبر، حسّن.

في هذه الوحدة، انتقلت من **بناء** حلول الذكاء الاصطناعي إلى **هندستها**: تحليل المشكلة قبل اختيار الأدوات، واختيار المعمارية المناسبة، واختبار سلوك النظام في الحالات الطبيعية وغير الطبيعية، وتشخيص الأعطال ومعالجتها.

### 🧠 ماذا تعلمت

- **اختيار المعمارية:** متى أحتاج Workflow، ومتى RAG، ومتى AI Agent.
- **مسارات تجهيز المعرفة:** تحويل المستندات إلى Embeddings مخزنة في قاعدة بيانات Vector.
- **الاسترجاع كأداة:** استخدام الـAgent لقاعدة المعرفة كأداة بحث.
- **المعرفة مقابل الذاكرة:** المعرفة الدائمة مقابل سياق المحادثة.
- **الاسترجاع مقابل التوليد:** تحديد مصدر الخطأ عند فشل الإجابة.

### 🛠️ المشاريع

- **مشروع المحاضرة — RAG with n8n:** مسار يحوّل ملفات PDF إلى Embeddings في Supabase، وAgent يجيب بعد البحث فيها.

> لا يوجد مشروع مستقل لهذه الوحدة.

### 📌 الخلاصة

اختر المعمارية بناءً على المشكلة، لا على الأداة.

</div>
