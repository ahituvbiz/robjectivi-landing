# רובייקטיבי — PROJECT CONTEXT
# ═══════════════════════════════════════════════════════════════
# קובץ זה מיועד לספק הקשר מלא ל-Claude בכל שיחה חדשה.
# שמור אותו בתיקייה הראשית של הפרויקט במחשב.
# בתחילת כל שיחה חדשה ב-Cowork, ודא ש-Claude קורא קובץ זה.
# ═══════════════════════════════════════════════════════════════

## 🔑 GitHub — גישה אוטומטית
## בכל פעם שצריך לדחוף קבצים ל-GitHub:
## 1. קרא את ה-token מ: `<תיקיית הפרויקט>/.github_token`
## 2. הגדר remote: `git remote set-url origin "https://${TOKEN}@github.com/ahituvbiz/robjectivi-landing.git"`
## 3. הגדר user: email=ahituv.biz@gmail.com, name=ahituvbiz
## 4. בצע add/commit/push כרגיל — ללא צורך לבקש token מהמשתמש
## ═══════════════════════════════════════════════════════════════

## ⚠️ הוראה ל-Claude — חובה!
## בסיום כל שיחה שבוצעו בה שינויים בפרויקט (קבצים חדשים, שינוי מבנה,
## שינוי תסריט, URLs חדשים, כללים חדשים, תיקוני באגים משמעותיים וכו'):
## 1. עדכן את הסעיפים הרלוונטיים בקובץ זה כך שישקפו את המצב החדש.
## 2. הוסף שורה ללוג שינויים (סעיף 9) עם תאריך ותיאור קצר.
## 3. אם נוצרו קבצים חדשים — הוסף אותם למפת הקבצים (סעיף 2).
## 4. אם השתנה תסריט השיחה — עדכן את טבלת ה-States (סעיף 3.2).
## ═══════════════════════════════════════════════════════════════

## 1. תיאור הפרויקט

**רובייקטיבי** הוא כלי אוטומטי לניתוח דוחות פנסיה ישראליים.
המשתמש מעלה PDF של דוח פנסיה → Claude API מחלץ JSON מובנה → ניתוח rule-based בפייתון → הצגת תוצאות בעברית.

הפרויקט פועל ב-**2 מדיות** (ערוצים) שחולקות את אותו ליבת ניתוח (`pension_core.py`):
1. **אפליקציית Streamlit** — אתר ווב
2. **בוט WhatsApp** — Flask + Twilio

בנוסף יש **דף נחיתה** סטטי ב-HTML (GitHub Pages).

> ⚠️ **בוט Telegram הוסר לחלוטין (2026-03-18)** — תיקיית `טלגרם/` נמחקה. ה-repo `rubiyakteevi-telegram` ב-GitHub עשוי עדיין להתקיים.

**הבעלים:** איתן (אין רקע טכני, מסתמך על Claude להנחיות צעד-אחר-צעד).

---

## 2. מיקום הקבצים

### 2.1 מחשב מקומי
```
תיקייה ראשית/
├── סטרימליט/
│   ├── app.py                    # Entry point — Streamlit web app
│   ├── requirements.txt          # streamlit, PyMuPDF, openai, pandas, openpyxl
│   └── core/
│       ├── __init__.py
│       └── pension_core.py       # ליבת ניתוח משותפת (~37KB)
│
├── ווצאפ/
│   ├── main.py                   # Entry point — Flask webhook
│   ├── charts.py                 # matplotlib — bar chart + gauge
│   ├── start.sh                  # auto-pull מ-GitHub + gunicorn
│   ├── requirements.txt          # flask, twilio, anthropic, requests
│   └── core/
│       ├── __init__.py
│       └── pension_core.py       # עותק של אותו pension_core
│
└── דף נחיתה/
    └── robjectivi-landing-main.zip   # GitHub Pages repo (ZIP)
```

> **בוט טלגרם — נמחק 2026-03-18.** תיקיית `טלגרם/` הוסרה לחלוטין.

### 2.2 GitHub (user: ahituvbiz)
| Repo | תוכן | GitHub Pages |
|------|-------|-------------|
| `ahituvbiz/pension-analyzer` | Streamlit app (private) | ❌ |
| `ahituvbiz/robjectivi-landing` | דף נחיתה + terms.html + advisor_explain.html | ✅ `ahituvbiz.github.io/robjectivi-landing/` |
| `ahituvbiz/whatsapp-pension-bot` | בוט WhatsApp (private) | ❌ |
| ~~`ahituvbiz/rubiyakteevi-telegram`~~ | ~~בוט טלגרם~~ — **הוסר 2026-03-18** | ❌ |

