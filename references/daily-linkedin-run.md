# Daily LinkedIn Run

Use this reference for the quick-start LinkedIn flow after setup is complete (`state.setup_complete` is `true` in `settings.json`). There are two parts:

- **Daily run.** Unattended: the scheduled task runs it every day. It searches, screens, and records. When `auto_submit` is `true` (the default), it then sends LinkedIn Easy Apply applications on its own, but only ones where every answer on the form comes from something the user already confirmed. Last, it reports what it sent and what's waiting.
- **Apply session.** Live, with the user present: the user replies `apply` to the report or says "ApplyPilot: apply". It handles the jobs that need the user (a new question, a lower fit, or everything when `auto_submit` is off). The agent fills each form, shows a short summary, and submits only after the user says yes.

Everything follows `SKILL.md`: never guess high-impact facts, count only confirmed submissions, and never bypass CAPTCHA, login, or security checks. Stop before final submit everywhere except the daily run's "Auto-apply" step, which has its own stricter rules.

## Files

All private files live in the user's workspace folder (`workspace_path` in `settings.json`, usually `~/ApplyPilot`):

| File | Purpose |
|---|---|
| `profile.json` | Candidate facts. Source of truth. |
| `settings.json` | Searches, screening rules, schedule, daily cap, run state. |
| `answers.md` | Confirmed wording for form questions. |
| `tracker.csv` | Every job seen, one row per job, current status. |
| `notes.md` | Open questions for the user, lessons, run log. |
| `reports/YYYY-MM-DD.md` | Daily reports. |
| `resume/` | The user's resume file(s). |

Tracker statuses use the core ApplyPilot states:

| `status` | `next_action` | Meaning |
|---|---|---|
| `Pending` | `Easy Apply` | Good fit with Easy Apply. Sent by the daily run's auto-apply step if it qualifies; otherwise waits for an apply session. |
| `Pending` | `Apply yourself` | Strong fit but not Easy Apply. Listed in the report with a link. |
| `Needs user` | the exact question or action | A user-owned fact or action blocks this job, including a form question auto-apply couldn't answer. |
| `Skipped` | (empty) | Doesn't fit the rules. `reason` says why. |
| `Blocked` | retry plan or `none` | The site interaction could not safely proceed. |
| `Submitted` | (empty) | LinkedIn showed explicit confirmation. `evidence` records it. |

Use the LinkedIn job ID (the number in `/jobs/view/<id>`) as `job_id`. It is the dedupe key.

## Daily run

### 0. Pre-flight

1. Read `settings.json`. If it is missing or `state.setup_complete` is not `true`, stop and report: "ApplyPilot setup isn't finished. Open Codex and say: set up ApplyPilot."
2. If `state.paused_until` is in the future, stop and report: "ApplyPilot is paused until {date}. Say 'ApplyPilot: resume' to restart."
3. If `state.last_find_run` is today and `state.last_find_run_status` is `complete`, stop and report that today's search already ran, with a link to today's report file.
4. **No updates.** This run never downloads or installs ApplyPilot updates, and it follows these instructions as they were when it started. Updates happen only with the user present (see "Updating ApplyPilot").
5. **Check the browser.** Using the Chrome extension, open `https://www.linkedin.com/jobs/`.
   - If Chrome or the extension isn't reachable, stop and report: "I couldn't reach Chrome. Please open Chrome and make sure the Codex extension is on, then say 'ApplyPilot: run now'."
   - If LinkedIn shows a login page, stop and report: "Please log into LinkedIn in Chrome, then say 'ApplyPilot: run now'." Never type passwords or codes.
   - If the logged-in name doesn't match `linkedin.account_name`, stop and ask which account to use.

### 1. Search

1. Work out the freshness window: seconds since `state.last_find_run` plus 3600, with a minimum of 86400 (24 hours) and a maximum of 604800 (7 days). This catches up after missed days. If `last_find_run` is empty (first run), use 86400.
2. For each entry in `settings.searches`, open its `url` with `f_TPR=r<window>` set to that window. See "LinkedIn search URLs" below.
3. Read the result list, newest first. Collect at most 25 listings per search and 60 per run: job ID, title, company, location, work type, posted time, and whether the card shows `Easy Apply`.
4. Skip any job ID already in `tracker.csv`. Also skip reposts: same company and near-identical title found in the last 30 days.

Browse like a person: one tab at a time, no parallel scraping, and don't open more job pages than the screening needs.

### 2. Screen

