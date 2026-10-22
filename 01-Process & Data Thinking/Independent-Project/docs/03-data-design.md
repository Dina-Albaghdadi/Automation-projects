# 03 — Data Design

## IT Support Requests

> يحدد هذا الملف شكل البيانات التي تدخل النظام، والمعلومات التي يستخرجها AI، والنتائج التي تُحفظ بعد المعالجة.
> أسماء الحقول هنا هي المرجع الوحيد، ويجب أن تطابقها كل الملفات الأخرى وإعدادات Zapier حرفيًا.

---

## 0. مصدر كل حقل

| الفئة | المعنى |
|---|---|
| **A — مطلوب من المشروع** | الواجب يطلب توثيق Trigger وInputs وUnstructured Data وStructured Data وSchema وMissing Information وValidation. **لا يفرض الواجب أسماء حقول محددة.** المطلوب منه هو وجود هذه العناصر المفاهيمية فقط |
| **B — مصمم لحالة الدعم الفني** | **DECISION**: حقول اخترتها لتناسب طلبات الدعم الفني |
| **C — قرارات مؤثرة على التوجيه** | قرارات تغيّر نتيجة التوجيه مباشرة، ومذكورة في القسم 9 |

---

## 1. Trigger — الحدث الذي يبدأ الأتمتة

| البند | القيمة |
|---|---|
| الحدث | **صف جديد** يُضاف إلى ورقة الردود بعد إرسال النموذج |
| التطبيق في Zapier | Google Sheets → **New Spreadsheet Row** |
| الملف | `IT-Support-Requests (الردود)` |
| الورقة (Worksheet) | `ردود النموذج 1` |

### لماذا Google Sheets وليس Google Forms كـTrigger؟

لأننا نريد **تحديث نفس الصف** لاحقًا، وهذا يتطلب أن يعرف Zapier رقم الصف (Row ID). الـTrigger من Sheets يوفّره مباشرة.

### تحذير: عدم استخدام "New or Updated Spreadsheet Row"

الـZap نفسه سيحدّث الصف بعد المعالجة. لو كان الـTrigger يلتقط **التحديثات** أيضًا، سيعيد تشغيل نفسه على نفس الصف في **حلقة لا تنتهي (Loop)**.
لذلك نستخدم **New Spreadsheet Row فقط**.

> **Trigger ≠ Input:** الـTrigger هو *الحدث* (صف جديد). الـInput هو *البيانات* التي جاءت معه (القسم 2).

---

## 2. Input Data — البيانات الداخلة (FACT: موجودة فعليًا في النموذج والشيت)

| # | عنوان العمود في الشيت | الاسم الداخلي | الغرض | النوع | مطلوب؟ | إذا كان فارغًا أو غير صالح |
|---|---|---|---|---|---|---|
| I1 | `طابع زمني` | `submitted_at` | وقت إرسال الطلب | Date/Time (يُنشئه Google تلقائيًا) | تلقائي | لا يمكن أن يكون فارغًا |
| I2 | `Requester Name` | `requester_name` | اسم الموظف صاحب الطلب | Text | ✅ | النموذج يمنع الإرسال. وإن وصل فارغًا لأي سبب → `NEEDS_REVIEW` (القاعدة V1) |
| I3 | `Requester Email` | `requester_email` | بريد التواصل مع الموظف | Text (Email) | ✅ | النموذج يمنع الإرسال إذا كان فارغًا أو بصيغة غير صحيحة (Response Validation: Email) |
| I4 | `Department` | `department` | قسم الموظف، للمعلومية فقط | Text | ❌ | يبقى فارغًا. **لا يؤثر على التوجيه** |
| I5 | `Issue Description` | `issue_description` | وصف المشكلة بلغة حرة | Long Text | ✅ | النموذج يمنع الإرسال. إذا كان غير مفهوم → يتعامل معه AI وفق القسم 4 |
| I6 | (من Zapier) | `row_id` | رقم الصف، لتحديث نفس الصف | Number | تلقائي | إذا لم يتوفر لا يمكن التحديث → **Technical Failure** وليس `NEEDS_REVIEW` |

> **الاسم الداخلي** يُستخدم في التوثيق فقط لتسهيل القراءة. في Zapier ستظهر الحقول بعناوين أعمدة الشيت كما هي.

---

## 3. Unstructured Data — البيانات غير المنظمة

الحقل `Issue Description` هو البيانات غير المنظمة الوحيدة. يكتبه الموظف بطريقته وبأي لغة.

**مثال (وهمي):**

> *"The printer on the 2nd floor (HP LaserJet) shows a paper jam error and won't print anything since this morning."*

