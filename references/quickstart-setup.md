# Quick-Start Setup

Use this reference when a user pastes the README quick-start message ("Set up ApplyPilot for me …"), or otherwise asks for the simple daily LinkedIn setup. The user is likely **not technical**. The goal is a working daily LinkedIn routine in about 15 minutes of their time.

## How to talk to the user

- Do all technical work yourself: downloading, folders, files, JSON, and the scheduled task. Never ask the user to open a terminal, run a command, or edit a file.
- Use plain words. Say "your ApplyPilot folder", not "workspace". Say "a daily search", not "automation". Avoid file names, paths, and code unless the user asks.
- Ask questions in at most two short batches, numbered, each with a suggested default, so the user can reply "defaults are fine".
- When Codex shows a permission prompt, tell the user in one sentence what to click and why, for example: "Codex will ask to let me create an ApplyPilot folder in your home folder. Please click Allow."
- Keep every message short. Celebrate the finish line, not each step.

Everything in `SKILL.md` still applies: never guess high-impact facts, stop before every final submit, and never bypass CAPTCHA, login, or security checks.

## What the finished setup looks like

```text
<skills folder>/applypilot/   ← this repo: instructions only; updated automatically
~/ApplyPilot/                 ← the user's private folder; never inside the repo
   AGENTS.md  profile.json  settings.json  answers.md  tracker.csv  notes.md
   resume/  reports/
```

On Windows, `~` means the user's home folder, for example `C:\Users\<name>`.

These files satisfy the ApplyPilot prerequisites in `SKILL.md`: `profile.json` is the Candidate Profile, `tracker.csv` is the Dashboard, `settings.json` screening is the Application Rules, and the Resume Strategy is Volume mode with one resume (or a few, by role family).

## Step 1: Install (silent)

Don't narrate this step beyond one line such as "Setting things up on your computer…".

1. Detect the OS (macOS or Windows) and the user's home folder.
2. Choose the skills folder: `~/.agents/skills/` (current Codex location for personal skills). If this Codex version still uses the legacy `~/.codex/skills/` instead, use that. The repo goes in `<skills folder>/applypilot`.
3. Get the repo from `https://github.com/Awinfredwa/applypilot`:
   - If it already exists there, update it instead (see the update step in `daily-linkedin-run.md`).
   - If git is available **without triggering an install prompt**, `git clone` it. On macOS, check `xcode-select -p` first; if it fails, don't run `git`, because that opens a developer-tools install dialog. Use the zip instead.
   - Otherwise download `https://github.com/Awinfredwa/applypilot/archive/refs/heads/main.zip`, unzip it, and move the `applypilot-main` folder's contents to `<skills folder>/applypilot`. On macOS use `curl` and `unzip`; on Windows use PowerShell `Invoke-WebRequest` and `Expand-Archive`.
4. Create `~/ApplyPilot/` with `resume/` and `reports/` inside. If it already exists with files, **don't overwrite anything**: this is a re-setup, so read the existing files and only fill gaps.
5. Copy the templates from `templates/internship/`: `profile.template.json` to `profile.json`, `settings.template.json` to `settings.json`, `answers.template.md` to `answers.md`, `tracker.template.csv` to `tracker.csv`, and `notes.template.md` to `notes.md`.
6. Save the user's resume into `~/ApplyPilot/resume/` with its original file name.
7. Fill `settings.json`: `repo_path`, `workspace_path`, `install_method` (`git` or `zip`), `os`, and `timezone` (from the system).
8. Copy `templates/internship/workspace_AGENTS.template.md` to `~/ApplyPilot/AGENTS.md` and fill in `{{REPO_PATH}}`. Then any Codex chat or scheduled task opened in the ApplyPilot folder knows where its instructions are, even if skill discovery fails.

If there is no resume attached, ask for it before going further ("Please attach your resume (PDF or Word) and I'll take it from there."). If the user didn't say what jobs they want, ask that in the first question batch.

## Step 2: Read the resume and draft the profile

1. Extract facts from the resume into `profile.json`: name, email, phone, city, LinkedIn and portfolio links, school, degree, major, graduation date, and skills.
2. Draft `profile.experience_years.table` from the resume: honest years per skill or tool that is evidenced (internships, jobs, projects, coursework). When unsure, round down. Leave it `confirmed: false` until the user confirms it in Step 3.
3. Don't fill work authorization, sponsorship, pay, GPA (unless it's on the resume), or relocation from inference. Those come from the user.

Then show a short summary:

```text
Here's what I got from your resume:
- {Name}, {school}, {major}, graduating {month year}
- Based in {city}
- Strongest areas: {3–5 skills}
Anything wrong or missing?
```

## Step 3: Questions (one or two batches, at most 8)

Adapt the wording to the user's country and what the resume already answered. Skip anything already known. Offer defaults.