1. Discard obvious misses from the title alone (clearly senior, clearly not an internship, clearly the wrong field). Record them as `Skipped` with a short reason.
2. For the rest, open the job and read the description. Check it against `settings.screening`, `profile.json`, and the tracker:
   - Internship season and dates versus `profile.search`.
   - Graduation window, degree level, and enrollment requirements versus `profile.education`.
   - Sponsorship, citizenship, and clearance statements versus `profile.work_authorization`.
   - Location and remote policy versus `profile.search.locations`, `open_to_remote`, and `open_to_relocate`.
   - Pay or unpaid status, if the user has a rule for it.
3. Assign `fit`: `High`, `Medium`, `Low`, or `Stretch`. Use judgment, not a fake numeric score.
4. Classify:
   - Fits and has Easy Apply: `Pending` / `Easy Apply`.
   - Fits well but no Easy Apply: at most `apply_yourself_picks_per_day` of the best as `Pending` / `Apply yourself`. Mark the rest `Skipped` with reason `Not Easy Apply; lower fit than today's picks`.
   - A posting conflicts with an unknown fact (for example, sponsorship is unknown and the posting says no sponsorship): `Needs user`, with the exact question in `next_action`.
   - Violates a rule: `Skipped`, with the reason.

### 3. Record

1. Append one row per new job to `tracker.csv`. Never delete rows. Update `status`, `next_action`, and `notes` in place when a job changes.
2. Add any new user question to "Questions to ask the user" in `notes.md` (dedupe similar questions).
3. Set `state.last_find_run` to now and `state.last_find_run_status` to `complete` in `settings.json`. If the run stopped early, use `stopped: <reason>`.
4. Add one line to the run log in `notes.md`.

### 4. Auto-apply

Skip this step, and say why in the report, if:

- `auto_submit` is not `true`, or
- `linkedin.stored_resume_name` is empty. The resume isn't saved on LinkedIn yet, and uploading a file needs the user to allow it, so the first apply session has to do that.

Otherwise:

1. **How many.** Count `Submitted` rows whose `applied_at` is today in the user's `timezone`. Send at most `daily_cap` minus that.
2. **Which jobs.** `Pending` / `Easy Apply` jobs with `fit` `High` or `Medium`, including leftovers from earlier days that are still open. High first, then newest. `Low` and `Stretch` jobs wait for the user.
3. **For each job, one at a time:**
   1. Open it. If it shows `Applied`, is closed, or no longer has Easy Apply, update the tracker and move on.
   2. Click `Easy Apply` and fill each step by the same field rules as the apply session (its step 1.2), with one difference: **there's no one to ask.** The moment any field can't be filled from `profile.json` facts or `Confirmed: yes` rows in `answers.md`, with wording that clearly matches, stop this application:
      - Click Dismiss and choose **Discard** (not Save).
      - Mark the job `Needs user`, put the exact question in `next_action`, and add it to "Questions to ask the user" in `notes.md`.
      - Move on to the next job.

      These also count as "can't fill": contact info that doesn't match the profile, any resume other than `linkedin.stored_resume_name`, any file upload, a cover letter or written answer with no confirmed pattern, a pay question while `pay.policy` is `ask_me`, an unlisted skill while `answer_for_unlisted_skills` is unset, and any browser permission prompt.
   3. **Check before sending.** On LinkedIn's review page, confirm the resume is `linkedin.stored_resume_name` and the work authorization and sponsorship answers match `profile.json`. If `linkedin.unfollow_company_on_submit` is `true`, uncheck "Follow {company}". If anything looks off, discard and mark `Needs user` as above.
   4. **Send and confirm.** Click Submit. Count it only on explicit confirmation, the same as the apply session (its step 1.4). Write the key answers you used in the tracker's `notes` so the report can show them.
   5. Close the tab and wait about a minute before the next job.
4. Any stop condition (see "Stop conditions") ends auto-apply and the rest of the run at once. Set `state.last_find_run_status` to `stopped: <reason>`.

### 5. Report

Write `reports/YYYY-MM-DD.md` and show the same text as the final message. Keep it short and plain. No file paths, no jargon, no status codes.

```text
ApplyPilot, {Weekday Mon D}

Sent: {n} · Waiting for you: {n} · Apply yourself: {n} · Skipped: {n}

Sent for you (LinkedIn Easy Apply)
1. {Company}, {Title} ({Location}). Authorized: {…}, sponsorship: {…}{, any other key answer}
...

Waiting for you
- {Company}, {Title}: {the question in plain words, or "lower fit, want to apply?"}

Worth applying yourself (company website)
- {Company}, {Title}: {link}

Reply "apply" to go through the ones waiting for you. Each answer you give me gets reused, so fewer will wait next time.
```

