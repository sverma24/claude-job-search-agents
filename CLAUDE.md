# Job Search Agents

This folder holds a team of AI agents helping one person (the user, whose details are in `resume.md`) run a job search. Each agent is a Claude Code skill in `.claude/skills/`.

## Source files (written by the user)
- `settings.md` — country, timezone, city and work style, currency and pay basis, salary floor and target, resume paper, where to report scams. **Every agent reads it first.**
- `resume.md` — resume
- `wins.md` — notes on best work, plus what the user wants next and known weak spots

## The profile folder: `profile/`
`profile/` is the shared memory for every agent. **Before doing any task, read `profile/README.md` and the files it points to that are relevant.** Do not re-derive the user's background from scratch if it is already there.

| File | Contents |
|---|---|
| `profile/facts.md` | Verified facts extracted from the sources: roles, dates, skills, numbers, stated preferences. **Source of truth; wins if files conflict.** |
| `profile/target-roles.md` | The 3 target job titles and why |
| `profile/target-companies.md` | 15 target companies with reasons |
| `profile/interview-stories.md` | Wins written as interview stories |
| `profile/linkedin.md` | Headline options, About section, post ideas |
| `profile/gaps.md` | Missing information and open questions for the user |

`jobs/` holds the Job Scout's output: dated reports (`YYYY-MM-DD-job-report.md`) and `seen.md`, the list of jobs already reported (used so no job is counted twice).

`applications/` holds the Application Kit's output: one dated folder per job (tailored resume as `.md` and print-ready `.html`, cover letter, keyword check, job post extract). Kits are tracked in `tracker.md`.

`companies/` holds the Company Brief's output: one dated folder per company (`brief.md` one-page brief, `first-90-days.html` 5-slide deck, `sources.md`).

If `profile/` doesn't exist or is stale, tell the user to run **"map my career"** (the `career-map` skill) first.

Agents may add new files to `profile/` and should list them in `profile/README.md`. Don't delete or overwrite another agent's files unless asked.

## Rules for every agent
1. **Never invent** experience, numbers, skills, titles, dates or preferences. Everything about the user must trace to `resume.md`, `wins.md`, `profile/`, or what the user says in chat.
2. **Tell the user when something important is missing** instead of guessing; record open questions in `profile/gaps.md`.
3. Quote numbers exactly as they appear in the sources. Don't round up or inflate.
4. Mark anything about outside companies or jobs that you haven't verified as unverified.
5. **Don't send, post, submit or apply** anything on the user's behalf without explicit approval in chat. Drafts only.
6. Respect the user's settings and preferences (`settings.md`, then target roles and industries in `profile/`). Use the timezone in `settings.md` for every date. When something looks like a job scam, tell the user where to report it (the place in `settings.md`) and never engage with the sender.

`outreach/` holds the Intro Writer's output: one dated folder per company (`intros.md`, `sources.md`). Drafts and follow-up dates are tracked in `tracker.md`.

`interviews/` holds the Interview Coach's output: one dated file per mock interview (`YYYY-MM-DD-company-job-title.md`, with questions, the user's answers, scores, feedback and top 3 fixes). Sessions are tracked in `tracker.md`.

`offers/` holds the Offer Coach's output: one dated folder per offer (offer summary, market research with links, value and comparison, negotiation plan, counter-offer email draft, phone script, practice session). Offers are tracked in `tracker.md`.

`manager/` holds the Job Search Manager's dated morning summaries and weekly reviews.

`.claude/agents/` holds the manager's worker subagents: `kit-builder` (runs the `application-kit` skill for one job) and `intro-drafter` (runs the `intro-writer` skill for one company). They run in parallel, so they never edit shared files (`tracker.md`, `profile/README.md`, `profile/gaps.md`); they return those lines to the manager, which adds them.

## The tracker: `tracker.md` (the only tracker)
`tracker.md` in the project root is the **one and only tracker**. Every agent records its work there. **Don't create other tracker files** (no `applications/tracker.md`, `outreach/tracker.md`, `interviews/tracker.md`, `offers/tracker.md`). Create `tracker.md` with this layout if it's missing:

```
# Job Search Tracker
Last updated: YYYY-MM-DD

## Jobs
| ID | Company | Title | Score | Safety | Job URL | Found | Status | Status date | Next action | Due | Files | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|

## Outreach
| Job ID | Company | Audience | Folder | First message sent | Follow-up 1 (day 5) | Follow-up 2 (day 12) | Status |
|---|---|---|---|---|---|---|---|

## History
| Date | ID | Change | Source |
|---|---|---|---|
```

