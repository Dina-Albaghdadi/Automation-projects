# TECHFIX — AI Computer Troubleshooting Assistant (Dify)

> An Arabic assistant built in Dify that helps users troubleshoot common computer problems using a dedicated knowledge base, asks for details when the description is not enough, and says so when the knowledge does not cover the problem.

**🌐 English** · [العربية](#-النسخة-العربية)

> 📸 **Evidence pending:** screenshots of the Dify knowledge settings, retrieval testing, and test conversations will be added.

---

## 🎯 Problem

### The Context

Many users struggle with computer problems when the symptoms are unclear or the available technical instructions are too complex for their situation.

### The Problem

Common issues — a slow computer, full storage, Wi-Fi or internet problems — get generic answers that do not match the user's actual case, and some "fixes" found online can cause data loss.

### Why It Matters

Clear, safe, first-step guidance in Arabic helps users understand the problem and act correctly, and tells them when they need a technician.

---

## 💡 Solution

A Dify chatbot connected to a knowledge base of four Arabic documents:

| Document | Scope |
|---|---|
| Slow computer | Performance problems and likely causes |
| Storage space | Why storage fills up and how to handle it |
| Internet connection | Network and internet troubleshooting |
| Safety and limits | Safe actions, warnings, and when to call a specialist |

What does the system do?

1. The user describes a problem in Arabic.
2. Dify retrieves the relevant parts of the knowledge base.
3. The model answers using only the retrieved knowledge.
4. If the description is not enough, it asks for details; if the knowledge does not cover it, it says so.

---

## 👤 Users

**Primary Users:**  
Arabic-speaking users with common computer problems.

**User Need:**  
Understand the problem and get safe first steps, in clear Arabic.

---

## 🧠 How I Designed the Solution

The knowledge base defines what the assistant can help with. The instructions keep the model inside that boundary:

- Do not invent information or steps that the knowledge does not support.
- Do not confirm a cause without enough information.
- Ask for more details when needed.
- Avoid actions that could lose data or damage the system, and warn before sensitive steps.

---

## ⚙️ How It Works

```text
User describes the problem (Arabic)
  ↓
Dify Knowledge Retrieval (4 documents)
  ↓
LLM + instructions (grounded, safe, ask if unclear)
  ↓
Answer, a clarifying question, or "not enough information"
```

---

## 🤖 Role of AI

What does AI do in this solution?

- Matches the user's description to the relevant troubleshooting knowledge.
- Writes clear, ordered first steps in Arabic, with warnings where needed.

### What AI Should NOT Do

- Diagnose a fault with certainty — it cannot inspect the device.
- Suggest steps that are not in the knowledge base.
- Recommend deleting system files, disabling protection, or installing unknown software.

---

## 🔄 Automation

What parts of the process are automated?

- Retrieving relevant knowledge for every question.
- Producing grounded first-step guidance and safety warnings.

---

## 🗂️ Data

### Input

The user's description of the problem.

### Output

Troubleshooting guidance, a clarifying question, or a clear statement that the knowledge does not cover the case.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Dify | Knowledge base, retrieval, and chatbot app |
| Knowledge base (4 Arabic documents) | The only source of answers |
| LLM | Answer generation |

---

## 🔑 Key Design Decisions

### Decision 1

**Decision:**  
A dedicated safety-and-limits document inside the knowledge base.

**Why:**  
Safety rules are retrieved like any other knowledge, so warnings appear next to the steps they apply to.

### Decision 2

**Decision:**  
Ask for details instead of guessing.

**Why:**  
"My computer is slow" can have many causes; a wrong guess wastes the user's time or leads to unsafe actions.

---

## 🧪 Testing

> Test results and screenshots will be added.

---

## 📸 Evidence

> Screenshots will be added.

---

## ⚠️ Challenges & Failures

> Will be documented with the test results.

---

## 📚 What I Learned

### Technical Lesson

How a Dify knowledge base, retrieval, and instructions work together to keep answers grounded.

### Problem-Solving Lesson

The scope of the knowledge base is the scope of the assistant; anything outside it must be handled honestly.

### AI Lesson

A helpful assistant knows when to ask, when to answer, and when to say it does not know.

---

## 🔮 Future Improvements

- Add more troubleshooting topics (printers, updates, accounts).
- Build a test set of clear, unclear, and out-of-scope questions.

---

## 🔗 Project Links

**Live Demo:** Dify web app — TechFix

**Workflow:** —

**Repository:** —

---

## 👩‍💻 Author

**Dina Albaghdadi**

[LinkedIn](https://www.linkedin.com/in/dina-albaghdadi-79bb9a25a/) · [GitHub](https://github.com/Dina-Albaghdadi)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

> مساعد بالعربية مبني على Dify يساعد المستخدمين على استكشاف مشكلات الحاسوب الشائعة اعتمادًا على قاعدة معرفة متخصصة، ويطلب تفاصيل عند الحاجة، ويصرّح عندما لا تغطي المعرفة المشكلة.

### 🎯 المشكلة

صعوبة التعامل مع مشكلات الحاسوب عندما تكون الأعراض غير واضحة، والإرشادات المتاحة عامة أو معقدة، وبعض الحلول المنتشرة قد تسبب فقدان البيانات.

### 💡 الحل

Chatbot في Dify مرتبط بأربعة مستندات: بطء الحاسوب، ومساحة التخزين، واتصال الإنترنت، والسلامة وحدود المساعدة. يسترجع المعرفة المناسبة ويجيب منها فقط، ويسأل عند نقص التفاصيل، ويصرّح عند عدم كفاية المعرفة.

### 🔑 قرارات التصميم

1. **مستند خاص بالسلامة وحدود المساعدة** حتى تظهر التحذيرات بجانب الخطوات المرتبطة بها.
2. **السؤال بدل التخمين** عندما يكون الوصف غير كافٍ.

### 🧪 الاختبارات

ستُضاف نتائج الاختبارات والصور.

</div>
