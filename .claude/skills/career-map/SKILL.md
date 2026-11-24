---
name: career-map
description: Build the user's career profile from resume.md and wins.md. Use when the user says "map my career" (or asks to refresh/redo their career map). Picks 3 target job titles, 15 target companies, turns each win into an interview story, writes LinkedIn headlines, an About section and post ideas, and saves everything to the profile/ folder for other agents to read.
---

# Career Map

Turns `resume.md` and `wins.md` into a reusable career profile in `profile/`. Every later agent in this folder reads that folder, so accuracy matters more than polish.

## Hard rules

1. **Never invent.** Every experience, number, skill, tool, title, date and preference must come from `resume.md`, `wins.md`, or something the user said in chat. If it isn't there, don't write it.
2. **Never round up, extrapolate or "improve" a number.** Quote it as written. You may do simple arithmetic on given numbers (e.g. 31 → 19 minutes is 12 minutes shorter) but say it's derived.
3. **Missing detail → mark it, don't fill it.** Write `[MISSING: what is needed]` in the output and list it in `profile/gaps.md`. A story with a thin "situation" is better than a made-up one.
4. **Company facts need care.** Company reasons must rest on the user's files plus well-known public facts you are confident about. If WebSearch is available, use it to check that each company is real, still operating, and hires for the target roles (and remote/Denver if relevant). Mark anything unverified as `(unverified)`. Never claim a specific open job, salary or headcount you haven't seen.
5. **Tell the user what's missing** (see "Gaps" below) in the final reply, not just in the file.
6. Do not send, post or publish anything. Output stays in local files.

## Steps

### 1. Read inputs
Read `resume.md` and `wins.md` fully. If either is missing or nearly empty, stop and tell the user what to add. Also read any existing `profile/` files so you can update rather than contradict them.

### 2. Write `profile/facts.md` (source of truth)
A plain, structured list extracted from the inputs, with no interpretation:
- Name, location, work-authorization/remote notes if stated
- Roles: title, employer, dates, one line each
- Skills and tools exactly as listed
- Every number claim, each tagged with its source (`resume` or `wins`) and the role it belongs to
- Education
- Stated preferences: target titles, industries, location, salary floor/target, anything else the user said they want
- Stated weak spots

Other agents lean on this file, so keep it exact. Flag any place the resume and wins disagree (e.g. different figures for the same result).

### 3. Pick 3 target job titles → `profile/target-roles.md`
Choose the 3 titles the user is best placed for, weighing their stated next step, seniority, years of experience, and evidence of the skills that title demands. For each:
- Title (and common alternate titles recruiters use)
- One line on why they fit, citing specific evidence from their files
- One honest line on the main gap for that title (use the user's own stated weak spots; don't invent others)

Order them best fit first. If the user's stated target title differs from what the evidence best supports, say so plainly.

### 4. Pick 15 target companies → `profile/target-companies.md`
Companies that hire for those roles and match the user's stated preferences (industry, location/remote, salary floor). A table with: company, what they do (short), which of the 3 roles, **reason it fits this person** (tie to a specific item from their files), and a verification note.
- Mix of sizes/stages; don't pad with one type.
- Honor stated preferences strictly. If a preference is unstated (company size, stage, dealbreakers), assume nothing and add it to gaps.
- Don't list the user's current employer.
- Salary: don't assert what a company pays. Note only if you verified a published range against the user's floor.

### 5. Interview stories → `profile/interview-stories.md`
One short story per win in `wins.md` (and any resume bullet that is clearly a major win and has enough detail). Each story:
- **Title** (a few words)
- **Situation** — the context and problem
- **What I did** — the user's actions, in first person, specific
- **Result** — with the numbers exactly as given
- **Good for answering** — 2–3 typical interview prompts (e.g. "tell me about influencing without authority")
- **Missing** — what would make it stronger (e.g. no baseline, no timeframe)

Stay in first person, plain spoken, ~120–180 words. Use only facts from the files; if the situation isn't described, say `[MISSING: ...]` rather than guessing. Wins that are weak or sparse should still get an entry, clearly marked as thin.

### 6. LinkedIn content → `profile/linkedin.md`
- **3 headline options** (max 220 characters each), each with a different angle (e.g. role + impact, domain + skills, target-role-forward). Only claims supported by the files.
- **About section** (first person, ~150–250 words, plain language, opens with something concrete, ends with what they're looking for if stated).
- **4 post ideas** based on the wins: for each, the hook (first line), the angle, the proof points (numbers from the files), and a one-line takeaway for readers.
- Add a note: before posting, check that employer-specific figures and any patient/customer/company information are OK to share publicly. Keep posts free of confidential detail.

### 7. Gaps → `profile/gaps.md`
List what's missing or unclear that affects job search quality, most important first. Always check for:
- Current LinkedIn headline/About/URL (needed to compare and to link)
- Preferences not stated: company size/stage, dealbreakers, work-authorization, willingness to relocate/travel, how many direct reports they'd want
- Results without numbers, baselines or timeframes
- Evidence for the target role that is absent (e.g. managing people, tools the target roles need)
- Contradictions between resume and wins
- Anything in the user's own "weak spots" that a story or headline needs to handle honestly

Phrase each as a question the user can answer in one line.

### 8. Index → `profile/README.md`
A short table of contents of every file in `profile/`, what it holds, and the date it was generated. Say this folder is the shared source for all agents and that `facts.md` wins if files conflict.

## Output location
All files go in `profile/` at the project root (create it if missing). Overwrite files this skill owns (`facts.md`, `target-roles.md`, `target-companies.md`, `interview-stories.md`, `linkedin.md`, `gaps.md`, `README.md`). Never delete other files in `profile/` that other agents or the user added.

## Final reply to the user
Keep it short:
- The 3 titles, one line each
- Where the files are
- The top gaps (what's missing and the questions to answer)
- Anything you could not verify (company facts)
Do not paste the full documents into chat.