"Waiting for you" lists every open `Needs user` job and every `Pending` / `Easy Apply` job that wasn't sent, oldest first, up to `daily_cap`. If auto-apply was skipped, say why in one line at the top (for example, "I haven't sent anything yet because your resume isn't saved on LinkedIn. Reply 'apply' and we'll send the first one together."). If `auto_submit` is off, there's no "Sent" section.

If nothing was found or sent, say so in one line and mention when the next search will run. If the run stopped (paused, login needed, Chrome not reachable, LinkedIn warning), the report is just that one message and what the user should do, plus anything already sent before the stop.

## Apply session

Run this when the user replies `apply` to a report, says "ApplyPilot: apply", or asks to apply to specific jobs from the tracker. It covers the jobs that are waiting: `Needs user` jobs once their question is answered, `Pending` / `Easy Apply` jobs the daily run didn't send, and all of them when `auto_submit` is off. The user is present, so questions can be answered right away.

### 0. Pre-flight

1. Check LinkedIn login the same way as the daily run.
2. Count today's `Submitted` rows. Only apply up to `daily_cap` in total for the day. If the cap is reached, say so and stop.
3. If `notes.md` has open questions, ask them first, in one short batch. Save confirmed answers to `answers.md` (and facts to `profile.json`) before starting.

### 1. For each queued job

Order: `fit` High first, then newest.

1. Open the job. If it shows `Applied`, is closed, or no longer has Easy Apply, update the tracker and move on.
2. Click `Easy Apply` and go through each step:
   - **Contact info.** Check that the prefilled email and phone match `profile.json`. If not, ask.
   - **Resume.** Select the stored LinkedIn resume named in `linkedin.stored_resume_name`. If none is stored, upload the file from `resume/`. Codex will ask the user to allow the upload. Afterwards, save the resume's LinkedIn display name in `settings.json`. Verify the right resume is selected before continuing.
   - **Questions.** Answer only from `profile.json` facts and `Confirmed: yes` rows in `answers.md`:
     - "Years of experience with X": use `profile.experience_years.table`. For unlisted skills, use `answer_for_unlisted_skills` only if it is set and confirmed; otherwise ask the user.
     - Yes/no work authorization and sponsorship questions: answer only when the wording clearly matches the profile facts; otherwise ask.
     - Voluntary self-ID: "Prefer not to say" / decline / leave blank.
     - Any other question with no confirmed answer: ask the user right now in one short sentence. Once they answer, fill it in and add it to "Learned from applications" in `answers.md` as confirmed.
   - **Review.** If `linkedin.unfollow_company_on_submit` is `true`, uncheck "Follow {company}".
3. **Stop before submit.** Show a short summary and wait for the user's go-ahead:

   ```text
   {Company}, {Title}
   Resume: {resume name}
   Key answers: authorized to work: {…}, sponsorship: {…}, {any pay, date, or new answer}
   Send it?
   ```

   On "yes" / "send", click Submit. On "no" / "skip", click Dismiss, choose **Discard** (not Save) in LinkedIn's "Save this application?" dialog, and mark the job `Skipped` with reason `User declined`.
4. **Confirm.** Count it only when LinkedIn shows explicit confirmation, such as "Your application was sent" or the job now showing "Applied". Record `Submitted`, `applied_at`, and the confirmation text in `evidence`. If there is no confirmation, record `Blocked` with what was observed. Never count it as submitted.
5. Close the job tab and any leftover dialogs before the next job.

### 2. Wrap up

End with one short line, for example: "Sent 6 applications. 2 skipped. See you tomorrow around {run_time}." Update the tracker, `notes.md`, and the run log.

Then, if an update check is due, run "Updating ApplyPilot" below.

## Updating ApplyPilot

An update changes the instructions every future run follows, so it only happens with the user present and only after they say yes. The daily run never updates.

Check for an update when the user says "ApplyPilot: update". Also check, if `updates` is `ask` and `state.last_update_check` is empty or more than 7 days ago, at the end of an apply session or after handling any other "ApplyPilot: …" request. If `updates` is `off`, only check when the user asks.

1. **Fetch without installing.**
   - `install_method` `git`: run `git -C <repo_path> fetch origin main`. If `HEAD` already matches `origin/main`, there's nothing new.
   - `install_method` `zip`: download `<repo_url>/archive/refs/heads/main.zip` into a new, empty temporary folder and unzip it there. Compare its `applypilot-main` folder with `repo_path`. If nothing differs, there's nothing new.
