# Project Status — Moodle Teacher Hub

עדכון סטטוס אמת לריפו `yanivmizrachiy/moodle-teacher-hub`.

---

## סטטוס כללי

הריפו קיים בגיטהאב.

התווספה שכבת governance חדשה:

- `PROJECT_RULES.md`
- `docs/system-rules.md`
- `docs/repository-map.md`
- `STATE/project-status.md`

המטרה של השכבה הזו היא למנוע סתירות, להפריד בין מה שעובד למה שמתוכנן, ולוודא שכל המשך עבודה על Lovable/Moodle Teacher Hub נשמר בצורה מקצועית.

---

## מה ידוע מהחומרים שסופקו

המשתמש סיפק חומרים מתוך Lovable ומהפרויקט:

- `index.html`
- `Index.tsx`
- `package.json`
- `postcss.config.js`
- `tailwind.config.ts`
- `tsconfig.json`
- README/אפיון מוצרי מפורט
- צילום Dashboard
- צילום מבנה Code View ב־Lovable
- דוח טכני קודם על מסכים, RPCs, imports ו־Supabase

---

## סטאק ידוע

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
- Testing Library
- ESLint

---

## מצב יכולות לפי הידע הנוכחי

| יכולת | סטטוס |
|---|---|
| Hebrew RTL UI | known |
| Dashboard | known from screenshot and docs |
| Import Wizard | known from docs/report |
| Manual Moodle data import | intended/known from docs |
| LTI launch | described as active, requires repo-code verification |
| Supabase Edge Functions | known from docs/report, requires code verification |
| RPC data layer | known from docs/report, requires code verification |
| Live Moodle WS sync | blocked — no token |
| Write-back to Moodle | blocked — no token |
| Demo data | forbidden |
| Production readiness | not verified |

---

## מה עדיין חסר כדי להגיע ל־100% אימות

חסרים קבצי הליבה המלאים:

- `src/App.tsx`
- `src/main.tsx`
- `src/pages/Dashboard.tsx`
- `src/pages/Import.tsx`
- `src/pages/Students.tsx`
- `src/pages/ActivityPage.tsx`
- `src/hooks/useImports.tsx`
- `src/lib/moodleImport.ts`
- `src/lib/dataAdapters/*`
- `src/integrations/supabase/client.ts`
- `src/index.css`
- `supabase/functions/lti-launch/index.ts`
- `supabase/functions/import-moodle-report/index.ts`
- `supabase/migrations/*.sql`

---

## כלל סטטוס

עד שכל קבצי הליבה ייבדקו בפועל, אין לסמן את המערכת כ־production-ready.

הסטטוס הנכון כרגע:

```text
Repository governance ready.
Lovable project understanding: high but not complete.
Codebase verification: partial.
Production readiness: not verified.
```
