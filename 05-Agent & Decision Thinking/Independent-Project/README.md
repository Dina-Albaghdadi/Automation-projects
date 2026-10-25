# My Daily Progress — Telegram AI Agent (n8n)

> A Telegram bot where I report what I did during the day. An AI agent matches each message to my daily routine, confirms it with me, logs it to Google Sheets, and sends an end-of-day report of what was done and what was missed.

**🌐 English** · [العربية](#-النسخة-العربية)

> **Status: in progress.** The design is complete and the Telegram bot is created; the n8n workflows are being built.

---

## 🎯 Problem

### The Context

I follow a fixed daily routine (wake-up time, five prayers, work days, laptop work, sleep time).

### The Problem

At the end of the day it is hard to see honestly what I did and what I skipped. Writing it down manually is easy to forget.

### Why It Matters

A quick message is easy to send; an automatic report turns those messages into a clear picture of the day.

---

## 💡 Solution

1. I message the bot in Arabic (Levantine dialect): *"I woke up at 8 and prayed Fajr and Dhuhr."*
2. The agent extracts the tasks, matches them to the routine, and shows a short summary.
3. After I confirm, it logs them to Google Sheets.
4. Every evening a separate workflow compares the plan with the log and sends a report.

**Why Telegram instead of WhatsApp:** the course project used WhatsApp; I do not use WhatsApp, so I built the same agent pattern on Telegram.

---

## 👤 Users

**Primary Users:**  
Me (a single user).

**User Need:**  
Log my day in seconds and see what was done and missed.

---

## 🧠 How I Designed the Solution

**Why an agent:** messages are free text, can contain several tasks, and can be unclear (*"I prayed"* — which prayer?). The agent must decide what to ask and what to log.

**Boundaries:**
- Only tasks from the routine are allowed.
- Ask when unclear; never guess.
- Flag a task not scheduled today (Women's Affairs is only Saturday, Monday, Wednesday).
- Log only after explicit confirmation, and never claim success unless the tool succeeded.
- The daily report is calculated in code, not by the model.

---

## ⚙️ How It Works

```text
1. Chat workflow
Telegram Trigger → AI Agent → Telegram Send Message
                    ├─ Chat model
                    ├─ Simple Memory (session = chat id)
                    ├─ Google Sheets: Log Task (append or update by Key)
                    └─ Google Sheets: Read Log (today)

2. End-of-day report
Schedule → Google Sheets (today) → Code: plan vs logged → Telegram message
```

---

## 🗂️ Data

### Input

Telegram messages about my day.

### Output

Rows in the **Log** sheet: `Key | Date | Day | Task | Status | Time | Notes` (`Key = Date | Task` prevents duplicates), plus the evening report.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Telegram (BotFather) | Interface |
| n8n | AI Agent, Simple Memory, Schedule, Code |
| Google Sheets | Daily log |

---

## 🧪 Testing (planned)

Several tasks in one message · unclear prayer · task not scheduled today · task outside the routine · correction before saving · no confirmation → nothing saved · confirmation → rows saved · tool failure → "not saved" · duplicate confirmation → one row · memory · end-of-day report.

---

## 🔗 Project Links

**Live Demo:** Telegram bot (private)

**Workflow:** n8n (in progress)

**Repository:** [My-Daily-Progress-Telegram-bot](https://github.com/Dina-Albaghdadi/My-Daily-Progress-Telegram-bot)

---

## 👩‍💻 Author

**Dina Albaghdadi**

[LinkedIn](https://www.linkedin.com/in/dina-albaghdadi-79bb9a25a/) · [GitHub](https://github.com/Dina-Albaghdadi)

---
---

<div dir="rtl">

## 🌍 النسخة العربية

> بوت تيليجرام أخبره بما أنجزته خلال اليوم، فيطابق الـAI Agent كل رسالة مع روتيني، ويؤكدها معي، ويسجلها في Google Sheets، ويرسل في نهاية اليوم تقريرًا بما أُنجز وما لم يُنجز.

> **الحالة: قيد التنفيذ.** التصميم مكتمل والبوت مُنشأ، ويجري بناء الـWorkflows.

### 🧠 لماذا Agent وما حدوده

الرسائل نص حر وقد تحتوي عدة مهام أو تكون غير واضحة، فيحتاج الـAgent أن يقرر ماذا يسأل وماذا يسجل. حدوده: مهام الروتين فقط، والسؤال عند الغموض، والتنبيه للمهمة غير المجدولة اليوم، والتسجيل بعد التأكيد فقط، والتقرير اليومي يُحسب بالكود.

**لماذا تيليجرام وليس واتساب:** مشروع المحاضرة استخدم واتساب، وأنا لا أستخدمه، فبنيت نفس النمط على تيليجرام.

</div>