يحتوي هذا النص على نوع المشكلة (طابعة) والجهاز المتأثر (HP LaserJet، الطابق الثاني)، لكنها ليست في حقول منفصلة يمكن للنظام اتخاذ قرار بناءً عليها.

### ما الذي يُرسل إلى AI؟

| يُرسل | لا يُرسل |
|---|---|
| `Issue Description` فقط | الاسم، والبريد، والقسم |

السبب: AI لا يحتاجها لاستخراج المعلومات، وعدم إرسالها يقلل البيانات الشخصية التي تمر عبره.

---

## 4. Structured Data — البيانات المنظمة (مخرجات AI)

| # | الحقل | الغرض | النوع | القيم المسموحة | القيمة عند الغياب |
|---|---|---|---|---|---|
| S1 | `issue_category` | نوع المشكلة | Text (Enum) | انظر 4.1 | `unknown` |
| S2 | `affected_item` | الجهاز أو النظام أو الخدمة المتأثرة **كما ذُكرت في النص** | Text | نص حر قصير | `Not provided` |
| S3 | `issue_summary` | ملخص قصير بالإنجليزية لما ذكره الموظف فقط | Text | جملة أو جملتان | — (يُكتب دائمًا، حتى لو كان: "Request is unclear") |
| S4 | `security_concern` | هل يذكر النص مؤشرًا أمنيًا؟ | Text (Enum) | `true` / `false` | — (يجب أن يكون إحدى القيمتين) |
| S5 | `missing_information` | المعلومات المطلوبة الناقصة أو الغامضة | Text | انظر 4.3 | `NONE` إذا لا شيء ناقص |

### 4.1 القيم المسموحة لـ `issue_category` (B — مقترح)

| القيمة | تُستخدم عندما يذكر النص... | مثال (وهمي) |
|---|---|---|
| `hardware` | جهازًا ماديًا: حاسوب، شاشة، لوحة مفاتيح، فأرة... **ما عدا الطابعات** | "My monitor stays black" |
| `software` | برنامجًا أو تطبيقًا لا يعمل أو يحتاج تثبيتًا، **ما عدا البريد** | "Excel crashes when I open a file" |
| `network` | الإنترنت، أو Wi-Fi، أو VPN، أو الاتصال بالشبكة | "I can't connect to the office Wi-Fi" |
| `account_access` | تسجيل الدخول، أو كلمة المرور، أو قفل الحساب، أو الصلاحيات | "My account is locked" |
| `email` | البريد الإلكتروني (Outlook أو Gmail...) | "I'm not receiving any emails" |
| `printer` | طابعة أو ماسحًا ضوئيًا | "The printer shows paper jam" |
| `other` | المشكلة مفهومة وتقنية، لكنها لا تنتمي للفئات أعلاه | "Please back up the 'Finance Reports' folder on the shared drive" |
| `unknown` | لا يمكن تحديد نوع المشكلة من النص | "Nothing works, please help" |

**قواعد الأسبقية عند التداخل:**
- الطابعة تُصنّف `printer` وليس `hardware`.
- مشاكل البريد تُصنّف `email` وليس `software`.
- مشكلة كلمة مرور البريد تُصنّف `account_access`، لأن جوهرها هو الدخول.

**الفرق بين `other` و`unknown`:**
- `other`: الطلب **مفهوم** لكنه خارج القائمة. وجود `other` وحده **لا يجعل الطلب غير صالح**.
- `unknown`: الطلب **غير مفهوم**، فيحتاج مراجعة.

### 4.2 تعريف `security_concern` (C)

تكون القيمة `true` إذا ذكر النص صراحة أيًا من الحالات التالية:
- الضغط على رابط مشبوه، أو فتح مرفق مشبوه، أو رسالة احتيال (Phishing).
- مشاركة كلمة مرور أو رمز تحقق مع جهة غير موثوقة.
- اختراق حساب، أو نشاط غير مألوف في الحساب، أو دخول غير مصرح به.
- فيروس، أو برنامج فدية (Ransomware)، أو برنامج ضار (Malware).
- فقدان جهاز يحتوي على بيانات العمل، أو سرقته.
- تسريب بيانات.

في غير ذلك تكون القيمة `false`. ومن الأمثلة المهمة:
- **نسيان كلمة المرور** أو **قفل الحساب** وحدهما **ليسا** شبهة أمنية، والقيمة فيهما `false`.

> AI **لا يشخّص** إن كان هناك اختراق فعلي. هو فقط يرصد هل *ذُكر* مؤشر أمني في النص.
> القرار بشأنه يتخذه الإنسان.

### 4.3 القيم المسموحة لـ `missing_information`

قائمة مفصولة بفواصل من القيم التالية فقط، أو `NONE`:

