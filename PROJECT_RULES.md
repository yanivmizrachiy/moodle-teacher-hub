# PROJECT_RULES — Moodle Teacher Hub

מסמך זה הוא מקור האמת העליון של הריפו `moodle-teacher-hub`.

כל שינוי אמיתי בפרויקט חייב להיבדק מול המסמך הזה. אם קוד, מסך, README, דוח סטטוס או תיעוד אחר סותרים את המסמך הזה — המסמך הזה קובע, ואז חובה לעדכן את הקובץ הסותר או לתקן את `PROJECT_RULES.md` בצורה מפורשת ומנומקת.

---

## 1. מטרת הפרויקט

`Moodle Teacher Hub` הוא מרכז מורה עברי, RTL, עבור מורים העובדים בתוך מרחבי Moodle.

המטרה היא לא להחליף את Moodle, לא ליצור תוכן לימודי חדש, ולא להציג נתוני דוגמה. המטרה היא לארגן, לנתח, להציג ולייצא נתוני Moodle אמיתיים שהמורה הביא ממקור אמיתי.

המערכת צריכה לאפשר למורה לראות בצורה נוחה:

- תלמידים
- משימות
- פרקים / סקציות
- ציונים
- השלמות פעילות
- לוגים / פעילות
- דוחות
- ייצוא נתונים
- סטטוס מקורות נתונים

---

## 2. מצב עבודה פעיל

המצב הפעיל נכון לעכשיו הוא:

**Manual Real Data Import**

כלומר:

- אין להניח שקיים Moodle Web Services API token.
- אין למשוך נתונים חיים מ־Moodle בלי token אמיתי ומאומת.
- אין לבקש שם משתמש או סיסמה של Moodle.
- אין לבצע scraping של Moodle.
- אין להציג כאילו יש live sync אם אין כזה.
- הנתונים מגיעים מקבצי Moodle אמיתיים או מטבלאות Moodle אמיתיות שהמורה מייבא ידנית.

LTI משמש לכניסה ולהקשר בלבד: קורס, מורה, תפקיד, launch/session. LTI אינו מקור נתוני תלמידים, ציונים, משימות או פעילות.

---

## 3. אמת לפני הכול

כללי אמת מחייבים:

- אין נתוני דמו.
- אין תלמידים מזויפים.
- אין ציונים מזויפים.
- אין משימות מזויפות.
- אין פעילות מזויפת.
- אין זמן עבודה מומצא.
- אין counters מזויפים.
- אין badges או סטטוסים שמרמזים על חיבור חי אם אין חיבור כזה.
- ערך חסר נשאר חסר.
- חוסר ציון אינו אפס.
- אם חסר מקור נתונים, יש לומר זאת בממשק בעברית פשוטה.

ניסוחי אמת מותרים:

- `חסר מהנתונים שיובאו`
- `לא זמין בקובץ הזה`
- `נדרש דוח נוסף ממודל`
- `לא ניתן לחשב ללא לוגים`
- `עדיין לא יובאו נתונים אמיתיים מדוח Moodle`

ניסוחים אסורים:

- `מחובר` כאשר אין חיבור חי אמיתי.
- `סונכרן` כאשר הנתון יובא ידנית בלבד.
- `0` במקום ערך חסר.
- `לא ידוע` כאשר המשמעות האמיתית היא שהנתון לא קיים בקובץ שיובא.

---

## 4. ארכיטקטורת נתונים מחייבת

המערכת חייבת לתמוך בשתי שכבות adapter, אך רק אחת פעילה כרגע:

### 4.1 ManualImportAdapter — פעיל

מקבל נתוני Moodle אמיתיים שהמורה מייבא ידנית:

- XLSX
- CSV
- TSV/TXT
- טבלה מועתקת מ־Moodle ומודבקת לממשק
- ODS רק אם הפענוח בפועל נבדק ומאומת

