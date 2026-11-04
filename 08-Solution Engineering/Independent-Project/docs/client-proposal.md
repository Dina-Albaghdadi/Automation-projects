# 💼 Client Proposal — Athar | أثر

**🌐 English** · [العربية](#-النسخة-العربية)

> The client in this proposal is fictional (Amal Center), like all data in the project. The hours are the real time spent building Athar.

---

## 1. Project Information

| Item | Details |
|---|---|
| **Project Name** | Athar: Telegram activity intake and donor report automation |
| **Client** | Amal Center (fictional women's organization) |
| **Prepared By** | Dina Albaghdadi |
| **Proposal Date** | 4 November 2026 |
| **Version** | 1.0 |
| **Proposal Validity** | 30 days |

---

## 2. Executive Summary

Amal Center's field staff record activities in chat messages and on paper, and a staff member rebuilds them by hand into a donor report every month. The numbers do not always match, and each donor asks for a different format. Athar lets staff record each activity with one Telegram message, then prepares a monthly draft report: code calculates the figures, the donor's requirements are retrieved from documents, and AI writes the narrative. The manager reviews and approves every report before it is final.

---

## 3. Understanding the Client's Problem

### Current Situation

Activities are reported in chat messages, attendance is taken on paper, and photos are kept on phones. At the end of the month, one person collects everything, counts attendance, and writes the report in the donor's format.

### Main Challenges

1. Activity data is scattered and incomplete.
2. Figures disagree between messages and attendance sheets.
3. Each donor has its own requirements and structure.

### Business Objectives

The project aims to:

1. Record every activity from one message, with no missing required fields.
2. Keep report figures exactly as the records show.
3. Give the manager a reviewable draft in the donor's format, with warnings about data that needs checking.

---

## 4. Proposed Solution

### Solution Overview

Staff describe an activity on Telegram in Arabic or English. Athar asks for anything missing and saves the activity only after the staff member confirms it. When the manager sends `/report`, Athar prepares a branded draft and emails it to her. She approves it, or requests changes with a note that the next version applies.

### Workflow

**Trigger → Processing → Decision → Action → Result**

Telegram message → AI extracts the fields → rules check missing data, dates, duplicates, and privacy → the staff member confirms → the activity is saved. Then: `/report` → code calculates the figures → RAG retrieves donor guidance → AI writes → a validator checks the numbers → a Google Doc and an approval email are sent → the manager approves or requests changes.

### Solution Type

- [ ] Workflow
- [x] AI-Powered Workflow
- [ ] AI Assistant
- [ ] AI Agent
- [x] RAG (Retrieval-Augmented Generation)

### Architecture Rationale

The steps are known in advance, so a fixed workflow is safer than an agent: AI never decides what to save, send, or approve. A rules-only workflow is not enough, because messages are free text and the report needs a written narrative. RAG is used because each donor has its own requirements, and the narrative should follow them with sources.

### Tools and Integrations

| Tool or Component | Purpose in the Solution |
|---|---|
| Telegram Bot | Entry point for staff and the manager, buttons, and notifications |
| n8n | Two workflows: intake and report |
| Google Gemini | Field extraction, embeddings for retrieval, and report writing, with a fallback model |
| Google Sheets | Source of truth for staff, activities, attendance, reports, and knowledge |
| Google Apps Script | Attendance cleaning, branded Google Docs, and the approval page |
| Gmail | HTML approval email to the manager |

### AI's Role

AI reads each message and returns the activity fields, and writes the report narrative. AI does not calculate figures, save data, or approve anything.

### Human Review

Staff confirm every activity before it is saved. The manager approves every report before it is final, and can request changes with a note.

---

## 5. Scope of Work

| # | Task | Description |
|---|---|---|
| 1 | Discovery and design | Confirm the process, users, required fields, and donor requirements. Design the data and architecture |
| 2 | Google Sheet and Apps Script | Tabs, attendance cleaning, branded document template, and the approval web app |
| 3 | Intake workflow | Telegram intake in Arabic and English, missing-field questions, confirmation, duplicate, date, and privacy checks |
| 4 | Report workflow | Figures in code, RAG over donor guidance, AI narrative, validation, draft, and approval email |
| 5 | Testing | Normal, missing data, invalid input, duplicates, permissions, and service failures |
| 6 | Documentation and handover | Setup guide, user guide, test report, and a one-hour handover session |

---

## 6. Deliverables

- [x] n8n workflows: Athar - Intake and Athar - Report (JSON export).
- [x] Apps Script code and approval page.
- [x] Configured Google Sheet with sample data.
- [x] Testing documentation (22 live tests and 12 unit tests).
- [x] Technical documentation and setup guide (repository README).
- [x] Deployment plan (section 10).
- [x] Handover session (one hour).

---

## 7. Out of Scope

- Sending reports to donors automatically.
- Storing participants' names or ID numbers.
- Financial reports.
- Integration with an existing MIS.
- Photo or voice-note intake.
- Ongoing maintenance after the support period (available as a separate monthly option).

Any new request outside this scope is estimated separately and starts only after the client approves its cost and timeline.

---

## 8. Project Timeline

**Estimated Duration:** 5 working days (40 hours)

| Phase | Activities | Estimated Duration |
|---|---|---|
| M1 — Discovery & Design | Confirm requirements and design the solution | 1 day (8 hours) |
| M2 — Development | Sheet, Apps Script, and both n8n workflows | 2 days (16 hours) |
| M3 — Testing | Test all cases and fix issues | 1 day (8 hours) |
| M4 — Deployment & Handover | Deploy, document, and hand over | 1 day (8 hours) |

### Timeline Dependencies

The client provides, before M2: a Google account for the organization, the manager's email, the staff list with Telegram accounts, the project list, and the donor's reporting guidelines. Delays in access or approvals move the delivery date by the same time.

---

## 9. Investment and Payment Terms

**Pricing Model:** Fixed price, based on 40 hours at $15 per hour.

**Total Project Fee:** **$600 (USD)**

### Payment Schedule

| Milestone | Payment Amount |
|---|---:|
| Proposal approved (start of M1) | $240 (40%) |
| Testing complete (end of M3) | $240 (40%) |
| Handover complete (end of M4) | $120 (20%) |
| **Total** | **$600** |

### Additional Costs

The project fee **does not include** the running costs in section 10. These are paid by the client directly to each provider:

- n8n Cloud subscription.
- Gemini API usage, if the paid tier is used.
- Maintenance after the support period.

---

## 10. Hosting & Deployment

### Deployment Environment

- [x] n8n Cloud
- [ ] Self-Hosted

### Deployment Approach

The workflows run on n8n Cloud. Data and documents stay in the client's Google account (Sheets, Drive, Docs, Gmail). The approval page is an Apps Script web app deployed from the sheet. No server, domain, or VPS is needed.

### Hosting Provider

n8n Cloud and Google Workspace.

### Hosting Responsibility

Client. The accounts are created in the client's name, and the developer is given temporary access during the project.

### Operational Costs

| Item | Responsible Party | Estimated Cost |
|---|---|---:|
| Hosting: n8n Cloud Starter (2,500 executions per month) | Client | about €24 per month (€20 if billed yearly) |
| AI API usage: Gemini | Client | $0 on the free tier. Under $5 per month on the paid tier for a small team (estimate) |
| External services: Telegram, Google Sheets, Docs, Gmail | Client | $0 with the organization's existing Google account |
| Domain / other services | — | Not needed |

**Recommendation:** use the paid Gemini tier when real data is entered, because free-tier requests may be used to improve Google's products.

The Gemini cost is an estimate for about 200 activities a month. It should be checked against the first month's usage.

### Maintenance & Updates

Bug fixes within the agreed scope are included for 30 days after handover. After that, maintenance is available as an optional monthly plan: up to 3 hours a month for $45, covering monitoring, small fixes, and updates when a service changes.

---

## 11. Assumptions and Client Responsibilities

### Project Assumptions

- One donor format and up to five projects in the first version.
- Up to 20 staff members, with one manager role.
- Attendance is recorded with participant codes, not names.
- The organization already uses Google accounts.

### Client Responsibilities

- Google account, manager email, and Telegram accounts for staff.
- The donor's reporting guidelines and a sample of a previous report.
- Sample activities and attendance for testing.
- Feedback and approvals within two working days.
- One contact person for project decisions.

---

## 12. Testing and Acceptance Criteria

| Test Scenario | Expected Result |
|---|---|
| A staff member sends an activity without a duration | The bot asks only for the duration |
| The staff member presses Confirm | The activity is saved once, with an ID |
| A message contains an ID or phone number | The message is blocked before AI, and nothing is saved |
| The same activity is sent again | It is rejected as a duplicate |
| An unregistered user, or a staff member, sends `/report` | The request is refused |
| The manager sends `/report <project> <month>` | Draft figures match the Activities tab exactly |
| The manager requests changes with a note | The next version applies the note |
| Gemini does not respond | The workflow stops within its time limit, and the user gets a clear message. Nothing is saved |

### Acceptance Criteria

The solution is accepted when all the scenarios above pass on the client's own accounts, and the deliverables in section 6 have been provided.

### Acceptance Process

- **Review Period:** 5 working days after handover.
- **Reporting Method:** a shared list of issues, each with the message sent, the expected result, and a screenshot.
- **Defect Resolution:** issues that break an agreed scenario are fixed at no cost within the 30-day support period.

---

## 13. Handover and Support

### Handover Deliverables

- [x] Workflow export files.
- [x] Technical documentation.
- [x] Setup and operating instructions.
- [x] Troubleshooting notes for common issues.
- [x] One-hour handover session for the manager and one staff member.
- [x] Transfer of access to the client's accounts.

### Ownership and Access

All files, workflows, and data belong to the client. Credentials are created in the client's accounts and stored in n8n credentials and Apps Script properties, never in documents or the repository. The developer's temporary access is removed after handover.

### Post-Delivery Support

- **Support Period:** 30 days.
- **Included Support:** fixing defects in the agreed scope, and answering questions about setup and use.
- **Excluded Support:** new features, new donors or templates, and changes caused by the client editing the workflows. These are estimated separately.

---

## 14. Risks and Limitations

| Risk or Limitation | Potential Impact | Mitigation Approach |
|---|---|---|
| Gemini free tier is slow or unavailable | Delayed replies or reports | Retries, a fallback model, time limits, and the paid tier for production |
| Approval page opens with a link | Anyone with the link could open it | One-time token per report. Google sign-in is recommended as the next step |
| Privacy check uses patterns | Some personal data could be missed | Staff guidance, participant codes, and the manager's review |
| Time saved is not yet measured | Benefit is not proven in numbers | Measure the time spent on one report before and after a one-month pilot |

---

## 15. Next Steps

1. The client reviews the proposal.
2. Both parties agree on the scope and acceptance criteria.
3. The first payment is made.
4. The client provides the accounts, staff list, and donor guidelines.
5. The kickoff date is confirmed.

---
---

<div dir="rtl">

## 🌍 النسخة العربية

### ملخص عرض السعر

- **العميل:** مركز أمل (مؤسسة تجريبية).
- **المدة:** 5 أيام عمل (40 ساعة)، من السبت إلى الأربعاء من 9 إلى 5.
- **السعر:** سعر ثابت **600 دولار** (40 ساعة × 15 دولارًا).
- **الدفعات:** 40٪ عند الموافقة، و40٪ بعد الاختبارات، و20٪ عند التسليم.
- **تكاليف التشغيل الشهرية على العميل:** n8n Cloud حوالي 24 يورو، وGemini مجاني أو أقل من 5 دولارات بالخطة المدفوعة (تقدير)، وGoogle وتيليجرام بدون تكلفة.
- **الدعم:** 30 يومًا لإصلاح الأعطال ضمن النطاق، ثم صيانة اختيارية بـ 45 دولارًا شهريًا (حتى 3 ساعات).
- **خارج النطاق:** إرسال التقارير للممول تلقائيًا، وتخزين أسماء المستفيدات، والتقارير المالية، والربط بنظام MIS.

</div>