### 2.3 Replit
| Repl | תוכן | Secrets נדרשים |
|------|-------|---------------|
| WhatsApp bot | Flask + gunicorn | `ANTHROPIC_API_KEY`, `TWILIO_ACCOUNT_SID`, `TWILIO_AUTH_TOKEN`, `GITHUB_TOKEN`, `STATS_SECRET` |

#### ארכיטקטורת עדכון (Replit ← GitHub)
הבוט מ-Replit שולף קוד עדכני מ-GitHub בכל הפעלה.

**זרימת עדכון שוטפת:**
1. Claude מעדכן קוד מקומי ← דוחף ל-GitHub
2. איתן מבצע Pull + Republish ידנית ב-Replit

**⚠️ כללי עבודה ל-Claude:**
- אין לגעת ב-`start.sh` — עבר אופטימיזציה לסביבת Deployment של Replit
- אין להוסיף פקודות `git pull` לקוד
- קבצי עבודה בלבד: `main.py`, `charts.py`, `core/pension_core.py`

### 2.4 Streamlit Cloud
האפליקציה מ-`pension-analyzer` deployed ב-Streamlit Cloud.

### 2.5 שירותים חיצוניים
- **Meshulam** — עמוד תשלום ליועץ: `https://meshulam.co.il/s/9b71389b-1564-a6c6-5f08-883647e04c53`
- **Google Sites** — אתר הלקוחות (דף הנחיתה מוטמע בו)
- **Twilio WhatsApp Sandbox** — webhook לבוט ווצאפ

---

## 3. ארכיטקטורה

### 3.1 pension_core.py (משותף ל-2 הערוצים)
קובץ ליבה (~37KB) המכיל:
- `SYSTEM_PROMPT` + `USER_PROMPT` — הפרומפט ל-Claude API שמחלץ JSON מ-PDF
- `format_number()`, `g()` — עיצוב מספרים + פונקציית מין (גבר/אשה)
- `detect_deposit_source()` — זיהוי שכיר/עצמאי/מעורב
- `validate_report()` — אימות שה-JSON תקין
- `compute_analysis()` — חישובי ליבה (הכנסה מבוטחת, קצבת שארים/נכות, גיל, צבירה...)
- `check_insurance()` — בדיקת כיסויים ביטוחיים + אזהרות
- `extract_fee_rates()`, `calc_annual_fee()` — חישוב דמי ניהול
- `FUND_PLANS`, `ADVISOR_PLAN`, `MAX_FEES` — טבלאות דמי ניהול לפי קרנות
- `EQUITY_TRACKS`, `MADEDEI_WARNING_FUNDS` — מסלולי השקעה מנייתיים
- `find_fund_key()`, `is_equity_track()`, `is_age_related_track()` — זיהוי מסלולים
- `GOV_EMPLOYERS`, `is_gov_employer()` — רשימת מעסיקים ממשלתיים

**חשוב:** כל שינוי ב-pension_core.py צריך להיות מסונכרן בשתי התיקיות (ווצאפ + סטרימליט)!

### 3.2 בוט WhatsApp — מבנה מפורט

#### תסריט שיחה (States כ-strings)
```
"welcome" → "consent" → "gender" → "marital" → ["children"] → "awaiting_pdf" → "processing" → "post_analysis"
```

| State | תיאור | מעבר הבא |
|-------|--------|----------|
| `welcome` | ברכה + לינק תנאים + כפתורי אישור | → consent |
| `consent` | אישור בכפתור (1/2) או טקסט | → gender |
| `gender` | כפתורי quick-reply: גבר / אישה | → marital |
| `marital` | כפתורי quick-reply: נשוי/רווק/אלמן-גרוש | → children (אם גרוש/אלמן) / awaiting_pdf |
| `children` | כפתורי quick-reply: יש/אין ילדים מתחת ל-21 | → awaiting_pdf |
| `awaiting_pdf` | ממתין לקובץ PDF | → processing |
| `processing` | ניתוח מתבצע ב-background thread | → post_analysis |
| `post_analysis` | שותק; כל הודעה = ניתוח חדש (אם consent בתוקף) | → gender / welcome |