| القيمة | تُستخدم عندما |
|---|---|
| `issue_category` | لا يمكن تحديد نوع المشكلة |
| `affected_item` | الجهاز أو النظام المتأثر غير مذكور |
| `issue_details` | الوصف غير كافٍ لفهم ما حدث، أو فيه تناقض |
| `NONE` | لا توجد معلومات مطلوبة ناقصة (تُكتب بهذا الشكل حرفيًا وبأحرف كبيرة) |

**مثال:** `affected_item, issue_details`

---

## 5. Processing Results — نتائج المعالجة

| # | الحقل | الغرض | النوع | القيم المسموحة | من يحدده؟ |
|---|---|---|---|---|---|
| R1 | `request_status` | نتيجة التحقق من **اكتمال الطلب**، وليس حالة حل المشكلة | Text (Enum) | `VALID` / `NEEDS_REVIEW` | **AI** يحسبها من القواعد المكتوبة في الـPrompt، وPaths يوجّه بناءً عليها، ثم تُكتب في الشيت كقيمة ثابتة داخل كل مسار |
| R2 | `requires_human` | هل يحتاج الطلب مراجعة بشرية قبل المتابعة؟ | Text (Enum) | `true` / `false` | قيمة ثابتة داخل كل مسار |
| R3 | `routing` | المسار الذي حُدد للطلب | Text (Enum) | `it_support_queue` / `human_review` | قيمة ثابتة داخل كل مسار |
| R4 | `processed_at` | وقت معالجة Zapier للطلب | Date/Time | — | قيمة الوقت من Zapier (`{{zap_meta_human_now}}`) |

**الترابط الإلزامي بين القيم:**

| `request_status` | `requires_human` | `routing` |
|---|---|---|
| `VALID` | `false` | `it_support_queue` |
| `NEEDS_REVIEW` | `true` | `human_review` |

أي تركيبة أخرى تعني خطأً في إعداد الـZap، أي **Technical Failure**.

> **من يحدد `request_status`؟ (تغيير بتاريخ 2026-10-15)**
> التصميم الأول جعل Paths يقرر بستة شروط (V1–V6)، ولا يرجع AI القيمة.
> اعتُمدت بعدها **طريقة المحاضرة**: AI يرجع `request_status` بتطبيق قواعد مكتوبة صراحة في الـPrompt على الحقول S1 وS2 وS4 وS5، وPath A يفحص `request_status = VALID` فقط.
> **المقابل:** القرار يعتمد على التزام AI بالقواعد. لذلك يُفحص اتساق `request_status` مع الحقول الأربعة في كل حالة اختبار، والشيت يأخذ القيمة الثابتة للمسار حتى يبقى متسقًا مع `requires_human` و`routing`.

---

## 6. Data Schema — هيكل الشيت النهائي

الأعمدة A–E موجودة الآن (FACT). أما الأعمدة F–N فهي **أعمدة نتائج المعالجة** وتُضاف في الصف الأول، بالأسماء التالية **حرفيًا**:

| العمود | العنوان | المصدر | يُكتب بواسطة |
|---|---|---|---|
| A | `طابع زمني` | Google Forms | Google Forms |
| B | `Requester Name` | Google Forms | Google Forms |
| C | `Requester Email` | Google Forms | Google Forms |
| D | `Department` | Google Forms | Google Forms |
| E | `Issue Description` | Google Forms | Google Forms |
| F | `issue_category` | AI | Zapier (تحديث) |
| G | `affected_item` | AI | Zapier (تحديث) |
| H | `issue_summary` | AI | Zapier (تحديث) |
| I | `security_concern` | AI | Zapier (تحديث) |
| J | `missing_information` | AI | Zapier (تحديث) |
| K | `request_status` | AI (يحدد المسار)، ويُكتب كثابت المسار | Zapier (تحديث) |
| L | `requires_human` | Rule (ثابت المسار) | Zapier (تحديث) |
| M | `routing` | Rule (ثابت المسار) | Zapier (تحديث) |
| N | `processed_at` | Zapier | Zapier (تحديث) |

**قاعدة السجل:** كل طلب = **صف واحد**. الـZap **يحدّث** الصف ولا ينشئ صفًا جديدًا.

**صف فيه A–E وفارغ في F–N** يعني أن الطلب وصل لكن لم يُعالج، وهذا مؤشر على **Technical Failure** يجب فحصه في Zap History.

### مثال صف بعد المعالجة (بيانات وهمية، القيم متوقعة وليست نتيجة تشغيل)

