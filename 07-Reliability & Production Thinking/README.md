# U7 — MAKE IT RELIABLE

## Reliability & Production Thinking

> Understand the problem. Choose the architecture. Build. Test. Improve.

في هذه الوحدة، أنتقل من **بناء** حلول الذكاء الاصطناعي إلى **هندستها**: تحليل المشكلة قبل اختيار الأدوات، واختيار المعمارية المناسبة، واختبار سلوك النظام في الحالات الطبيعية وغير الطبيعية، وتشخيص الأعطال ومعالجتها.

---

## 🎯 Capability

**Reliability & Production Thinking**

القدرة على اختيار المعمارية المناسبة للمشكلة (Workflow، أو AI Workflow، أو RAG، أو AI Agent)، وبناء حل يمكن الاعتماد عليه وفهم مصادر فشله.

---

## 🧠 What I Learned

- **Architecture Choice:** متى أحتاج Workflow، ومتى RAG، ومتى AI Agent.
- **Knowledge Pipelines:** تحويل المستندات إلى Embeddings مخزنة في Vector Database.
- **Retrieval as a Tool:** استخدام الـAgent لقاعدة المعرفة كأداة بحث.
- **Knowledge vs. Memory:** الفرق بين المعرفة الدائمة وسياق المحادثة.
- **Retrieval vs. Generation:** تحديد مصدر الخطأ عند فشل الإجابة.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** RAG with n8n — Knowledge Pipeline & RAG AI Agent

- **Problem:** في Dify كانت خطوات الـRAG مخفية، فيصعب فهمها أو إعادة استخدامها.
- **Solution:** مسار يحوّل ملفات PDF إلى Embeddings في Supabase، وAgent يجيب بعد البحث فيها.
- **Tools:** n8n، Google Drive، Supabase (pgvector)، OpenAI Embeddings، Google Gemini

[Explore the Project](./Course-Project/)

> لا يوجد مشروع مستقل (Independent Project) لهذه الوحدة.

---

## 📌 Key Takeaway

**Choose the architecture from the problem, not from the tool.**

---

## 🔗 Related Projects

- [Guided Project](./Course-Project/)
