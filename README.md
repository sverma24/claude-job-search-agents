# AI Agents for Job Search with Claude Code

Eight AI agents that help you find jobs, apply, and get ready for interviews and offers. They run in Claude Code (the **Code** tab of the Claude desktop app, or the `claude` command in a terminal) on your normal Claude plan. No API keys, nothing extra to sign up for.

**They never apply, send, post or connect with anyone for you.** Everything they write is a draft for you to check.

| Agent | Say this | What you get |
|---|---|---|
| Career Map | `map my career` | Your 3 best-fit job titles, 15 target companies, interview stories from your wins, LinkedIn headline and post ideas, and a list of what's missing (`profile/`) |
| Job Scout | `scout jobs` | Jobs posted in the last 14 days, from company career pages first, each one opened and checked for date, pay, location and whether it's still open, scored 0–10 and marked Safe / Check / Red flag for scam and ghost-job signs (`jobs/`) |
| Application Kit | `build my kit for <job>` or paste a job link | A tailored resume (Markdown + print-ready HTML), a cover letter and a keyword check, using only facts from your files (`applications/`) |
| Company Brief | `brief me on <company>` | A cited one-page brief and a 5-slide "My first 90 days" deck you can click through (`companies/`) |
| Intro Writer | `write intros for <company>` | Who to contact (by job title, with LinkedIn search links), short first messages and two follow-ups each (`outreach/`) |
| Interview Coach | `interview me for <job>` | A mock interview, one question at a time, each answer scored 1–5 with the story that would make it stronger. Say `end interview` for your top 3 fixes (`interviews/`) |
| Offer Coach | `coach my offer` + paste the offer | Market pay research with links, first-year value, what to ask for, a counter-offer email and phone script, then a "final offer" role-play (`offers/`) |
| Job Search Manager | `run my job search` · `weekly review` | Runs the Job Scout, then builds kits and intros for the top jobs with two worker subagents in parallel, keeps one `tracker.md`, and gives you a morning summary or a weekly review |

## Set it up

1. Download this repository (green **Code** button → **Download ZIP**) and unzip it, or `git clone` it.
2. Replace `resume.md` and `wins.md` with your own. The ones here belong to Morgan Ellis, a made-up job seeker, so you can see the format.
3. Edit `settings.md`: country, timezone, city and work style, currency, salary floor and target, notice period, resume paper. In India? Copy the India example in that file.
4. Open the Claude desktop app, go to the **Code** tab, and pick this folder. (Or run `claude` in this folder.)
5. Say `map my career` first. Every other agent reads the profile it builds.

Claude asks before it searches the web, runs a command or edits files in `.claude/`. Read each request and choose **Allow once** while you get to know the agents.

To make it run by itself, say: *"Run my job search by itself every weekday at 8 AM my time."* Claude creates a scheduled task. The Claude app has to be open at that time.

## Where things live

- `CLAUDE.md`: the shared rules every agent follows, the folder map and the tracker layout.
- `.claude/skills/`: one folder per agent, each with a `SKILL.md` written in plain English. Change them to change how an agent works.
- `.claude/agents/`: the manager's two workers, `kit-builder` and `intro-drafter`.
- Your results (`profile/`, `jobs/`, `applications/`, `tracker.md` and the rest) are in `.gitignore`, so they stay on your computer.

## Good to know

- **Check every job yourself before you apply.** The agents can't read pages behind a sign-in, and job boards copy each other.
- **Read every draft before you use it.** The agents only use facts from your files and tell you what's missing, but you know your story best.
- Each run counts toward your Claude plan's usage limits. A full `run my job search` is the heaviest one.
- Salary sites are estimates, and nothing here is legal or financial advice.

Made for the YouTube course "AI Agents for Job Search with Claude Code: Build 8 Agents".
