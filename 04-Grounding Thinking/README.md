# U4 — GROUND ITS KNOWLEDGE

## Knowledge Grounding

> An AI system should know where its answers come from.

In this unit, I learned how to build an AI system that relies on a real, specific knowledge source: how it gets the knowledge, how it retrieves it, how it generates answers from it, and how to test and improve its reliability.

**🌐 English** · [العربية](#-النسخة-العربية)

---

## 🎯 Capability

**Knowledge Grounding**

The ability to build a RAG system grounded in trusted sources, separate retrieval problems from generation problems, and say so when the knowledge is not enough.

---

## 🧠 What I Learned

- **Knowledge Sources:** choose sources and review their quality and freshness.
- **Document Processing & Chunking:** prepare documents and split them well.
- **Retrieval:** keyword, semantic, and hybrid search, and Top K.
- **Grounded Answers:** answer only from the knowledge, with the source.
- **Hallucination Control:** say "not enough information" instead of inventing.
- **Evaluation & Improvement:** test, find the cause of failures, improve, and re-test.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** TECHFIX — AI Computer Troubleshooting Assistant

- **Problem:** Common computer problems get generic or unsafe advice.
- **Solution:** An Arabic assistant in Dify grounded in a four-document knowledge base, which asks for details when needed.
- **Tools:** Dify, Knowledge Base, LLM

[Explore the Project](./Course-Project/)

### 02 — Independent Project

**Project:** PolicyBot — Knowledge-Grounded IT Policy Assistant *(in progress)*

- **Problem:** Long IT policies nobody reads, leading to repeated questions and accidental violations.
- **Solution:** A RAG assistant that answers only from two official policies and cites the clause.
- **Tools:** Dify (Chatflow, Knowledge, Retrieval Testing), Gemini, OpenAI Embeddings

[Explore the Project](./Independent-Project/)

---

## 🔍 Engineering Focus

1. Is the knowledge source trusted, current, and clean?
2. Was the right information retrieved?
3. Was the answer built only from what was retrieved?
4. What happens when the knowledge does not contain the answer?
5. Is a failure caused by retrieval or by generation?

---

## 📌 Key Takeaway

**If you can't explain where an answer came from, you can't trust it.**

---

## 🔗 Related Projects

- [Guided Project — TECHFIX](./Course-Project/)
- [Independent Project — PolicyBot](./Independent-Project/)

---

**Next:** [U5 — CONTROL ITS AGENCY](../05-Agent%20%26%20Decision%20Thinking/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U4 — أسّس معرفته

> يجب أن يعرف نظام الذكاء الاصطناعي من أين تأتي إجاباته.

في هذه الوحدة، تعلمت كيف أبني نظام ذكاء اصطناعي يعتمد على مصدر معرفة حقيقي ومحدد: كيف يحصل على المعرفة، وكيف يسترجعها، وكيف يولّد الإجابة منها، وكيف أختبر موثوقيته وأحسّنه.

### 🧠 ماذا تعلمت

- **مصادر المعرفة:** اختيار المصادر وتقييم جودتها وحداثتها.
- **تجهيز الوثائق والتقسيم:** تجهيز الوثائق وتقسيمها بشكل مناسب.
- **الاسترجاع:** البحث بالكلمات وبالمعنى والبحث الهجين، وTop K.
- **الإجابات المستندة للمعرفة:** الإجابة من المعرفة فقط مع ذكر المصدر.
- **التحكم بالهلوسة:** التصريح بعدم توفر المعلومة بدل اختلاقها.
- **التقييم والتحسين:** اختبار، وتحليل سبب الفشل، وتحسين، وإعادة اختبار.

### 🛠️ المشاريع

- **مشروع المحاضرة — TECHFIX:** مساعد بالعربية في Dify يعتمد على قاعدة معرفة من أربعة مستندات، ويسأل عن التفاصيل عند الحاجة.
- **المشروع المستقل — PolicyBot** *(قيد التنفيذ)*: مساعد RAG يجيب من سياستين رسميتين فقط مع ذكر البند.

### 📌 الخلاصة

إذا لم تستطع تفسير مصدر الإجابة، فلا يمكنك الوثوق بها.

</div>
