# U4 — GROUND ITS KNOWLEDGE

## Knowledge Grounding

> An AI system should know where its answers come from.

في هذه الوحدة، أتعلم كيف أبني نظام ذكاء اصطناعي يعتمد على مصدر معرفة حقيقي ومحدد: كيف يحصل على المعرفة، وكيف يسترجعها، وكيف يولّد الإجابة منها، وكيف أختبر موثوقيته وأحسّنه.

---

## 🎯 Capability

**Knowledge Grounding**

القدرة على بناء نظام RAG يستند إلى مصادر موثوقة، ويميّز بين مشكلات الاسترجاع ومشكلات التوليد، ويصرّح عندما لا تكفي المعرفة.

---

## 🧠 What I Learned

- **Knowledge Sources:** اختيار المصادر وتقييم جودتها وحداثتها.
- **Document Processing & Chunking:** تجهيز الوثائق وتقسيمها.
- **Retrieval:** البحث بالكلمات، والبحث بالمعنى، والبحث الهجين، وTop K.
- **Grounded Answers:** الإجابة من المعرفة فقط مع ذكر المصدر.
- **Hallucination Control:** التصريح بعدم توفر المعلومة بدل اختلاقها.
- **Evaluation & Improvement:** اختبار، وتحليل سبب الفشل، وتحسين، وإعادة اختبار.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** TECHFIX — AI Computer Troubleshooting Assistant

- **Problem:** صعوبة التعامل مع مشكلات الحاسوب والإرشادات العامة أو غير الآمنة.
- **Solution:** مساعد بالعربية في Dify يعتمد على قاعدة معرفة من أربعة مستندات.
- **Tools:** Dify، Knowledge Base، LLM

[Explore the Project](./Course-Project/)

### 02 — Independent Project

**Project:** PolicyBot — Knowledge-Grounded IT Policy Assistant

- **Problem:** سياسات IT طويلة لا يقرؤها أحد، فتتكرر الأسئلة والمخالفات.
- **Solution:** مساعد RAG يجيب من سياستين رسميتين فقط مع ذكر البند.
- **Tools:** Dify (Chatflow، Knowledge، Retrieval Testing)، Gemini، OpenAI Embeddings

[Explore the Project](./Independent-Project/)

---

## 📌 Key Takeaway

**If you can't explain where an answer came from, you can't trust it.**

---

## 🔗 Related Projects

- [Guided Project](./Course-Project/)
- [Independent Project](./Independent-Project/)