2. **Read every changed file yourself.** If a change would loosen the safety contract (auto-submitting answers the user hasn't confirmed, raising the daily cap, entering passwords or codes, getting past security checks, sending the user's data somewhere new, or running downloaded scripts), don't install it. Tell the user in plain words what the change would do, and suggest they ask whoever shared ApplyPilot with them about it.
3. **Ask.** Otherwise, say in one or two plain sentences what's new and ask: "Want me to install this update?"
4. **Install only on yes.**
   - `git`: run `git -C <repo_path> merge --ff-only origin/main`.
   - `zip`: replace the contents of `repo_path` with the unzipped `applypilot-main` folder's contents. The repo folder holds no private data, so replacing it is safe.
   - Set `state.installed_version`: the new commit ID for `git`, or `main.zip from {date}` for `zip`.
   - On no, change nothing.
5. **Record the check.** Set `state.last_update_check` to now, whatever the outcome, and delete the temporary folder. If the check itself fails (no network, GitHub unreachable), don't mention it; try again next time.

## Stop conditions (both parts)

Stop all LinkedIn activity immediately and tell the user if any of these appear:

- CAPTCHA, "unusual activity", identity or security verification, or any account restriction or warning. Set `state.paused_until` to 48 hours from now and tell the user in plain words. Don't retry.
- A LinkedIn message that Easy Apply or application limits were reached. Stop for today.
- A login page, 2FA, or a request for a password or code.
- Payment, subscription, or "upgrade to Premium" gates blocking the flow. Don't subscribe; skip that job.
- Any browser permission prompt the user hasn't approved before.

## LinkedIn search URLs

Build each saved search as:

```text
https://www.linkedin.com/jobs/search/?keywords=<keywords>&location=<location>&f_JT=I&f_TPR=r86400&sortBy=DD
```

| Parameter | Meaning |
|---|---|
| `keywords` | Role words, URL-encoded, e.g. `marketing%20intern`. |
| `location` | City, state, or country, URL-encoded, e.g. `New%20York%2C%20NY` or `United%20States`. |
| `f_JT=I` | Job type: Internship. If results are thin, try `f_E=1` (experience level: Internship) instead. |
| `f_TPR=r<seconds>` | Posted within the last N seconds. `r86400` is 24 hours; the daily run replaces it with the freshness window. |
| `f_WT=2` | Remote only. `1` is on-site and `3` is hybrid; combine with `%2C`, e.g. `f_WT=2%2C3`. |
| `f_AL=true` | Easy Apply only. Not used by default, so strong non-Easy-Apply jobs can still be listed. |
| `sortBy=DD` | Most recent first. |

For entry-level full-time roles instead of internships, use `f_E=2&f_JT=F`.

LinkedIn changes these parameters now and then. If a filter stops working, set it with the page's filter buttons instead, copy the resulting URL into `settings.searches[].url`, and add a line to Lessons in `notes.md`.

## Changing things later

When the user says "ApplyPilot: …" in a normal chat, make the change and confirm it in one line:

| User says | Do this |
|---|---|
| "apply" | Start an apply session. |
| "run now" | Run the daily run now (search, auto-apply, report), even if it already ran today. |
| "stop auto-applying" / "start auto-applying" | Set `auto_submit` to `false` or `true`. When off, every application waits for an apply session. |
| "also look for X" / "stop looking for X" | Add or remove a search in `settings.searches`. Show the new search in plain words. |
| "skip anything …" | Add a plain-English rule to `settings.screening.skip_if`. |
| "my answer to X is Y" | Save it to `answers.md` as confirmed, or to `profile.json` if it is a fact. |
| "pause until …" / "resume" | Set or clear `state.paused_until`. |
| "run at {time} instead" | Update `run_time`, then update the Codex scheduled task. If you can't edit it directly, give the user the exact clicks. |
| "apply to at most N a day" | Update `daily_cap`. Above 15, mention that a lower pace is gentler on the LinkedIn account. |
| "what did I apply to …" | Summarize from `tracker.csv`. |
| "new resume" | Save it in `resume/`, update the profile from it, confirm changes with the user, and clear `linkedin.stored_resume_name` so the next apply session uploads the new one. |
| "update" | Run "Updating ApplyPilot" now. |
| "turn off updates" / "turn on updates" | Set `updates` to `off` or `ask`. |
| "stop completely" | Set `paused_until` far in the future and explain how to delete the scheduled task in Codex. Never delete the user's files. |
