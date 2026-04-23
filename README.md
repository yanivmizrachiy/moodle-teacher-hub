# Moodle LTI Production Skeleton

שלד אמיתי לשרת כלי LTI 1.3 עבור Moodle.

מה יש כאן:
- שרת Express בסיסי
- מסלולי health, home, login, launch
- בדיקת launch בסיסית
- שמירת session בסיסית
- דף בית בעברית עם לוח בקרה
- מקום ברור לחיבור למסד נתונים, NRPS, AGS ו-Deep Linking

מה עוד חסר כדי לעלות ל-production:
- אימות JWT/JWKS מלא
- רישום הכלי ב-Moodle
- דומיין ציבורי עם HTTPS
- מסד נתונים קבוע
- שכבת משתמשים/מרחבים אמיתית
- NRPS / AGS / Deep Linking אמיתיים

הפעלה:
1. העתק את .env.example ל-.env
2. מלא את הערכים האמיתיים
3. התקן:
   npm install
4. הפעל:
   npm run dev
