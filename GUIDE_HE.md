# מדריך שימוש — מערכת סוכני הפיתוח

## מה זה?

מערכת של 11 סוכני AI מומחים שעובדים יחד כצוות פיתוח מלא ב-Claude Code. לכל סוכן יש תפקיד, כלים, הרשאות, וזיכרון מתמשך.

## הסוכנים

| סוכן | תפקיד | מודל | הרשאות |
|---|---|---|---|
| **Lead Orchestrator** | מנתב משימות לסוכן המתאים | opus | מלאות |
| `architect` | ארכיטקטורה, עיצוב מערכת, סקלביליות | opus | קריאה בלבד (מייעץ) |
| `frontend-developer` | לוגיקת React, state, routing, hooks | sonnet | קריאה + כתיבה |
| `ui-designer` | עיצוב ויזואלי, TailwindCSS, קומפוננטות | sonnet | קריאה + כתיבה |
| `backend-developer` | API routes, middleware, Express | sonnet | קריאה + כתיבה |
| `database-expert` | סכמות PostgreSQL, מיגרציות, queries | sonnet | קריאה + כתיבה |
| `devops-engineer` | CI/CD, Docker, deployment | sonnet | קריאה + כתיבה |
| `code-reviewer` | ביקורת קוד, best practices | opus | קריאה בלבד (דוחות) |
| `security-analyst` | ביקורת אבטחה, OWASP, פרצות | sonnet | קריאה בלבד (דוחות) |
| `performance-optimizer` | פרופיילינג, אופטימיזציה, caching | sonnet | קריאה + כתיבה |
| `qa-expert` | בדיקות, ציד באגים, כיסוי בדיקות | sonnet | קריאה בלבד (דוחות) |
| `tech-writer` | תיעוד API, README, JSDoc, מדריכים | sonnet | קריאה + כתיבה |

**סוכנים עם קריאה בלבד** (architect, code-reviewer, security-analyst, qa-expert) מייעצים ומדווחים — הם יכולים לכתוב רק לדוחות ביקורת ולקבצי הזיכרון שלהם.

## התחלה מהירה

### אפשרות 1: פרויקט חדש (GitHub Template)

1. לחץ על **"Use this template"** ב-GitHub כדי ליצור ריפו חדש
2. שכפל ופתח ב-VSCode
3. הקלד `/init-project` בצ'אט של Claude — המערכת מזהה אוטומטית את הסטאק שלך ומגדירה הכל
4. התחל לעבוד — הסוכנים מוכנים

### אפשרות 2: הוספה לפרויקט קיים (Git Subtree — מומלץ)

שיטה זו מאפשרת למשוך עדכונים עתידיים לסוכנים.

**פעם ראשונה — הוספת סוכנים לפרויקט:**

```bash
# הוסף את הריפו כ-remote
git remote add agents https://github.com/razielamir1/LeadOrchestratorAgent.git
git fetch agents

# משוך את תיקיית .claude לפרויקט
git subtree add --prefix=.claude agents main --squash
```

פתח את הפרויקט ב-VSCode והקלד `/init-project` — המערכת תזהה אוטומטית את הסטאק ותייצר `CLAUDE.md`.

**בעתיד — משיכת עדכונים לסוכנים:**

```bash
git subtree pull --prefix=.claude agents main --squash
```

### אפשרות 3: העתקה ידנית

```bash
cp -r /path/to/LeadOrchestratorAgent/.claude /path/to/your-project/
```

ואז הקלד `/init-project` בצ'אט של Claude.

> שים לב: העתקה ידנית לא תומכת במשיכת עדכונים עתידיים.

## פקודות מהירות (Slash Commands)

| פקודה | מה עושה |
|---|---|
| `/init-project` | מזהה אוטומטית את הסטאק ומייצרת `CLAUDE.md` מותאם |
| `/full-audit` | מריצה 4 סוכני ביקורת במקביל → ארכיטקט מסכם ל-`FIXES.md` |
| `/new-feature <תיאור>` | Pipeline מלא: architect → db → backend → frontend → ui → qa |
| `/fix-bug <תיאור>` | Pipeline אבחון: qa → תיקון → qa אימות |
| `/security-check` | סריקת אבטחה מלאה עם דוח |
| `/document <סוג>` | מייצר תיעוד (api / readme / onboarding / jsdoc) |

## איך להשתמש

### האצלה אוטומטית
פשוט תאר מה אתה צריך. האורקסטרטור קורא את התיאור של כל סוכן ומאציל אוטומטית:

```
"תבנה לי טופס התחברות עם ולידציה"
→ frontend-developer (לוגיקה) + ui-designer (עיצוב) + qa-expert (בדיקות)
```

### הפעלה ישירה (@-mention)
פנה ישירות לסוכן ספציפי:

```
@architect תתכנן את הארכיטקטורה למערכת התראות בזמן אמת
@code-reviewer תעבור על הקוד ב-src/api/users.ts
@security-analyst תסרוק את הפרויקט לפרצות אבטחה
```

### שרשור (Chaining) — לפיצ'ר full-stack
הסוכנים רצים ברצף:

```
architect → database-expert → backend-developer → frontend-developer → ui-designer → qa-expert
```

או פשוט הקלד `/new-feature <תיאור>` והכל רץ אוטומטית.

### עבודה במקביל (Parallel)
משימות עצמאיות רצות בו-זמנית:

```
"תריץ את @code-reviewer ו-@security-analyst על src/ במקביל"
```

או הקלד `/full-audit` להרצת כל 4 סוכני הביקורת במקביל עם דוחות אוטומטיים.