How to update it:
- **One row per job** in Jobs. Find the job by URL, or company + title + location. If it isn't there, add a row with the next ID (`J001`, `J002`, ...; never reused). Update rows in place; never delete rows or history.
- **Status** is exactly one of `new`, `kit ready`, `applied`, `interview`, `offer`, `closed`. **Only the user can make a job `applied` or `interview`**, by saying so in chat. Never move a job backwards (e.g. don't set `kit ready` on a job that is `applied`).
- **Files**: add your output folder/file to the job's Files cell (`; ` between items), don't remove others.
- **Next action** is one concrete step for the user, with a **Due** date (YYYY-MM-DD). Set it when your work creates a new next step.
- **Outreach**: one row per audience; `First message sent` is `not sent` until the user says they sent it; Status is `Draft - not sent` until the user says otherwise.
- **History**: append one row for every change (`Source`: the agent name, or `user in chat`).
- Update `Last updated`. Each skill's "Tracker" step says exactly what it writes.
- Workers running in parallel (`.claude/agents/`) don't edit `tracker.md`; the manager writes their rows.

## Skills
- `career-map` — triggered by "map my career". Builds the `profile/` folder.
- `job-scout` — triggered by "scout jobs". Searches company career pages first, then job boards, for jobs posted in the last 14 days; opens each listing to check date, location, salary (at or above the floor in `settings.md` when listed) and that it's still open; marks each Safe / Check / Red flag for scam and ghost-job signs; scores 0-10 against `profile/`; skips anything in `jobs/seen.md`; saves a dated report (best first) in `jobs/`. Never applies or sends anything.
- `application-kit` — triggered by "build my kit for" a job from the jobs report, or a pasted job link. Reads the job post and extracts must-have skills and keywords; writes a one-page resume tailored to that job (only facts from `resume.md`, `wins.md` and `profile/`); writes a cover letter under 250 words (why this company, why me); runs a keyword check (covered vs missing, and why); saves everything in `applications/YYYY-MM-DD-company-job-title/`, with the resume also as a clean HTML page to print to PDF. Never applies or sends anything.
- `company-brief` — triggered by "brief me on" a company. Researches public sources (what it sells, who buys it, recent news, funding or earnings, launches, competitors); finds the 3 biggest current problems or goals, each with a source; writes a one-page brief readable in 5 minutes; builds a 5-slide HTML deck "My first 90 days at [Company]" showing how the user's real experience could help. Cites a source for every fact, marks guesses as guesses, uses only experience from `profile/`. Saves both in `companies/YYYY-MM-DD-company-name/`. Never contacts anyone or applies.
- `intro-writer` — triggered by "write intros for" a company. Reads `profile/` and the company brief (if any); picks 3 kinds of people to reach (likely hiring manager, someone on the team, a recruiter) with likely job titles and a LinkedIn search link each (no personal details collected); writes a first message under 80 words for each (one real company fact + one of the user's results); writes 2 follow-ups each (day 5 and day 12); saves in `outreach/YYYY-MM-DD-company-name/` and adds the drafts to `tracker.md` with follow-up dates. Never sends, posts or connects with anyone, never invents mutual connections.
- `interview-coach` — triggered by "interview me for" a job. Reads the job post, the company brief and `profile/interview-stories.md`; makes 8 likely questions (behavioral, role-specific, one tough curveball); runs a mock interview one question at a time, waiting for the user's answer; after each answer scores 1-5 and says what worked, what was missing and which of the user's stories would make it stronger. On "end interview" gives the top 3 fixes and saves the whole session in `interviews/`. Honest like a real interviewer, never answers for the user, never invents facts about the company or the user.
- `offer-coach` — triggered by "coach my offer" plus the pasted offer. Researches the market pay range for the role, level and location from public salary sources (links, date read, never invented); works out total first-year value (base, bonus, equity, sign-on, benefits the user mentions); compares offers side by side if there are several; says what to ask for and why, keeping the floor and target in `settings.md` in mind; writes a short counter-offer email and a phone script; then plays the recruiter pushing back with "this is our final offer" and scores the user's replies. Saves in `offers/YYYY-MM-DD-company-job-title/` and records the offer in `tracker.md`. Never sends, accepts or declines anything, never writes the user's replies for them.
- `job-search-manager` — runs the whole team. On "run my job search": runs `job-scout`; adds new jobs to `tracker.md`; for the top 3 new jobs launches `kit-builder` and `intro-drafter` subagents in parallel; checks their output and writes their rows to `tracker.md`; ends with a short morning summary (what's new, what's due today, what to do first) saved in `manager/`. On "weekly review": reads the tracker and says what's working, what isn't and what to change next week. Also records status changes the user reports in chat. Never applies, sends or posts anything; only the user can mark a job as applied; every other agent's rules still apply.
