# Data — Lynx Studio Ashkelon

נתונים תפעוליים שהסוכנים (רוני, אורי, שירה) קוראים. מה כל קובץ מכיל ואיך הוא מתעדכן.

## members.csv — CRM מנויים ⚠️ gitignored — PII

עמודות:

- `id` — מזהה ייחודי
- `name` — שם מלא
- `email`
- `phone`
- `join_date` — YYYY-MM-DD
- `plan_type` — `monthly` / `quarterly` / `annual` / `trial`
- `monthly_fee` — ₪
- `status` — `active` / `inactive` / `trial` / `frozen`
- `last_attendance` — YYYY-MM-DD

**עדכון**: ייצוא מהתוכנה הייעודית פעם בשבוע (לפחות).

**שימוש**: שירה (לכל פנייה), רוני (חישוב MRR), אורי (זיהוי העדפות, אופציונלי).

## schedule.csv — לוח שיעורים שבועי

עמודות:

- `day` — Sunday..Saturday
- `time_slot` — HH:MM (start time)
- `class_type` — מתוך `ori/class-catalog.md`
- `instructor` — מתוך `ori/instructors.md`
- `room` — שם האולם
- `capacity` — מספר המקומות

**עדכון**: שבועי / לפי שינויים.

**שימוש**: אורי (תכנון), שירה (להגיד למנוי מתי שיעור).

## budget.csv — תקציב חודשי ⚠️ gitignored — financial

עמודות:

- `month` — YYYY-MM
- `category` — `rent` / `salaries` / `equipment` / `marketing` / `utilities` / `software` / `misc`
- `planned` — ₪ מתוכנן
- `actual` — ₪ בפועל

**שימוש**: רוני בעיקר.

## revenue.csv — הכנסות חודשיות ⚠️ gitignored — financial

עמודות:

- `month` — YYYY-MM
- `mrr` — Monthly Recurring Revenue (₪)
- `new_members` — מספר מנויים חדשים בחודש
- `churned_members` — מספר מנויים שעזבו
- `refunds` — ₪ של החזרים שניתנו

**שימוש**: רוני בעיקר.

---

## הערה על PII ובטיחות

הקבצים `members.csv`, `budget.csv`, ו-`revenue.csv` מכילים מידע רגיש (PII של מנויים, נתונים פיננסיים). הם **gitignored** ולא יעלו ל-GitHub. ראה `.gitignore` בשורש.

`schedule.csv` כן יכול להיות ב-git — לוח שיעורים לא רגיש.

אם מוסיפים קבצים חדשים עם PII — להוסיף שורה תואמת ל-`.gitignore`, או להשתמש בסיומת `.private.csv` שגם נכנסת לדפוס שב-`.gitignore`.

---

## דוגמאות (`.example` files)

לכל קובץ יש `<name>.csv.example` עם header בלבד. שכפלו, מלאו, ושמרו כשם בלי `.example` — אז יישמרו gitignored אוטומטית עבור members / budget / revenue.