### 4.2 MoodleWsAdapter — עתידי בלבד

נשאר כ־stub או adapter חסום עד שקיים token אמיתי ומאומת.

כל עוד אין token:

- `canRead` אינו מוכיח יכולת קריאה חיה.
- `canWrite` חייב להיות false.
- פעולות כתיבה ל־Moodle חייבות להיות חסומות.
- כל UI שמדבר על live sync חייב להיות מוסר או מסומן כלא זמין.

---

## 5. מקורות Moodle נתמכים

סוגי הדוחות שהמערכת צריכה לתמוך בהם, לפי סדר עדיפות:

1. Students / Participants
2. Grades / Gradebook export
3. Activity completion
4. Logs / יומנים
5. Activity report
6. Participation report
7. Quiz attempts / grades — אם וכאשר ייבנה parser אמיתי
8. Assignment submissions / grades — אם וכאשר ייבנה parser אמיתי
9. Course page / activity list — רק אם ניתן לחלץ ממנו מבנה אמיתי

אין להציג תמיכה בסוג דוח אם אין parser אמיתי, preview אמיתי, mapping אמיתי וכתיבה אמיתית למודל הנתונים.

---

## 6. מודל נתונים מנורמל

כל מסך חייב לקרוא ממודל נתונים מנורמל, לא מהקובץ הגולמי ישירות.

יש לשמור הפרדה בין:

- מקור הייבוא
- batch ייבוא
- mapping עמודות
- warnings
- entities מנורמלים
- תצוגת UI

ישויות יעד:

- Course
- TeacherContext
- Student
- ChapterTopic
- ActivityTask
- GradeItem
- GradeResult
- AttemptRecord
- ActivityLogEvent
- DailyStudentActivitySummary
- ImportBatch
- ImportSource
- DataQualityIssue

אם ישות לא קיימת עדיין בקוד — אסור לטעון שהיא קיימת. יש לסמן אותה כמתוכננת.

---

## 7. כללי מסכים

כל מסך חייב לעמוד בכללים האלה:

- קריאה דרך adapter / hook / RPC בלבד.
- אין גישה ישירה מתוך page ל־`imported_*`.
- יש empty state פעיל וברור.
- יש הסבר איזה דוח Moodle חסר.
- אין כפתור שאינו מבצע פעולה אמיתית.
- אין נתונים פיקטיביים בזמן טעינה.
- אין fallback שקט לדמו.

מסכי יעד:

| Route | תפקיד | מקור נתונים |
|---|---|---|
| `/` | Dashboard | overview אמיתי |
| `/import` | ייבוא נתונים | קבצים/הדבקה |
| `/students` | תלמידים | students import |
| `/students/:id` | פרופיל תלמיד | students + grades + completion + logs |
| `/grades` | ציונים | gradebook import |
| `/tasks` | משימות | activity completion / course structure |
| `/chapters` | פרקים | course structure |
| `/chapters/:sectionId` | פרק בודד | tasks לפי section |
| `/activity` | פעילות וזמנים | logs |
| `/reports` | דוחות | overview + imports |
| `/export` | ייצוא | imported normalized data |
| `/settings` | הגדרות ומקורות | import batches / metadata |
| `/sites` | מטא־ניהול | LTI/site metadata |
| `/setup` | התקנה | LTI/config metadata |

---

## 8. LTI

LTI הוא שער כניסה והקשר בלבד.

חובה:

- אימות OAuth1 HMAC-SHA1 כאשר עובדים ב־LTI 1.x.
- ניקוי whitespace ו־Markdown wrapping מערכי URL/consumer key/secret.
- שמירת session token מוגבל בזמן.
- session חדש במכשיר חדש או launch חדש.
- הודעת שגיאה אמיתית כאשר launch נכשל.

אסור:

- להניח שה־launch מספק רשימת תלמידים.
- להניח שה־launch מספק ציונים.
- להניח שה־launch מספק משימות.
- להציג Moodle API כפעיל רק בגלל שה־LTI הצליח.