1. **What to look for.** "I'll search LinkedIn for: {2–3 proposed searches from what they told you, e.g. 'marketing intern, New York' and 'marketing intern, remote'}. Which internship season: {e.g. Summer 2027}? Anything to add?"
2. **Where.** "Which cities work for you? Is remote OK? Would you relocate for the right internship?"
3. **Work authorization.** Plain multiple choice, e.g. for the US: "(a) US citizen or green card, (b) international student on F-1 (you'd use CPT/OPT), (c) something else. Tell me which." Then set `authorized_to_work`, `requires_sponsorship_now`, and `requires_sponsorship_future` from the answer. If it's unclear, ask one follow-up; don't guess.
4. **School status.** Confirm the expected graduation date and that they're currently enrolled. Ask whether they'll return to school after the internship.
5. **Skip list.** "Anything I should always skip, such as certain companies, industries, unpaid roles, or on-site only?"
6. **Experience table.** "LinkedIn forms often ask 'how many years of experience with X'. From your resume I'd answer: {Excel: 1, Python: 1, …}. Is that right? For skills not on this list, should I answer 0, or ask you each time?" Save it as confirmed; set `answer_for_unlisted_skills` only if the user explicitly chooses 0.
7. **Pay.** "If a form asks your expected pay, what should I say? (Default: I'll ask you each time.)"
8. **Timing.** "I'll search every day at about {suggested time} (see 'Picking the run time'). Your laptop needs to be on with Chrome open then. Does that work, or is another time better?" Also: "I'll go through up to 10 applications with you a day. OK?"

Tell the user (don't ask): "For optional questions about gender, race, veteran status, or disability, I'll choose 'Prefer not to say'. I'll only enter your GPA if you give it to me and a form requires it."

Save everything: facts to `profile.json`, wording to `answers.md` (mark `Confirmed: yes` only for what the user said or approved), and searches, screening, run time, and cap to `settings.json`. Build each search URL following "LinkedIn search URLs" in `daily-linkedin-run.md`.

### Picking the run time

Most companies post jobs on weekday mornings in their local time. A search around **12:30pm in the timezone of the job market** catches the morning wave while it's fresh, and the freshness window catches everything since the last run. But a run only happens when the laptop is awake with Chrome open, and a missed day costs more than a few hours of freshness. So:

- If the user's laptop is usually on at midday, use 12:30pm.
- Otherwise use a time it's reliably on, such as early evening (about 7pm).
- If the job market is in a different timezone from the user, convert and say both times.

## Step 4: LinkedIn and Chrome check (with the user)

1. Open `https://www.linkedin.com/jobs/` through the Codex Chrome extension.
   - If Codex asks for permission to use linkedin.com, tell the user: "Please choose **Always allow** for linkedin.com. That lets the daily search run while you're away."
   - If the extension isn't installed or connected, walk the user through it in plain steps: install "Codex" from the Chrome Web Store, sign in, and turn it on.
2. If LinkedIn isn't logged in, ask the user to log in themselves in Chrome. Never type their password.
3. Save the logged-in account's display name to `settings.linkedin.account_name`.

## Step 5: Practice search (no applying)

Run the "Daily find run" from `daily-linkedin-run.md`, with three changes: skip pre-flight checks 1–4 (setup isn't marked complete yet), don't set `last_find_run`, and label the report "Practice run". Aim for about 10 jobs.

Show the report and ask: "Do these look like the right kind of jobs? Anything I should skip or add?" Adjust searches and skip rules from the answer. Repeat once if the results were clearly off.

## Step 6: First application together

Pick the best `Pending` / `Easy Apply` job and run the "Apply session" from `daily-linkedin-run.md` for just that one, with the user watching:

- This is where the resume gets uploaded to LinkedIn for the first time. Tell the user Codex will ask to allow the upload.
- Answer any new questions with the user and save them.
- Stop before submit, show the summary, and submit only when the user says yes.
- Confirm LinkedIn showed "application sent".

If the user doesn't want to send a real application yet, skip this step. The first real apply session will cover the resume upload instead.

## Step 7: Schedule the daily search

1. Fill in `templates/internship/daily_task_prompt.md` with the real `repo_path` and `workspace_path`.
2. Create a Codex scheduled task:
   - Name: `ApplyPilot daily search`
   - Schedule: every day at `run_time`
   - Folder or project: the user's ApplyPilot folder
   - Model: the same one the user is using (the README recommends GPT-6 Sol, High effort)
   - Prompt: the filled-in text from `daily_task_prompt.md`
3. If you can create scheduled tasks directly, do it and confirm. If you can't, copy the prompt to the clipboard (`pbcopy` on macOS, `Set-Clipboard` on Windows) and give the user short click-by-click steps: in the Codex sidebar, open the scheduled tasks area (called Automations or Scheduled tasks), create a new one, paste the prompt, pick the time, and save. Then verify it exists before moving on.
4. Set `state.setup_complete` to `true` in `settings.json`.

## Step 8: Wrap up

Send one short final message, for example:

```text
You're all set!

Every day at {time}, I'll check LinkedIn for new {role} internships and send you a short list.
Reply "apply" and we'll go through them together. About 1–2 minutes each, and nothing is sent without your OK.

Keep your laptop on with Chrome open around {time}. If it misses a day, it catches up next time.

Anytime, just say things like:
- "ApplyPilot: also look for data analyst internships"
- "ApplyPilot: pause until Monday"
- "ApplyPilot: what did I apply to this week?"

Your tracker is the ApplyPilot folder in your home folder (tracker.csv opens in Excel or Google Sheets).
```
