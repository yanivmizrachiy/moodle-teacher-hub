# Repository Map — Moodle Teacher Hub

מסמך זה מתכנן את מבנה הריפו כך שיהיה ברור, יציב, וללא סתירות.

---

## מבנה יעד

```text
moodle-teacher-hub/
  PROJECT_RULES.md              # מקור אמת עליון
  README.md                     # תיאור קצר, הפעלה, סטטוס ציבורי
  .gitignore                    # מניעת secrets ו־artifacts
  .env.example                  # שמות משתני סביבה בלבד, בלי ערכים סודיים
  package.json                  # scripts ותלויות frontend
  index.html                    # entry HTML, RTL
  src/                          # אפליקציית React
    main.tsx
    App.tsx
    index.css
    components/                 # רכיבי UI כלליים
    hooks/                      # hooks, React Query, RPC wrappers
    integrations/supabase/      # client/types auto-generated
    lib/
      dataAdapters/             # ManualImportAdapter + MoodleWsAdapter
      moodleImport.ts           # parsing/detection/mapping
    pages/                      # routes
  supabase/
    functions/                  # Edge Functions
    migrations/                 # SQL migrations
  docs/
    system-rules.md             # כללי עבודה מעשיים
    repository-map.md           # המסמך הזה
    import-contract.md          # חוזה ייבוא נתונים
    lti-contract.md             # חוזה LTI
    testing-plan.md             # בדיקות חובה
  STATE/
    project-status.md           # סטטוס אמת עדכני
    evidence-log.md             # הוכחות בדיקה
```

---

## עקרון מקור אמת

- `PROJECT_RULES.md` — חוקים מחייבים.
- `README.md` — מה משתמש/מפתח צריך לדעת מהר.
- `docs/*` — פירוט מקצועי.
- `STATE/*` — סטטוס ומה באמת אומת.

אם README אומר שמשהו עובד אבל `STATE/project-status.md` אומר שלא נבדק — הסטטוס הנכון הוא לא נבדק.

---

## אזורי קוד

### Frontend

האפליקציה היא React/Vite/TypeScript בעברית RTL.

אחריות:

- routes
- layout
- sidebar
- screens
- import wizard
- reports
- empty states

### Data layer

אחריות:

- adapters
- hooks
- RPC wrappers
- normalized model
- no direct page access to imported tables

### Supabase

אחריות:

- LTI launch
- import write path
- RPC/session validation
- future Moodle API adapters

---

## מסמכי docs מתוכננים

| קובץ | תפקיד | סטטוס |
|---|---|---|
| `docs/system-rules.md` | כללי פיתוח מעשיים | קיים |
| `docs/repository-map.md` | מפת ריפו | קיים |
| `docs/import-contract.md` | חוזה ייבוא Moodle | מתוכנן |
| `docs/lti-contract.md` | חוזה LTI/OAuth/session | מתוכנן |
| `docs/testing-plan.md` | בדיקות build/import/LTI/UI | מתוכנן |

---

## STATE מתוכנן

| קובץ | תפקיד |
|---|---|
| `STATE/project-status.md` | סטטוס אמת עדכני |
| `STATE/evidence-log.md` | לוג הוכחות: מה נבדק, מתי, איך |
| `STATE/lovable-intake.md` | מה התקבל מ־Lovable ומה עדיין חסר |

---

## כללי אי־סתירה

1. אין לתעד feature כ־done אם אין קוד.
2. אין לתעד feature כ־production אם לא נבדק end-to-end.
3. אין להשאיר docs ישנים שסותרים את `PROJECT_RULES.md`.
4. אין לשלב localStorage-only ו־Supabase-only באותו תיאור בלי לציין מה המצב האמיתי.
5. אם יש מעבר ארכיטקטורה — חייבים לתעד אותו במפורש.

---

## מצב הריפו כרגע

הריפו קיים בגיטהאב תחת:

```text
yanivmizrachiy/moodle-teacher-hub
```

נמצאו README ו־.gitignore קיימים. נוספו מסמכי governance כדי להפוך את הריפו למקור אמת חכם ומסודר.