| العمود | القيمة |
|---|---|
| Requester Name | `Test User 01` |
| Department | `Finance` |
| Issue Description | `The printer on the 2nd floor (HP LaserJet) shows a paper jam error...` |
| issue_category | `printer` |
| affected_item | `HP LaserJet printer, 2nd floor` |
| issue_summary | `Printer on 2nd floor shows paper jam error and cannot print.` |
| security_concern | `false` |
| missing_information | `NONE` |
| request_status | `VALID` |
| requires_human | `false` |
| routing | `it_support_queue` |
| processed_at | *(وقت التشغيل الفعلي)* |

---

## 7. Missing Information — التمييز بين القيم

كل قيمة من القيم التالية لها معنى مختلف، ولا يجوز استبدال إحداها بالأخرى:

| القيمة | الحقل | المعنى |
|---|---|---|
| `Not provided` | `affected_item` | الموظف لم يذكر الجهاز أو النظام |
| `unknown` | `issue_category` | لا يمكن تحديد نوع المشكلة |
| `other` | `issue_category` | المشكلة مفهومة لكنها خارج الفئات، **ولا تعني نقصًا** |
| `NONE` | `missing_information` | لا توجد معلومات مطلوبة ناقصة |
| (فارغ) | `Department` | حقل اختياري لم يُعبّأ، **وليس نقصًا** |
| (فارغ) | F–N | الطلب لم يُعالج، أي **Technical Failure** |

**قاعدة عدم الاختراع:** إذا لم يذكر الموظف معلومة، يكتب AI `Not provided` أو `unknown`، **ولا يخمّن**.
مثال: *"My computer is slow"* تعطي `affected_item = computer`، لأن الحاسوب مذكور. لكن AI لا يضيف ماركة أو موديلًا لم يُذكر.

---

## 8. Validation — قواعد التحقق

### 8.1 تحقق النموذج (قبل دخول النظام)

| # | القاعدة | الحالة |
|---|---|---|
| FV1 | `Requester Name` مطلوب | FACT: مفعّل |
| FV2 | `Requester Email` مطلوب | FACT: مفعّل |
| FV3 | `Requester Email` بصيغة بريد صحيحة | FACT: مفعّل (Response Validation → Text → Email) |
| FV4 | `Issue Description` مطلوب | FACT: مفعّل |

### 8.2 قواعد التحقق (تحدد `VALID`)

يكون الطلب **`VALID`** فقط إذا تحققت **كل** الشروط التالية (AND):

| # | الشرط | أين يُطبق؟ |
|---|---|---|
| V1 | الاسم موجود | النموذج (FV1) |
| V2 | البريد موجود | النموذج (FV2) |
| V3 | نوع المشكلة معروف | AI، في الـPrompt: `issue_category` ليست `unknown` |
| V4 | الجهاز أو النظام مذكور | AI، في الـPrompt: `affected_item` ليست `Not provided` |
| V5 | لا معلومات ناقصة | AI، في الـPrompt: `missing_information` = `NONE` |
| V6 | لا شبهة أمنية | AI، في الـPrompt: `security_concern` = `false` |

AI يرجع النتيجة في `request_status`، وPath A في Zapier يقبل فقط `request_status` **يطابق تمامًا** `VALID`.
**أي قيمة أخرى** (بما فيها قيمة فارغة أو غير متوقعة، E5 في الملف 02) تذهب إلى `NEEDS_REVIEW` (المسار البديل).

> الإعداد الدقيق في Zapier موجود في `04` و`05`.

---

## 9. قرارات التصميم المؤثرة على التوجيه (فئة C)

| # | القرار | ما اخترته | البديل الذي استبعدته ولماذا |
|---|---|---|---|
| D1 | قائمة `issue_category` | 8 قيم كما في 4.1 | فئات أكثر تفصيلًا: لا حاجة لها، لأن المسار واحد لكل الفئات |
| D2 | تعريف `security_concern` | كما في 4.2، ونسيان كلمة المرور **ليس** شبهة أمنية | تعريف أوسع: سيحيل طلبات عادية كثيرة إلى المراجعة بلا داعٍ |
| D3 | `affected_item` شرط لـ`VALID` | نعم (V4) | جعله اختياريًا: يقلل الإحالات البشرية، لكن يصل الطلب إلى فريق الدعم ناقصًا |
| D4 | `issue_summary` بالإنجليزية دائمًا، حتى لو كان الطلب بالعربية | نعم، لتوحيد السجل | ملخص بلغة الطلب: يجعل السجل مختلط اللغات |
| D5 | `Department` لا يؤثر على التوجيه | نعم | التوجيه حسب القسم: لا توجد فرق متعددة في النطاق |
| D6 | من يحسب `request_status`؟ | AI، بقواعد مكتوبة صراحة في الـPrompt (طريقة المحاضرة، 2026-10-15)، وPaths يفحص `VALID` فقط | ستة شروط في Paths (التصميم الأول): أكثر ثباتًا، لكن اعتُمدت طريقة المحاضرة |