---

## 9. Supabase / Backend

כאשר משתמשים ב־Supabase:

- secrets נשארים מחוץ לריפו.
- `.env` לעולם לא נכנס ל־GitHub.
- יש ליצור `.env.example` בלבד.
- Edge Functions משתמשות ב־service role רק בצד השרת.
- client לא מקבל service role.
- imported tables לא נקראות ישירות מהלקוח.
- RPCs / Edge Functions חייבים לבדוק session/context.

Edge Functions צפויות:

- `lti-launch`
- `lti-config`
- `import-moodle-report`
- `moodle-probe`
- `moodle-proxy`
- `site-admin`

אין להשתמש בנתיב `/api/...` בתוך frontend אם הפרויקט מבוסס Supabase Functions. יש להשתמש ב־`supabase.functions.invoke()` או URL מלא מתוך `VITE_SUPABASE_URL`.

---

## 10. Frontend stack

ה־stack הידוע מהחומרים שסופקו:

- Vite
- React 18
- TypeScript
- React Router
- TanStack React Query
- Supabase JS
- Tailwind CSS
- shadcn/Radix UI
- lucide-react
- xlsx
- Vitest

כל שינוי frontend חייב לשמור:

- RTL מלא.
- עברית מלאה.
- נגישות בסיסית.
- mobile-first סביר.
- ללא צבעים hardcoded ברכיבים.
- שימוש ב־CSS variables / Tailwind tokens.
- שימוש בפונטים Heebo / Assistant כאשר מוגדרים.

---

## 11. קבצים שאסור לשנות בלי סיבה חזקה

- `src/integrations/supabase/client.ts`
- `src/integrations/supabase/types.ts`
- קבצי migration ישנים שכבר הופעלו
- secrets או קבצי env

אם נדרש שינוי בקבצי auto-generated, חובה לתעד למה ולוודא שלא שוברים generation עתידי.

---

## 12. GitHub governance

כל שינוי אמיתי חייב לכלול:

- commit ברור.
- עדכון README אם ההתנהגות הציבורית השתנתה.
- עדכון `PROJECT_RULES.md` אם כלל/ארכיטקטורה השתנו.
- עדכון `STATE/project-status.md` אם סטטוס היכולת השתנה.
- תיעוד מה נבדק ומה לא נבדק.

אסור להעלות:

- `.env`
- service role key
- OAuth secrets
- Moodle secrets
- tokens
- קבצי backup עם secrets
- קבצי export של תלמידים אמיתיים

---

## 13. הגדרת Done

Feature נחשב Done רק אם:

1. קיים קוד אמיתי.
2. אין demo fallback.
3. יש empty state אמיתי.
4. יש טיפול בנתון חסר.
5. יש בדיקת build/typecheck או הסבר למה לא נבדק.
6. המסך/feature מתועד ב־README וב־PROJECT_RULES אם הוא ציבורי.
7. אין secrets בריפו.

אם אחד התנאים חסר — הסטטוס הוא partial או planned, לא done.

---

## 14. סטטוס אמת נוכחי

הפרויקט מכוון להיות:

**Pilot-ready על נתוני Moodle אמיתיים בייבוא ידני.**

אין לטעון production-ready עד שיתקיימו כל התנאים:

- ייבוא end-to-end נבדק עם קבצי Moodle אמיתיים.
- LTI launch אמיתי נבדק מול Moodle אמיתי.
- build/test עוברים בריפו הנוכחי.
- אין secrets.
- כל המסכים המרכזיים נבדקו עם empty state ועם נתונים.
- gaps ידועים מתועדים.

---

## 15. כלל אחרון

מה שלא נבדק — לא כותבים שהוא עובד.
מה שחסר — מוצג כחסר.
מה שלא קיים — מתועד כמתוכנן בלבד.
