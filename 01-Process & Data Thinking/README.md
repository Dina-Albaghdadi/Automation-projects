# U1 — UNDERSTAND THE WORK

## Process & Data Thinking

> Before automating the work, understand the work.

In this unit, I learned how to turn a real-world problem into a clear process that can be analyzed, and how to design a suitable AI Automation solution for it.

The goal is not to start with the tool. It is to understand the work first: define the problem, analyze the data, and design the decisions the system needs to make.

---

## 🎯 Capability

**Process & Data Thinking**

The ability to analyze work, decompose processes, understand inputs, and design decision logic before implementing automation.

---

## 🧠 What I Learned

During this unit, I developed my ability to:

- **Understand the Work:** understand the problem, the business context, and the required outcome.
- **Decompose the Work:** break a process down into a workflow, tasks, and subtasks.
- **Understand the Input:** identify the required data and understand structured vs. unstructured data.
- **Design the Decision:** decide when to use AI and when clear rules are enough.
- **Handle Exceptions Safely:** handle unexpected cases and decide when Human Review is needed.
- **Design the Automation:** turn the process design into an executable workflow.

---

## 🛠️ Projects

### 01 — Guided Project

**Project:** AI Client Intake & Request Routing System

A hands-on project where I applied work analysis and designed an AI-powered automation process.

- **Problem:** A services company receives client requests through an online form. Each client writes differently and often leaves out important details, so an employee has to read, organize, and check every request manually before deciding the next step.
- **Solution:** A Zapier automation that uses AI to extract the service type, timeline, and budget from each request, returns a clear `request_status`, and routes complete requests to the web team and incomplete ones to human review — recording every request in Google Sheets and sending the matching email.
- **Tools:** Google Forms, Zapier, AI by Zapier, Paths by Zapier, Google Sheets, Gmail

[Explore the Project](./Course-Project/)

### 02 — Independent Project

**Project:** IT Support Requests — AI Intake & Routing

An independent project where I applied what I learned to a different problem: designing the solution, building it, testing it, and documenting the results.

- **Problem:** Employees report IT problems in free text. Some requests are clear, while others are vague, miss the affected device, contradict themselves, or hide a security incident — and sorting them manually is slow and risky.
- **Solution:** A Zapier automation that uses AI to classify each request (in Arabic or English), extract the affected item, flag security concerns, and list missing information, then routes it to the IT support queue or to human review. It updates the same sheet row and sends the right email. Eight test cases were run end to end, including an AI prompt fix after a failed test.
- **Tools:** Google Forms, Zapier, AI by Zapier, Paths by Zapier, Google Sheets, Gmail

[Explore the Project](./Independent-Project/)

---

## 🔍 Engineering Focus

In this unit, I focused on these questions:

1. What is the real problem we want to solve?
2. How is the process done today?
3. What inputs and data does the system need?
4. Where do we need AI, and where are clear rules enough?
5. What happens when information is missing or unclear?
6. How do we turn the design into an automation workflow?

---

## 📌 Key Takeaway

**A good automation starts with a well-understood process, not with a tool.**

Successful automation begins with understanding the work and designing the process — not just with choosing a tool.

---

## 📈 Capability Progress

- [x] Understand the problem and the required outcome.
- [x] Break the work down into clear steps.
- [x] Define the inputs and outputs.
- [x] Design AI decisions and rule-based logic.
- [x] Identify the cases that need Human Review.
- [x] Implement the solution and document the results.

---

## 🔗 Related Projects

- [Guided Project — AI Client Intake & Request Routing System](./Course-Project/)
- [Independent Project — IT Support Requests](./Independent-Project/)

---

**Next:** [U2 — BUILD THE SYSTEM](../02-System%20Thinking/)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### U1 — افهم العمل

> قبل أتمتة العمل، افهم العمل.

في هذه الوحدة، تعلمت كيف أحوّل مشكلة حقيقية إلى عملية واضحة قابلة للتحليل، وكيف أصمم لها حل أتمتة بالذكاء الاصطناعي. الهدف ليس البدء بالأداة، بل فهم العمل أولًا: تحديد المشكلة، وتحليل البيانات، وتصميم القرارات التي يحتاجها النظام.

### 🧠 ماذا تعلمت

- **فهم العمل:** فهم المشكلة وسياقها والنتيجة المطلوبة.
- **تفكيك العمل:** تقسيم العملية إلى Workflow ومهام ومهام فرعية.
- **فهم المدخلات:** تحديد البيانات المطلوبة والتمييز بين البيانات المنظمة وغير المنظمة.
- **تصميم القرار:** متى نستخدم الـAI ومتى تكفي القواعد الواضحة.
- **التعامل الآمن مع الاستثناءات:** معرفة متى نحتاج مراجعة بشرية.
- **تصميم الأتمتة:** تحويل تصميم العملية إلى Workflow قابل للتنفيذ.

### 🛠️ المشاريع

- **مشروع المحاضرة — AI Client Intake & Request Routing:** أتمتة بـZapier تستخرج نوع الخدمة والمدة والميزانية من طلب العميل، وتوجّه الطلب المكتمل لفريق المواقع والناقص لمراجعة بشرية.
- **المشروع المستقل — IT Support Requests:** أتمتة تصنّف طلبات الدعم الفني بالعربية والإنجليزية، وتكتشف المخاوف الأمنية، وتوجّه الطلب لفريق الدعم أو لمراجعة بشرية، مع 8 اختبارات.

### 📌 الخلاصة

الأتمتة الجيدة تبدأ بعملية مفهومة جيدًا، وليس بأداة.

</div>