### הרצה ברקע
הרץ סוכנים ברקע בזמן שאתה ממשיך לעבוד:

```
"תריץ את @qa-expert ברקע לסרוק את כל הפרויקט"
```

אפשר גם ללחוץ **Ctrl+B** כדי להעביר משימה שכבר רצה לרקע.

### צפייה בכל הסוכנים
הקלד `/agents` בצ'אט כדי לראות תפריט אינטראקטיבי של כל הסוכנים הזמינים.

## מערכת בטיחות

### Micro-Checkpoint Protocol
סוכנים עם הרשאות כתיבה חייבים **לעצור ולהציג תוכנית פעולה** לפני ביצוע שינויים שמערבים:
- יצירה או שינוי של יותר מ-3 קבצים
- מיגרציות או שינויי סכמה
- קבצי קונפיגורציה (package.json, tsconfig, Dockerfile, CI/CD)
- לוגיקת אימות או הרשאות
- מחיקת קבצים או הסרת פונקציונליות

המשתמש חייב לכתוב **PROCEED** כדי שהסוכן ימשיך.

### Safety Hooks (הגנה קשיחה)
Hook מסוג `PreToolUse` ב-`settings.json` חוסם פעולות מסוכנות ברמת המערכת:

| פעולה חסומה | דוגמה |
|---|---|
| SQL הרסני | `DROP TABLE`, `TRUNCATE`, `DELETE FROM` |
| מחיקה רקורסיבית | `rm -rf` |
| פעולות כוח | `--force`, `--hard` |

הסוכן מקבל הודעת שגיאה ולא יכול להמשיך — זו הגנה טכנית, לא רק הנחיה.

## מערכת דוחות ביקורת

סוכני הביקורת כותבים את הממצאים שלהם ל-`.claude/audits/` במקום להציף את השיחה:

| סוכן | קובץ דוח |
|---|---|
| `qa-expert` | `AUDIT_QA.md` |
| `security-analyst` | `AUDIT_SECURITY.md` |
| `code-reviewer` | `AUDIT_CODE_REVIEW.md` |
| `performance-optimizer` | `AUDIT_PERFORMANCE.md` |

ה-`architect` יכול לסכם את כל הדוחות ל-`FIXES.md` מתועדף. דוחות הביקורת לא עולים ל-Git (דרך `.gitignore`).

## תזרימי עבודה מומלצים

### פיצ'ר חדש (Full-Stack)
`architect` → `database-expert` → `backend-developer` → `frontend-developer` → `ui-designer` → `qa-expert`
> קיצור: `/new-feature <תיאור>`

### תיקון באג
`qa-expert` (אבחון) → סוכן פיתוח מתאים (תיקון) → `qa-expert` (אימות)
> קיצור: `/fix-bug <תיאור>`

### ביקורת מלאה (במקביל)
`code-reviewer` + `security-analyst` + `qa-expert` + `performance-optimizer` → `architect` (מסכם ל-FIXES.md)
> קיצור: `/full-audit`

### פריסה (Deployment)
`devops-engineer` → `qa-expert` (אימות) → פריסה

### תיעוד
`tech-writer` → קורא את הקוד → מייצר תיעוד
> קיצור: `/document <סוג>`

## זיכרון מתמשך

כל סוכן מתחזק קובץ זיכרון משלו ב-`.claude/agent-memory/<שם-הסוכן>/MEMORY.md`. הסוכנים קוראים את הזיכרון שלהם בתחילת כל משימה ומעדכנים אותו בסיום.

הסוכנים **לומדים לאורך זמן** — סוכן ה-QA זוכר אזורים שבירים, הארכיטקט זוכר החלטות עיצוב קודמות, מומחה הבסיס נתונים זוכר היסטוריית סכמות.

הזיכרון הוא **לכל פרויקט בנפרד**, אז הסוכנים לומדים כל פרויקט באופן עצמאי.

## יעילות הקשר (Context Efficiency)

תת-סוכנים רצים בסביבות מבודדות — העיבוד הכבד שלהם נשאר בסשן שלהם ורק סיכום נקי חוזר לשיחה הראשית. זה מונע "ריקבון הקשר" וחוסך עלויות.

- העדף האצלה לסוכנים על פני עבודה ישירה
- השתמש ב-`/full-audit` במקום לפנות לכל סוכן ביקורת בנפרד
- לפרויקטים גדולים, כוון לתיקיות ספציפיות (למשל, "audit src/api/")

## חיבור כלים חיצוניים (MCP)

חבר את הסוכנים למערכות חיצוניות כמו PostgreSQL, GitHub, Slack ועוד. ראה [MCP_SETUP.md](MCP_SETUP.md) להוראות מפורטות.

## התאמה אישית

- **זיהוי סטאק אוטומטי:** הקלד `/init-project` לסריקה ויצירת `CLAUDE.md` מותאם
- **הוספת סוכן חדש:** צור קובץ `.md` חדש ב-`.claude/agents/` עם YAML frontmatter ופרומפט
- **שינוי סוכן קיים:** ערוך את קובץ ה-`.md` שלו ב-`.claude/agents/`
- **שינוי מודל:** קבע `model: opus` לחשיבה עמוקה או `model: sonnet` לתגובות מהירות
- **הגבלת הרשאות:** הסר `Write` ו-`Edit` מהשדה `tools` כדי להפוך סוכן לקריאה בלבד
- **הוספת שרתי MCP:** ערוך `.claude/settings.json` — ראה [MCP_SETUP.md](MCP_SETUP.md)
- **הוספת פקודות:** צור קובץ `.md` חדש ב-`.claude/commands/`
