# Safety and Boundaries

Use this reference for privacy, public sharing, browser automation limits, and uncertain application questions.

## Public Package Rules

Never include the user's private materials in a public ApplyPilot repo:

- Real resumes.
- Phone numbers, email addresses, addresses, or IDs.
- Immigration documents or work authorization evidence.
- Application history.
- Browser sessions, cookies, local storage, or screenshots with private data.
- OTPs or security codes.
- Private scripts that contain real job URLs, account state, or personal information.

Use templates and placeholders instead.

## Truthfulness Rules

Do not fabricate or alter:

- Legal name.
- Work authorization.
- Sponsorship needs.
- Employment history.
- Degree, school, dates, GPA, certificates, or licenses.
- Compensation history or expectations if not provided.
- Portfolio, work samples, publications, or references.
- Disability, veteran, gender, ethnicity, or other voluntary self-ID answers.

If unsure, ask or mark `Needs user`.

## Browser Automation Boundaries

Allowed with user authorization:

- Reading public job pages.
- Filling application forms with user-provided facts.
- Uploading user-provided resumes.
- Using autofill tools as accelerators.
- Reading email verification codes if the user connected an email tool and requested it.
- Recording outcomes.

Stop or hand off for:

- CAPTCHA, hCaptcha, reCAPTCHA, Cloudflare, or anti-bot checks.
- Login, password, 2FA, or unknown account switching.
- Payment, subscription, or purchase prompts.
- Camera, microphone, location, or extension permission prompts.
- Missing resume, portfolio, video, writing sample, or reference materials.
- Unclear legal, identity, employment, sponsorship, or compensation questions.

## Scheduled Daily Runs

The quick-start daily run (`references/daily-linkedin-run.md`) runs unattended, so it is limited to finding and screening:

- It never opens an application form and never submits anything. Applying happens only in a live apply session, with the user approving each submission.
- It keeps a modest pace: one tab at a time, a capped number of listings per run, and a daily application cap (10 by default).
- On any CAPTCHA, security check, "unusual activity" notice, or account warning, it stops all LinkedIn activity, pauses itself for 48 hours, and tells the user. It does not retry or work around it.
- It never enters passwords or login codes.

## Submission Counting

Only count an application after explicit evidence.

If the agent clicked submit but did not see confirmation, record `Pending confirmation` or `Blocked`, not `Submitted`.

## User Responsibility

The user remains responsible for:

- Truthfulness of all application materials.
- Final review of sensitive answers.
- Compliance with job board and employer terms.
- Handling human verification.
- Deciding whether Volume or Precision mode matches their job search.

ApplyPilot should make the workflow more structured and less draining, not remove human responsibility.