**מנגנונים מיוחדים:**
- **pending_pdf** — PDF שנשלח בכל שלב נשמר ומעובד אוטומטית כשמגיעים ל-awaiting_pdf
- **timeout** — חוסר פעילות >1 שעה מאפס את ה-session
- **חסימת מספרים זרים** — מספרים ללא `+972` מקבלים תגובה ריקה בשקט (ללא קריאה ל-API)
- **ניקוי sessions** — sessions חסרי פעילות >4 שעות נמחקים אוטומטית מהזיכרון
- **מכסה שבועית** — MAX_WEEKLY_REPORTS=4 דוחות לשבוע לכל מספר טלפון
- **Session storage — Replit DB** (לא זיכרון!) — sessions עמידים ל-restart. ניהול: `get_session()` / `save_session()` / `_serialize_session()` / `_deserialize_session()`
- **welcome flow — background thread** — webhook מחזיר 204 מיידית; welcome messages נשלחות ב-thread נפרד (למניעת Twilio timeout)

**קבצי הבוט:**
- **`main.py`** — Flask webhook + session management + Claude API + ניתוח + שליחה. קובץ יחיד.
- **`charts.py`** — `create_deposits_chart()` + `create_fee_gauge()` (matplotlib)
- **`core/pension_core.py`** — ליבת הניתוח המשותפת

**Twilio Content Templates (TEMPLATE_SIDS ב-main.py):**
- Quick-reply: `consent`, `gender`, `marital`, `children`
- CTA links: `terms_link`, `advisor_booking` (SID: `HX0e9bc7803e058f3f2f4b5e161f3d0a5c`), `advisor_explain`, `subsidy_link`, `advisor_card`

**Analytics:**
- `log_activity()` — שומר מספר טלפון + תאריך ראשון + האם שלח PDF ב-Replit DB
- `/stats?key=<STATS_SECRET>` — endpoint מוגן להצגת סטטיסטיקות

### 3.3 Streamlit App — מבנה מפורט
- **קובץ יחיד: `app.py`** (~1140 שורות) — כולל UI + ניתוח + גרפים inline.
- עיצוב RTL + CSS מותאם + Dark theme.
- Upload PDF → Claude API → תצוגת כרטיסיות (tabs) ל-4 נושאים.
- Gauge ו-bar chart מוטמעים ב-HTML (לא matplotlib).
- כפתורי CTA: WhatsApp ויועץ.

---

## 4. גרף דמי ניהול (Fee Gauge) — מבנה נוכחי

ב-`charts.py` — semicircular gauge עם matplotlib:
- **צד שמאל** = אדום (יקר/מקסימום) — תווית: `₪{max_fee} מקסימום`
- **אמצע** = צהוב (בינוני)
- **צד ימין** = ירוק (זול/הזול ביותר) — תווית: `₪{cheapest_fee} הזול ביותר`
- מחט מצביעה למיקום המשתמש (לאחר `pct = 1.0 - pct`)
- כיתוב בעברית: "מקסימום" / "הזול ביותר" / "העלות השנתית שלך:" / "דמי ניהול: X% מהפקדה + Y% מצבירה"

**הסדר הנוכחי: אדום-שמאל (יקר), ירוק-ימין (זול).**

---

## 5. דף תנאי שימוש (terms.html)

דף HTML עצמאי עם:
- עיצוב RTL עברי (פונט Heebo)
- 5 סעיפי תנאים: מהות השירות, לא ייעוץ פנסיוני, מגבלות, אחריות, פרטיות
- אישור התנאים מתבצע ישירות בכפתורי ה-WhatsApp (מאשר / לא מאשר) לאחר שהמשתמש קורא את הדף

---

## 6. URLs חשובים

| מפתח | URL | סטטוס |
|------|-----|-------|
| TERMS_URL | `https://ahituvbiz.github.io/robjectivi-landing/terms.html` | ✅ פעיל |
| ADVISOR_URL | `https://pay.grow.link/70bd635ea6f32e700aace057ec21543a-MzIyNDIyNw` | ✅ פעיל |
| ADVISOR_EXPLAIN_URL | `https://ahituvbiz.github.io/robjectivi-landing/advisor_explain.html` | פעיל |
| SUBSIDY_URL | `https://ahituvbiz.github.io/robjectivi-landing/SUBSIDY.pdf` | ✅ פעיל |
| ADVISOR_PHOTO_URL | `https://ahituvbiz.github.io/robjectivi-landing/advisor_photo.jpg` | ✅ פעיל |
| דף נחיתה | `https://ahituvbiz.github.io/robjectivi-landing/` | פעיל |
| Streamlit App | `https://ahituv-pension-bot.streamlit.app/` | פעיל |

