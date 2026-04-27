# System Rules — Moodle Teacher Hub

מסמך זה מפרט את כללי המערכת המעשיים לפיתוח, בדיקה, תיעוד והמשך עבודה.

`PROJECT_RULES.md` הוא מקור האמת העליון. המסמך הזה מפרט איך מיישמים אותו בפועל.

---

## A. כללי אמת בממשק

1. אין דמו.
2. אין נתונים מומצאים.
3. אין fallback לנתוני דוגמה.
4. אין שימוש ב־0 עבור ערך חסר.
5. אין להציג `מחובר`, `סונכרן`, `זמין`, או `פעיל` בלי בדיקת אמת.
6. כל מסך ריק חייב להסביר מה חסר ומה צריך לייבא.

דוגמאות טקסט מומלצות:

- `עדיין לא יובאו נתונים אמיתיים מדוח Moodle`
- `נדרש דוח ציונים ממודל כדי להציג ציונים`
- `נדרש דוח Logs כדי להציג פעילות`
- `לא ניתן לחשב ללא לוגים`
- `לא זמין בקובץ הזה`

---

## B. כללי ייבוא

כל Import חייב לשמור:

- `source_type`
- `file_name`
- `row_count`
- `column_mapping`
- `warnings`
- `created_at`
- `imported_by`

כל Import חייב לכלול preview לפני כתיבה.

אם parser מזהה דוח בביטחון נמוך — חובה לאפשר mapping ידני.

---

## C. כללי Adapter

כל מסך קורא דרך adapter/hook/RPC בלבד.

אסור:

```text
src/pages/* -> imported_* direct query
src/pages/* -> fake hardcoded rows
src/pages/* -> local demo arrays
```

מותר:

```text
src/pages/* -> hooks/useImports.tsx
src/pages/* -> lib/dataAdapters/*
src/pages/* -> typed RPC wrappers
```

---

## D. כללי Supabase

- `VITE_SUPABASE_URL` ו־`VITE_SUPABASE_ANON_KEY` יכולים להופיע ב־`.env.example` בלבד.
- `SERVICE_ROLE_KEY` לעולם לא נכנס לקוד frontend.
- Edge Functions בלבד משתמשות ב־service role.
- imported data לא נקרא ישירות מהלקוח.
- RPC חייב לבדוק session token / teacher context.

---

## E. כללי LTI

- LTI משמש לפתיחה והקשר בלבד.
- Moodle data מגיע רק מייבוא ידני או מ־WS token עתידי מאומת.
- כל כשל signature חייב להציג הודעת שגיאה אמיתית.
- אין להסתיר כשל OAuth.
- אין להשתמש ב־URL Markdown wrapped בתוך signature base string.

---

## F. כללי UI / RTL

- עברית מלאה.
- `dir="rtl"` ברמת האפליקציה או ה־HTML.
- אין צבעים hardcoded ברכיבים.
- צבעים דרך CSS variables / Tailwind tokens בלבד.
- סטטוסים צריכים להשתמש ב־tokens:
  - proven
  - missing
  - blocked

---

## G. כללי GitHub

לפני כל שינוי משמעותי:

1. לבדוק מה כבר קיים.
2. לא למחוק קוד עובד בלי סיבה מתועדת.
3. לעדכן תיעוד יחד עם שינוי התנהגות.
4. לתעד בדיקות שבוצעו.
5. לתעד דברים שלא נבדקו.

קבצים מחייבים:

- `PROJECT_RULES.md`
- `README.md`
- `docs/system-rules.md`
- `docs/repository-map.md`
- `STATE/project-status.md`

---

## H. הגדרת סטטוס

סטטוסים מותרים:

- `planned` — מתוכנן בלבד, אין קוד.
- `partial` — יש קוד חלקי או לא נבדק.
- `pilot-ready` — מתאים לפיילוט עם נתוני אמת ידניים.
- `production-ready` — רק אחרי בדיקות end-to-end מלאות.
- `blocked` — חסום בגלל token, API, הרשאה, או מקור נתונים חסר.

אסור להשתמש ב־production-ready בלי הוכחה.
