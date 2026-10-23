# Daymark

**A private, all-in-one personal diary and life tracker** — daily tasks, a diary, workouts and meals, yearly goals, a library of your own books, expenses and a weekly summary. It works on desktop and phone, can be installed like an app, and keeps your data private to your account.

Live site: **https://daily.shivampal.tech**

> Built with React, TypeScript, Vite and Supabase. Hosted on Vercel.

---

## Table of contents

1. [What Daymark does](#1-what-daymark-does)
2. [Technology used (and why)](#2-technology-used-and-why)
3. [How it works](#3-how-it-works)
4. [Project structure](#4-project-structure)
5. [The database](#5-the-database)
6. [Set it up from scratch](#6-set-it-up-from-scratch)
7. [Run it on your computer](#7-run-it-on-your-computer)
8. [Put it online (Vercel + your own domain)](#8-put-it-online-vercel--your-own-domain)
9. [Install it on your phone](#9-install-it-on-your-phone)
10. [Security and privacy](#10-security-and-privacy)
11. [Free-plan limits](#11-free-plan-limits)
12. [Backup and restore](#12-backup-and-restore)
13. [Customize it](#13-customize-it)
14. [Troubleshooting](#14-troubleshooting)
15. [FAQ](#15-faq)
16. [Ideas for later](#16-ideas-for-later)

---

## 1. What Daymark does

### Today
- Add tasks for any date, with a **priority** (low, medium, high). Tick them off, remove them, filter by *All / To do / Done*.
- A **progress ring** and bar show how much of the day is finished.
- **Daily routine:** tick "Save this task to my daily routine" when adding a task. Then press **Add my daily routine** on any day to add all routine tasks at once (tasks already on that day are skipped).
- **Move unfinished to tomorrow** carries open tasks forward in one click.
- A **week strip** and ‹ › arrows let you jump to any day, past or future. A dot marks days with a diary entry.
- **Daily wellness:** "Did you work out today?" (with a note for what you did) and Breakfast / Lunch / Dinner checkboxes.
- **Mood** picker (5 faces) for each day.
- **Diary** with four questions (what I planned, what I completed, what I learned, what to improve). For a new day, the first two boxes are filled from that day's tasks. A *Tasks of the day* box lists what is done and open.
- **Diary streak** counter, a **goals-due-soon banner**, and a **Sunday banner** that links to the weekly summary.

### Review
- **Calendar** with colour that gets darker the more tasks you finished, a dot for days with a diary, and a red marker on goal deadlines. Click any date to open that day.
- **Open a date** box, **This week** bar chart and **Missed diary** shortcuts.
- **Day view** for any past day: tasks (completed / not finished), mood, workout and meals, and everything you wrote. *Edit this day* jumps to it on the Today page.
- **Habits panel:** workout streak, best streak, workouts in 30 days, full-meals streak, and 5-week grids.
- **Goal deadlines** list and an **All days** list with search and **Export .md**.

### Weekly summary
A one-page report for any week: tasks done, diary days, workout days, pages read, money spent, a day-by-day table, highlights in plain sentences, spending by category, and goals due that week. **Copy summary** puts the text on your clipboard.

### Goals
- Pick a **year** and write what you plan to achieve, with a **category**, notes, a **start date** and an **end date**.
- Counters: planned this year, completed, average progress, overdue.
- Each goal has a **progress bar**, a **days-left countdown**, a **time-used bar**, and an optional **checklist of steps** (progress then follows the steps). Without steps you can use −10% / +10% / Mark done.
- **Delete** any goal (asks to confirm).

### Books (library)
- Create **subjects** (Maths, Physics, Computer Science, anything) and add books inside them.
- Each book has a title, author, total pages, status (*Want to read / Reading / Finished*) and an optional **uploaded file** (PDF, text, image, up to 50 MB).
- **📖 Read** opens PDFs, text files and images right inside Daymark. Other types (for example EPUB or Word) can be downloaded.
- **🔖 Bookmark** your current page; pages read each day are logged for the weekly summary.
- **📝 Notes** per book: quotes, ideas, formulas, with an optional page number.
- Search, filter by subject, delete a book (its file is deleted too).

### Expenses
- Add an amount, category, date, payment method (UPI / Cash / Card / Other) and a note.
- Month-by-month totals, today's spend, daily average, change versus last month.
- **Monthly budget** with a bar that turns red if exceeded.
- **Monthly bills:** add rent, a recharge or a subscription once. It is added to your expenses automatically on its day each month, and the page shows how much is still to come this month.
- Spending **by category** (click to filter), transaction list, delete, **Export CSV**.

### Everywhere
- **Search** everything with `Ctrl + K` (or `⌘ K`, or the search icon): tasks, diary, goals, books, book notes, expenses.
- **Dark and light mode** (remembers your choice).
- **"Saved ✓" messages** for every save and delete, and clear red messages if something fails.
- **Mobile layout** with a bottom tab bar, slide-up sheets and large tap targets.
- **Installable** on a phone or computer (PWA), opens fast, and starts even when offline.
- **Backup and restore** as a single file.

---

## 2. Technology used (and why)

| Part | Technology | Why it is used |
|---|---|---|
| Interface | **React 19** | Builds the screens from small reusable components. |
| Language | **TypeScript 5.7** | Catches mistakes before the site runs. Strict mode is on. |
| Build tool | **Vite 6** | Fast development server and optimized production build. |
| Database | **Supabase** (PostgreSQL) | A real SQL database with a ready-made API. |
| Login | **Supabase Auth** | Email and password accounts, password reset, secure sessions. |
| File storage | **Supabase Storage** | A *private* bucket for uploaded books. |
| Privacy | **Row Level Security (RLS)** | The database itself makes sure you can only see your own rows. |
| Client library | **@supabase/supabase-js 2** | Lets the website talk to Supabase. |
| Hosting | **Vercel** | Serves the built website over HTTPS from a global network. |
| Domain | **Hostinger DNS** | Points `daily.shivampal.tech` at Vercel. |
| Styling | **Plain CSS** (one file) | Colour variables for light/dark, responsive rules, no framework. |
| Fonts | **Fraunces, DM Sans, DM Mono** (Google Fonts) | Headings, body text, small labels. |
| Installable app | **Web App Manifest + Service Worker** | Lets the site be installed and open offline. |
| Tests | **Vitest** | Unit tests for date, streak, goal and money logic. |
| Code quality | **ESLint + typescript-eslint** | Keeps code consistent. |

There is **no separate backend server**. The website talks straight to Supabase, and security is enforced inside the database.

---

## 3. How it works

```
        ┌──────────────────────────────────────────────┐
        │  Your phone or computer (the browser)        │
        │  Daymark = React single-page app             │
        └──────────────┬───────────────────────────────┘
                       │ downloaded once from
                       ▼
        ┌────────────────────────────┐
        │  Vercel (static hosting)   │   daily.shivampal.tech
        │  HTML, JS, CSS, icons      │
        └────────────────────────────┘

        The app then talks directly (HTTPS) to:

        ┌──────────────────────────────────────────────┐
        │  Supabase                                    │
        │   • Auth      – who you are                  │
        │   • Postgres  – tasks, diary, goals, …  + RLS│
        │   • Storage   – your uploaded book files     │
        └──────────────────────────────────────────────┘
```

1. You open the site. Vercel sends the app (a few static files).
2. You sign in. Supabase Auth gives your browser a session token.
3. Every read or write goes to Supabase with that token. RLS rules make Postgres return and accept **only rows whose `user_id` is yours**.
4. Book files are stored in a private bucket under a folder named with your user id. The app asks Supabase for a one-hour signed link when you press **Read**.

Small things that are kept **only in your browser** (`localStorage`) and do not sync between devices: theme (`daymark-theme`), mood per day (`daymark-mood-<date>`), and the monthly budget (`daymark-budget`).

---

## 4. Project structure

```
daymark/
├─ index.html                 App shell, phone/PWA meta tags
├─ package.json               Scripts and dependencies
├─ vite.config.ts             Vite configuration
├─ vercel.json                Makes every URL open the app (single-page app)
├─ tsconfig*.json             TypeScript settings (strict)
├─ eslint.config.js           Lint rules
├─ .env.example               Template for your Supabase keys
├─ .gitignore                 Keeps .env and node_modules out of Git
├─ public/                    Files served as they are
│  ├─ manifest.webmanifest    Name, colours and icons for installing
│  ├─ sw.js                   Service worker (fast start, offline)
│  └─ icon-*.png, icon.svg    App icons
├─ supabase/migrations/       Database setup, run in order
│  ├─ 20260101000000_daymark_schema.sql
│  ├─ 20260102000000_goals_books_expenses.sql
│  └─ 20260103000000_notes_recurring_log.sql
└─ src/
   ├─ main.tsx                Starts React, registers the service worker
   ├─ App.tsx                 Sign-in, layout/navigation, Today, Review, Settings
   ├─ Goals.tsx               Goals page
   ├─ Books.tsx               Library, reader and notes
   ├─ Expenses.tsx            Expenses and monthly bills
   ├─ Weekly.tsx              Weekly summary
   ├─ Habits.tsx              Workout/meal streaks (on Review)
   ├─ Wellness.tsx            Daily workout + meals card
   ├─ Search.tsx              Global search (Ctrl + K)
   ├─ Backup.tsx              Download and restore backups
   ├─ Install.tsx             "Install on your phone" card
   ├─ Icon.tsx                Small icon set
   ├─ ui.tsx                  Page title and the "Saved ✓" toast
   ├─ util.ts                 Date, streak, money and goal helpers
   ├─ util.test.ts            Unit tests
   ├─ styles.css              All styling (light, dark, mobile)
   └─ lib/
      ├─ supabase.ts          Creates the Supabase client from .env
      ├─ api.ts               All database and storage calls
      ├─ notify.ts            Toast messages and friendly error text
      ├─ planning.ts          Today's date and task progress
      └─ types.ts             Shared TypeScript types
```

---

## 5. The database

All tables live in your Supabase **PostgreSQL** database. Every table has a `user_id` column and a Row Level Security policy so each account only sees its own rows.

| Table | What it stores |
|---|---|
| `tasks` | Daily tasks: title, status, priority, due date, position |
| `daily_reflections` | The four diary answers for each day (one per user per date) |
| `daily_habits` | Workout and meal checks per day (`workout`, `breakfast`, `lunch`, `dinner`) |
| `routine_tasks` | Tasks you can add to any day with one click |
| `goals` | Title, notes, category, start and end date, progress |
| `goal_steps` | Checklist steps for a goal |
| `book_subjects` | Subjects such as Maths or Physics |
| `books` | Title, author, subject, status, pages, uploaded file info |
| `book_notes` | Notes and quotes per book |
| `reading_log` | Pages read per day (for the weekly summary) |
| `expenses` | Amount, category, date, method, note |
| `recurring_expenses` | Monthly bills that are added automatically |
| `workout_templates`, `meal_templates`, `daily_wellness_checks` | Created by the first migration; no longer used by the screens |

**Storage:** a **private** bucket named `books` (50 MB per file). Files are saved as `<user-id>/<timestamp>-<filename>`, and policies allow a user to read, add and delete only inside their own folder.

### Migrations (run once, in this order)

| # | File | Adds |
|---|---|---|
| 1 | `20260101000000_daymark_schema.sql` | Core tables and security rules |
| 2 | `20260102000000_goals_books_expenses.sql` | Daily habits, goal steps, book subjects and file storage, expenses, daily routine |
| 3 | `20260103000000_notes_recurring_log.sql` | Book notes, reading log, monthly bills |

Every file is safe to run again. They only add what is missing.

---

## 6. Set it up from scratch

You need free accounts at **GitHub**, **Supabase** and **Vercel**, plus **Node.js 20 or newer** on your computer (https://nodejs.org).

### 6.1 Create the Supabase project
1. Go to https://supabase.com → **New project**. Choose a name, a strong database password and the region nearest you.
2. Open **Project Settings → API** and note two values:
   - **Project URL**
   - **Publishable / anon key** (the *public* key)
   
   Never use the `service_role` key in this project.

### 6.2 Turn on email sign-in
**Authentication → Providers → Email** must be enabled. Then open **Authentication → URL Configuration**:
- **Site URL:** `https://daily.shivampal.tech` (use `http://localhost:5173` while testing locally)
- **Redirect URLs:** add `https://daily.shivampal.tech/**` and `http://localhost:5173/**`

### 6.3 Create the tables
1. Open **SQL Editor** and click **+** for a new query.
2. Paste the contents of migration **1**, click **Run**, then do the same for **2** and **3**.
3. If Supabase shows *"Potential issues detected"*, click **Run and enable RLS**. The "destructive" warning only refers to replacing old access rules on these tables; no data is deleted.
4. You should see *Success. No rows returned* each time.

### 6.4 Add your keys to the project
Copy `.env.example` to `.env` and fill it in:

```
VITE_SUPABASE_URL=https://YOUR-PROJECT.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_PUBLISHABLE_ANON_KEY
```

If these are missing or still contain placeholders, the app shows a "Connect your private diary" screen instead of the login. Restart the dev server after editing `.env`.

---

## 7. Run it on your computer

```bash
npm install
npm run dev
```

Open http://localhost:5173, create an account (check your email to confirm) and sign in.

| Command | What it does |
|---|---|
| `npm run dev` | Starts the development server with live reload |
| `npm run build` | Type-checks, then builds the production site into `dist/` |
| `npm run preview` | Serves the built site locally to test it |
| `npm test` | Runs the unit tests once |
| `npm run test:watch` | Re-runs tests when files change |
| `npm run lint` | Checks code style |

> **Windows tip:** keep the project in a normal folder such as `C:\projects\daymark`. Folders inside OneDrive can slow down or break `npm install` because OneDrive tries to sync thousands of files.

---

## 8. Put it online (Vercel + your own domain)

### Which database should I use for hosting?
**Keep Supabase.** Vercel only hosts the website's files. Your data stays in Supabase, so there is nothing to move and no second database to set up.

### 8.1 Upload the code to GitHub
Create a new repository and upload the project folder (or use `git push`). The included `.gitignore` keeps `.env` and `node_modules` out of the repository. **Never commit `.env`.**

### 8.2 Deploy on Vercel
1. Sign in at https://vercel.com with GitHub → **Add New → Project** → choose the repository.
2. Vercel detects **Vite** automatically (build command `npm run build`, output `dist`).
3. Open **Environment Variables** and add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` with your values.
4. Click **Deploy**. You get a temporary `*.vercel.app` address.

`vercel.json` already contains the rule that sends every URL to `index.html`, so refreshing any page works.

### 8.3 Connect `daily.shivampal.tech`
1. In Vercel: **Project → Settings → Domains → Add** `daily.shivampal.tech`. Vercel shows the DNS record to create (normally a **CNAME**).
2. In **Hostinger → Domains → shivampal.tech → DNS records**, add:
   - **Type:** CNAME
   - **Name:** `daily`
   - **Target:** exactly the value Vercel shows you
   - Delete any existing record named `daily` first.
3. Wait a few minutes up to an hour. Vercel marks the domain valid and enables HTTPS automatically.
4. Make sure the **Supabase Site URL and Redirect URLs** from step 6.2 contain your final `https://daily.shivampal.tech` address.

Adding a subdomain does **not** affect your main website on the root domain.

### 8.4 Updating later
Push changes to GitHub and Vercel redeploys on its own. If you change an environment variable, press **Redeploy** in Vercel.

---

## 9. Install it on your phone

Open the live site on the phone, then:
- **Android (Chrome):** menu ⋮ → **Install app**
- **iPhone (Safari):** Share → **Add to Home Screen**
- **Computer (Chrome/Edge):** click the install icon in the address bar

Settings also has an **Install Daymark** button when the browser allows it. This works on the deployed https site, not on `localhost`. The service worker only caches the site's own files and **never caches your data**.

---

## 10. Security and privacy

- **The browser only has the public (anon) key.** That key is meant to be public. What protects your data is **Row Level Security** inside the database.
- **Never put the `service_role` key** in `.env`, in Vercel's `VITE_*` variables or in any frontend file.
- **Every data table has RLS on** and a policy `user_id = auth.uid()`.
- **Books are private.** The bucket is not public; links expire after one hour.
- Keep `.env` out of Git. If a key is ever exposed, rotate it in **Project Settings → API**.

### Check your security after changes (use two test accounts)

| Test | Expected result |
|---|---|
| Not signed in, read `tasks` | Denied or no rows |
| User A reads or edits User B's row by id | Denied or nothing changes |
| User A inserts a row with User B's `user_id` | Rejected by RLS |
| User A opens User B's book file | Denied |

---

## 11. Free-plan limits

Approximate limits at the time of writing. Plans change, so check **Supabase → Settings → Usage** and the Supabase and Vercel pricing pages.

| Resource | Supabase Free |
|---|---|
| Database size | about 500 MB (text uses very little; lasts years) |
| File storage (your books) | about 1 GB total, 50 MB per file |
| Download traffic | about 5 GB a month |
| Inactivity | A free project may **pause after about a week with no activity**. Resume it from the dashboard; data is kept. |

Tips: compress large PDFs, or upgrade to Supabase Pro for much more storage.

---

## 12. Backup and restore

**Settings → Backup & restore**
- **Download backup** saves one JSON file with your tasks, diary, goals, steps, subjects, books info, notes, reading log, expenses, monthly bills, habits and routine.
- **Restore from file** brings it back. Existing rows with the same id are updated; nothing is deleted.
- **Uploaded book files are not inside the backup.** They stay in Supabase Storage.

Also available: **Export .md** (diary, in Review) and **Export CSV** (expenses).

---

## 13. Customize it

| Want to change | Where |
|---|---|
| The name in the greeting ("Good morning, Shivam") | `src/App.tsx`, search for `Shivam` |
| Colours, dark mode, fonts | `:root` variables at the top of `src/styles.css` |
| Currency | `money` in `src/util.ts` (currently ₹, Indian number format) |
| Expense categories and payment methods | `CATS` and `METHODS` in `src/Expenses.tsx` |
| Goal categories | `CATS` in `src/Goals.tsx` |
| Quick-start book subjects | `SUGGEST` in `src/Books.tsx` |
| Mood faces and meals tracked | `MOODS` and `HABITS` in `src/util.ts` |
| Max book upload size | `MAX_MB` in `src/Books.tsx` **and** `file_size_limit` in migration 2 |
| App name and icons | `public/manifest.webmanifest`, `public/icon-*.png` |

---

## 14. Troubleshooting

| Problem | Fix |
|---|---|
| `Failed to resolve import "./lib/api"` | The `src/lib` folder is missing. Use the complete project. |
| `Could not read package.json` | You are in the wrong folder. `cd` into the folder that contains `package.json`. |
| `npm install` is very slow or fails | Move the project out of OneDrive (for example `C:\projects\daymark`). |
| "Connect your private diary" screen | `.env` is missing or has placeholder values. Fix it and restart `npm run dev`. |
| "relation … does not exist" / "schema cache" | A migration has not been run. Run all three SQL files in order. |
| "new row violates row-level security" | You are signed out, or a migration was skipped. Sign in again and re-run the migrations. |
| A goal or book will not save | The red message shows the database's reason. A "check constraint" message means your old table does not allow that value. |
| No confirmation email | Check spam. In Supabase → Authentication, make sure Email is enabled and the Site URL is correct. |
| Email links open the wrong address | Fix the **Site URL** and **Redirect URLs** in Supabase. |
| Book will not open in the reader | PDFs, text and images open in the app. EPUB and Word files can only be downloaded. |
| Domain shows "Invalid configuration" in Vercel | The CNAME is missing or wrong. Use exactly the value Vercel shows. DNS can take up to an hour. |
| Website shows old content after an update | Hard refresh (`Ctrl + Shift + R`). The installed app updates the next time it opens online. |
| SQL Editor opens an old query | Click **+** to start a new empty query before pasting. |
| Free project paused | Open the Supabase dashboard and click **Restore**. |

---

## 15. FAQ

**Do I need a different database to host it?**  
No. Supabase stays your database. Vercel only serves the website.

**Is my data private?**  
Yes. Each account can read and write only its own rows, and book files are in a private bucket. Still, keep your Supabase password safe and your `.env` out of Git.

**Can I use it on several devices?**  
Yes, sign in with the same account. Tasks, diary, goals, books, notes and expenses sync. Theme, mood and the budget are per browser.

**Can someone else use it?**  
Yes. Anyone can create an account, and they see only their own data.

**What if I run out of book storage?**  
Compress your PDFs or upgrade the Supabase plan.

---

## 16. Ideas for later

- Save mood and the monthly budget in the database so they sync across devices
- Reminders (email or push) for tasks and goals
- Charts for long-term spending and reading
- Reading goals ("read 20 pages a day")
- Tags for tasks, and shared goals with a friend

---

Made by Shivam Pal. Built with React, TypeScript, Vite and Supabase.