---

## 7. Claude API

- מודל: `claude-sonnet-4-20250514`
- max_tokens: 8000
- שולח PDF כ-base64 document
- מקבל JSON מובנה עם: header, deposits, late_deposits, movements, insurance, investment_tracks

---

## 8. מגבלות ידועות

- **pension_core.py מכפיל ב-2 תיקיות** (ווצאפ + סטרימליט) — כל שינוי עתידי חייב sync ידני
- **URLs ב-config.py** — ✅ TERMS_URL עודכן ל-GitHub Pages. ✅ ADVISOR_URL עודכן ל-pay.grow.link
- **WhatsApp** — ✅ כפתורים אינטראקטיביים דרך Twilio Content API עם fallback לטקסט
- **GitHub repos** — pension-analyzer (private), whatsapp-pension-bot (private), robjectivi-landing (public)
- **Gauge** — ✅ תוקן. **chart הפקדות** — ✅ תוקן (עברית + RTL + בלי עמודה אדומה)

---

## 9. לוג שינויים אחרון

*(עדכן סעיף זה אחרי כל שינוי משמעותי)*

- **2026-03-11:** יצירת קובץ PROJECT_CONTEXT.md
- **2026-03-11:** שינויים מתוכננים — ראה TASKS.md
- **2026-03-11:** תיקון `charts.py` — gauge דמי ניהול: היפוך צבעים (אדום-שמאל, ירוק-ימין), היפוך מחט (`pct = 1.0 - pct`), תרגום כל כיתובים לעברית
- **2026-03-11:** יצירת `advisor_explain.html` — דף השוואה בוט לעומת ייעוץ פנסיוני מלא; הועלה ל-robjectivi-landing; עודכן `ADVISOR_EXPLAIN_URL` ב-config.py
- **2026-03-11:** GitHub token נשמר ב-`.github_token` (בתיקייה הראשית) — Claude יקרא אותו אוטומטית בכל push עתידי
- **2026-03-11:** שלב 4 — pending PDF: conversation.py קיבל `pending_pdf_file_id` ב-session + 3 methods (set/has/pop); main_telegram.py קיבל `_process_pdf_by_file_id()` ו-`_check_and_process_pending_pdf()` helpers; handle_document/text/callback עודכנו
- **2026-03-11:** שלב 5 — `del pdf_bytes` נוסף ב-`_process_pdf_by_file_id()` (מכסה גם flow רגיל וגם pending PDF)
- **2026-03-12:** ארכיטקטורת GitHub ← Replit הושלמה לבוט טלגרם: נוצר repo `rubiyakteevi-telegram`, אותחל git מקומי, נוספו `start.sh` + `setup_replit.sh` + `.replit` לאוטו-עדכון
- **2026-03-12:** תיקונים בגרפים ולינקים: RTL בגרף מחוג + גרף הפקדות (עברית, ללא עמודה אדומה); TERMS_URL תוקן; ADVISOR_URL → pay.grow.link; advisor_explain.html → כפתור WhatsApp; עודכן גם ב-robjectivi-landing (GitHub Pages): נוצר repo `rubiyakteevi-telegram`, אותחל git מקומי, נוספו `start.sh` + `setup_replit.sh` + `.replit` לאוטו-עדכון. תיקיית `טלגרם` מקומית כעת מסונכרנת עם GitHub.
- **2026-03-12:** הוספת תמונת license.png בכל 3 הערוצים: Streamlit (מעל תוצאות הניתוח), Telegram (BotResponse עם image בין הברכה לתנאים, נטען מקובץ מקומי `license.png`), WhatsApp (Twilio media מ-GitHub Pages URL: `ahituvbiz.github.io/robjectivi-landing/license.png`). הקובץ license.png הועלה גם ל-pension-analyzer, rubiyakteevi-telegram ו-robjectivi-landing.
- **2026-03-14:** נוסף URL של Streamlit App לטבלת URLs (סעיף 6): `https://ahituv-pension-bot.streamlit.app/`
- **2026-03-14:** עדכון PROJECT_CONTEXT.md — תיקון סעיף 6 (TERMS_URL כבר פעיל ב-GitHub Pages) + אישור sync pension_core.py (checksum זהה בכל 3 עותקים)
- **2026-03-15:** החלפת `pdf_export.py` בגרסה מעודכנת — תמיכה ב-bidi, DejaVu fonts, 4 סקציות ניתוח (כיסויים/הפקדות/דמי ניהול/השקעה), header עם שם חבר + קרן + תקופה, footer עם הצהרת אחריות. דחוף ל-GitHub repo `pension-analyzer`.
- **2026-03-15:** תיקון באג קריטי בטלגרם — כפתורי הנושאים (כיסויים/הפקדות/דמי ניהול/השקעה) לא הגיבו ללחיצה. הסיבה: `from charts import ...` הופעל לכל 4 הנושאים (כולל כאלה שלא צריכים גרף), וכל כשל בטעינה קרס בשקט. תיקון: ה-import הועבר לתוך try/except בתוך הבלוק הרלוונטי בלבד (deposits/fees). בנוסף תוקן טיפול ב-`analysis_sections=None` לאחר הפעלה מחדש של השרת. דחוף ל-GitHub repo `rubiyakteevi-telegram`.
- **2026-03-15:** Streamlit — תמונת הרישיון הועברה מראש הדף לתחתיתו, מתחת לכפתור "הורד דוח כ-PDF", עם רווח של 40px מעליה. דחוף ל-GitHub repo `pension-analyzer`.
- **2026-03-12:** סנכרון סקציית השקעה בין כל הערוצים: הושלמו משפטים חסרים בטלגרם ובווצאפ (5 שנים / עתיד, פיצויים+מעסיק לפי deposit_source, S&P 4 משפטים, מדדי מניות מפורט, הלכה+ציפיית תשואה). תמונות בטלגרם עברו מקבצים מקומיים ל-URLs של GitHub Pages (_LICENSE_IMAGE_URL, _EQUITY_IMAGE_URL) — תואם כעת לווצאפ. נוסף שדה `image_url` ל-BotResponse ו-main_telegram.py מטפל בו.
- **2026-03-16:** בוט WhatsApp חובר ל-WhatsApp Business API (Twilio + Meta). Webhook מוגדר ב-Twilio. UptimeRobot מונע שינה. ANTHROPIC_API_KEY הוחלף (מפתח ישן נחשף).
- **2026-03-16:** ארכיטקטורת GitHub ← Replit הושלמה לבוט ווצאפ: נוספו `start.sh` + `.replit` (modules python-3.11) לאוטו-עדכון מ-GitHub בכל הפעלה. דחוף ל-repo `whatsapp-pension-bot`.
- **2026-03-16:** שיפור תסריט שיחה בווצאפ (`main.py`): (1) הודעות welcome נפרדות (ברכה + רישיון + הסבר תנאים + לינק + אפשרויות אישור), (2) אישור תנאים ב-1/2 במקום טקסט ארוך, (3) 3 אפשרויות מצב משפחתי (נשוי/רווק/אלמן-גרוש), (4) כפתורי ילדים עם טקסט מלא, (5) pending PDF — שמירה ועיבוד אוטומטי, (6) timeout reset לאחר שעה, (7) תיקון TERMS_URL ו-ADVISOR_EXPLAIN_URL.
- **2026-03-16:** כפתורים אינטראקטיביים בווצאפ + שליחת כל הניתוחים אוטומטית: (1) נוספה תמיכה בכפתורי quick-reply אמיתיים דרך Twilio Content API עם fallback חלק לטקסט ממוספר; (2) הסרת states `results_menu` ו-`results_view` — הבוט שולח את כל 4 הניתוחים אחד אחרי השני אוטומטית; (3) `ButtonPayload` בוובהוק גובר על `Body`.
- **2026-03-16:** כפתורי CTA עם קישורי URL — נוצרו תבניות `advisor_booking` + `advisor_explain` בטwilio Content Template Builder; `send_cta_messages()` שולח אותן בסוף השיחה (כשמשתמש אומר "לא" לדוח נוסף או כשחרג ממכסה). Content Template SIDs של כל 6 התבניות שמורים ב-`TEMPLATE_SIDS` dict ב-`main.py`.
- **2026-03-16:** הוספת תבנית `advisor_card` (Card עם תמונת יועץ + 2 כפתורי URL) ותבנית `subsidy_link` (CTA לפתיחת SUBSIDY.pdf). `advisor_photo.jpg` + `SUBSIDY.pdf` הועלו ל-robjectivi-landing (GitHub Pages). `send_cta_messages()` עודכן להשתמש ב-Card. עובד מדינה מקבל כעת גם כפתור CTA לצפייה ב-SUBSIDY.pdf. תוקן באג: `cta_messages()` לא-קיים בסטייט `post_analysis` → `send_cta_messages()`. סה"כ 9 Content Templates ב-`TEMPLATE_SIDS`.
- **2026-03-16:** הסרת `license.png` מ-welcome בווצאפ (שני מקומות: welcome רגיל + post_analysis restart).
- **2026-03-16:** שיפור `send_cta_messages()` — שולח תמיד `advisor_booking` + `advisor_explain` כשני כפתורי URL נפרדים, ואחריהם `advisor_card` אם מוגדר. כך גם אם ה-Card נכשל בשקט — שני הכפתורים עדיין מגיעים.
- **2026-03-16:** תיקון תמונת מדדי מניות בווצאפ ובטלגרם: (1) תמונה נשלחת **לפני** טקסט ההשקעה (כמו בטלגרם); (2) עטיפה ב-try/except — אם URL נכשל הבוט ממשיך (ללא crash שמנע גם שליחת CTA); (3) הקובץ המקורי `madedei_maniut.png` (6.4MB) חרג ממגבלת WhatsApp — דוחס ל-`madedei_maniut.jpg` (132KB JPEG, 1200px), הועלה ל-robjectivi-landing; URL עודכן בווצאפ ובטלגרם.
- **2026-03-16:** תיקון סדר הודעות בווצאפ (`main.py`): הוחלפו כל `resp.message()` ב-`send_wa()` ישיר ב-states: welcome (ברכה), consent ("תודה על האישור!"), post_analysis (הודעת סיום). הבעיה: TwiML נשלח אחרי ה-HTTP response ולכן תמיד הגיע אחרי קריאות ישירות ל-API.
- **2026-03-16:** שיפורי הצגת ניתוח בווצאפ: (1) הוספת `time.sleep(0.5)` בין כל הודעה בזרם הניתוח לשמירת סדר; (2) כותרות הסקציות מודגשות (`*כותרת*`); (3) `send_cta_messages()` מופעלת אוטומטית אחרי כל ניתוח (לא רק כשמשתמש אומר "לא").
- **2026-03-16:** הסרת שאלת "רוצה דוח נוסף?" — `post_analysis` state עודכן: כל הודעה אחרי הניתוח מתחילה ניתוח חדש ישירות אם הסכמה בתוקף (< שעה); אם הסכמה פגה — חוזר ל-welcome+consent. אין צורך בשאלה מיותרת למשתמש.
- **2026-03-16:** דף נחיתה (`index.html`) — החלפת כפתור יחיד בשני כפתורים לפי מכשיר: (1) מחשב → כפתור כחול-סגול → Streamlit iframe; (2) מובייל → כפתור ירוק → WhatsApp (`wa.me`). זיהוי מכשיר ב-JS לפי UserAgent + רוחב מסך (<768px). שימוש ב-`window.open(_blank)` במקום `window.location.href` כדי לעקוף את מגבלות ה-iframe של Google Sites.
- **2026-03-16:** שחזור קבצים שנמחקו בטעות מ-robjectivi-landing (force push): שוחזרו `terms.html`, `advisor_explain.html`, `license.png`, `madedei_maniut.jpg`, `SUBSIDY.pdf`, `advisor_photo.jpg` (הומר מ-`profile.png`).
- **2026-03-16:** הגדלת `time.sleep` מ-0.5 ל-2 שניות בין כל הודעה בזרם הניתוח בווצאפ (טקסט, גרפים, תמונות, CTA) — למניעת הגעת הודעות שלא בסדר ב-WhatsApp.
- **2026-03-17:** תיקון קריסות בוט ווצאפ — תיקונים בקוד ובתשתית Replit: (1) `_process_pdf` עובר ל-daemon background thread בכל 4 נקודות הקריאה — webhook מחזיר תשובה ל-Twilio תוך פחות משנייה; (2) gunicorn ב-`start.sh` עודכן ל-`--workers 1 --worker-class gthread --threads 4 --timeout 180`; (3) תוקן memory leak ב-`_chart_cache` — `pop()` במקום `get()`; (4) פורט שונה מ-5000 ל-**8080**; (5) ב-Replit Deployment: Internal Port=8080, External Port=80, Secrets הועברו ל-Deployment environment; (6) imports של `charts` הועברו לראש הקובץ (טעינת matplotlib פעם אחת ב-startup); (7) `_process_pdf` עוטף ב-try-except — שגיאה לא צפויה שולחת הודעה למשתמש במקום לשתוק; (8) `start.sh` עודכן להשתמש ב-`GITHUB_TOKEN` מה-Secrets לצורך `git pull` — פותר בעיית קוד ישן בכל הפעלה; (9) נוסף מעקב משתמשים דרך Replit DB: `log_activity()` מתעדת כל משתמש ייחודי + מי שלח PDF; (10) נוסף route `/stats?key=<STATS_SECRET>` המחזיר JSON עם סטטיסטיקות; ה-URL: `https://whatsapp-pension-bot.replit.app/stats?key=...`; Secret `STATS_SECRET` הוגדר ב-Replit.
- **2026-03-16:** תיקון SYSTEM_PROMPT ב-`pension_core.py` (כל 3 עותקים): הוספת כלל לאיחוד שמות מסלולי השקעה שנפרסים על מספר שורות ב-PDF (דוגמה: "מסלול כלל פנסיה" + "עוקב מדד S&P 500" → שם אחד). ללא התיקון הבוט לא זיהה S&P 500 ולא שלח את תמונת מדדי המניות.
- **2026-03-16:** הסרת כפתור `advisor_card` (Card עם תמונת יועץ) מ-`send_cta_messages()` בווצאפ — נשארו רק `advisor_booking` + `advisor_explain`.
- **2026-03-18:** בוט טלגרם הוסר לחלוטין — תיקיית `טלגרם/` נמחקה. הפרויקט פועל כעת ב-2 ערוצים בלבד: Streamlit + WhatsApp.
- **2026-03-18:** אבטחה — סדרת שיפורים בבוט WhatsApp: (1) חסימת מספרי טלפון לא ישראליים (ללא `+972`) בכניסה לוובהוק, לפני כל קריאה ל-API; (2) ניקוי אוטומטי של sessions חסרי פעילות >4 שעות (`_cleanup_old_sessions()`); (3) תיקון הגדרה כפולה של `log_activity`; (4) sync קבצים ל-GitHub: `charts.py` (נוסף לראשונה), `start.sh`, `requirements.txt`, `core/pension_core.py`.
- **2026-03-18:** ריפוסיטוריז — `whatsapp-pension-bot` הוגדר כ-Private; `rubiyakteevi-telegram` + `PENSION-BOT` נמחקו.
- **2026-03-25:** עדכון `ADVISOR_URL` בכל הערוצים (ווצאפ + Streamlit) — לינק חדש לשיחת הסבר: `pay.grow.link/70bd635ea6f32e700aace057ec21543a-MzIyNDIyNw`.
- **2026-03-25:** עדכון Twilio Content Template `advisor_booking` — SID חדש: `HX0e9bc7803e058f3f2f4b5e161f3d0a5c` (תבנית ישנה נמחקה ונבנתה מחדש עם הלינק המעודכן).
- **2026-03-25:** הגדלת `time.sleep` ל-**5 שניות** בין כל הודעה בזרם הניתוח בווצאפ (בכל מקומות ה-sleep ב-main.py).
- **2026-03-25:** תיקון סיווג S&P 500 כמסלול מנייתי — נוסף fallback לפי keywords: `"s&p" in nl or "500" in nl` לזיהוי גם כשהקרן לא מוכרת ב-EQUITY_TRACKS.
- **2026-03-25:** עדכון טקסט אזהרת S&P — "אתה מושקע במסלול S&P 500. מדד מניות זה סובל היום מריכוזיות גבוהה..." (ווצאפ + Streamlit). סדר תמונה תוקן ב-Streamlit — תמונת מדדי מניות נשלחת לפני הטקסט.
- **2026-03-25:** הוספת הודעת "מסלול מומלץ" — כשכל מסלולי ההשקעה של הלקוח הם מנייתיים (רשימת `neq` ריקה ו-`eq` לא ריקה), נשלחת הודעה: "✅ לדעתי מסלול ההשקעה שלך הוא המומלץ ביותר" (ווצאפ + Streamlit).
- **2026-03-25:** תיקון לופ בבוט ווצאפ — welcome flow הועבר ל-background thread ו-webhook מחזיר 204 מיידית. פתרון: Twilio timeout היה 15 שניות ו-3×sleep(5) גרם לחזרה על ה-webhook.
- **2026-03-25:** מעבר sessions מזיכרון ל-**Replit DB** — נוספו: `_default_session()`, `_serialize_session()`, `_deserialize_session()`, `get_session()`, `save_session()`. Sessions כעת עמידים ל-restart של Gunicorn.
- **2026-03-25:** הוספת logging לוובהוק: `print(f"[WEBHOOK] From={from_num} | Body='{incoming}' | NumMedia={num_media}")`.
- **2026-03-25:** עדכון `requirements.txt` בבוט ווצאפ — נוספו: `gunicorn`, `pandas`, `openpyxl`.
- **2026-03-25:** ניקוי `start.sh` — הוסרו פקודות `git pull`; נשאר רק: `pip install -r requirements.txt -q` + לופ gunicorn.
- **2026-03-30:** הוספת הודעת הפניה ויראלית בסוף ניתוח הבוט (`main.py`, ווצאפ):
  - **`_has_significant_issues(sections, user_profile)`** — בודק אם יש בעיות משמעותיות: דמי ניהול גבוהים (חיסכון >0.25% + הפקדה >2%) **ובנוסף** כיסוי ביטוחי מיותר (הודעת check_insurance מכילה "מיותר") **או** כל מסלולי ההשקעה הם ברירת מחדל (גיל ≤52).
  - **`_send_referral_messages(from_num, to_num, use_version1)`** — שולח אחת משתי גרסאות:
    - **גרסה 1** (בעיות משמעותיות): "אם מה שגילית היום הפתיע אותך — כנראה שגם לחברים שלך יש אותה בעיה והם לא יודעים. שלח/י להם: 'בדקתי את הפנסיה שלי עם בוט חינמי וגיליתי דברים שלא ידעתי. אני בטוח ששווה לך להשקיע בזה 2 דקות — https://wa.me/972559289298'"
    - **גרסה 2** (ברירת מחדל): "שלח/י לחבר/ה שיש לו/לה פנסיה — פשוט העתק/י: 'בדקתי את הפנסיה שלי עם בוט חינמי. מסתבר שרוב האנשים לא יודעים מה ואיך לבדוק בדוח הפנסיה שלהם. שווה לבדוק — https://wa.me/972559289298'"
    - לאחר שתי הגרסאות: "אני ממליץ לך לחזור לכאן בעוד 3 חודשים עם הדוח הרבעוני הבא. תמיד שווה לבדוק את הפנסיה ובמיוחד כאשר תנאי השוק משתנים ואיתם ההמלצות שלי."
  - קריאה ב-`_process_pdf_inner()` אחרי `send_cta_messages()` עם `time.sleep(5)` לפני.
  - `BOT_LINK = "https://wa.me/972559289298"` הוגדר כקבוע גלובלי.
- **2026-03-29:** שכתוב מלא של לוגיקת מסלול ההשקעה — pension_core.py + main.py (ווצאפ) + app.py (סטרימליט):
  - **סיווג 10 קטגוריות:** `recommended`, `managed`, `ברירת_מחדל`, `ללא_מניות`, `sp500`, `טכנולוגיה`, `הלכה`, `ישראלי`, `ישראלי_כלל_מנורה`, `קיימות`, `אחר`.
  - **נרמול שם מסלול** (`normalize_track_name`): הסרת מילות רעש (שמות חברות, "מסלול", "עוקב" וכו') + איחוד גרסאות S&P.
  - **מבני נתונים חדשים ב-pension_core.py:** `RECOMMENDED_TRACKS`, `TECH_TRACKS`, `ISRAELI_TRACKS`, `ISRAELI_STRICT_FUNDS`, `EQUITY_TRACKS` (שמות בלבד), `TRACK_NOTES` (עם `{diff:,}` placeholder), `IMAGE_CATEGORIES`.
  - **פונקציות חדשות:** `normalize_track_name()`, `classify_track()`, `calc_fv_diff()` (הפרש ערך עתידי FV).
  - **ווצאפ — multi-message flow:** `build_investment_messages()` מחזירה רשימת `{"text":...}` / `{"image":True}`; `_process_pdf_inner()` שולח כל הודעה בנפרד עם `time.sleep(5)`.
  - **הודעות ווצאפ:** (1) רשימת מסלולים; (2) גיל>52 → הפחתה הדרגתית; גיל≤52: (2) המלצה מנייתית כללית; (3) פיצויים אם ברירת מחדל + מסלול אחר; (4) תמונה אם sp500/טכנולוגיה; (5א) all recommended/managed → ✅; (5ב) all ברירת מחדל → FV diff + המלצה; (5ג) שילוב → הערה לכל קטגוריה + המלצה.
  - **FV diff:** `ברירת_מחדל` → rates (5.25%, 4%); `ללא_מניות` → rates (5.5%, 4%).
  - **pension_core.py מסונכרן** לשתי התיקיות (ווצאפ/core + סטרימליט/core).
