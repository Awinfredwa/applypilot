# ApplyPilot

**English** · [中文](README.zh-CN.md)

An AI assistant that checks LinkedIn for new internships every day, picks the ones that fit you, and applies to them for you.

## Start here (no tech skills needed)

**You need:** a Mac or Windows computer, Google Chrome, a LinkedIn account, and a ChatGPT plan that includes Codex (Plus or higher).

1. Download the **ChatGPT desktop app** from [chatgpt.com/download](https://chatgpt.com/download), sign in, and open **Codex**.
2. In Google Chrome, add the **[Codex extension](https://chromewebstore.google.com/detail/codex/hehggadaopoacecdllhhajmbjkdcmajg)** and make sure you're logged into **LinkedIn**.
3. In Codex, keep the default model and set the effort to **High**.
4. Attach your resume, paste this message, and fill in the blank:

   ```text
   Set up ApplyPilot for me: https://github.com/Awinfredwa/applypilot
   I'm looking for: ______________________
   (example: summer 2027 marketing or finance internships in New York or remote)
   My resume is attached. Do all the technical setup yourself and only ask me what you need.
   ```

5. Answer its questions (about 15 minutes). It will do a practice search with you, send your first application together with you, and then schedule itself to run every day.

That's all the setup there is. You never need to touch any files or code.

## Every day

1. **It searches for you automatically.** At the time you pick, it checks LinkedIn for internships posted since the last search and skips the ones that don't fit.
2. **It applies for you.** For good-fit LinkedIn Easy Apply jobs where it already knows every answer, it fills in the form and sends it (up to 10 a day). You don't need to be there.
3. **It sends you a short report.** What it sent, plus anything waiting for you: usually a form question it hasn't seen before. Reply **apply** and you'll go through those together in a few minutes.
4. **It learns.** Each answer you give it once gets reused, so fewer jobs wait for you over time.

The report also includes a few great matches that aren't Easy Apply, with links, if you want to apply on the company's website yourself.

**Keep your laptop on with Chrome open around the scheduled time.** If it misses a day, it catches up on the next search. Most jobs are posted on weekday mornings, so a midday search catches them while they're fresh.

## Talking to it later

Open Codex and say things like:

- "ApplyPilot: apply"
- "ApplyPilot: also look for data analyst internships"
- "ApplyPilot: skip anything in Boston"
- "ApplyPilot: pause until Monday"
- "ApplyPilot: search at 7pm instead"
- "ApplyPilot: what did I apply to this week?"
- "ApplyPilot: stop auto-applying" (then every application waits for your OK)

Your records are in the **ApplyPilot** folder in your home folder. `tracker.csv` opens in Excel, Numbers, or Google Sheets.

## Good to know

- **It never makes things up.** Work authorization, pay, GPA, and dates come only from you. If it doesn't know, it asks.
- **It only sends what it's sure about.** It applies on its own only when every answer on the form is one you've already given it. Anything new waits for you. Applications can't be unsent, so check the daily report, and say "ApplyPilot: stop auto-applying" anytime to approve each one yourself instead.
- **It won't get past security checks for you.** CAPTCHAs, login codes, and "are you a robot" checks are yours. It stops and tells you.
- **This is against LinkedIn's rules, so there's some risk to your account.** LinkedIn's User Agreement doesn't allow bots or automated tools, and LinkedIn can restrict accounts that use them. ApplyPilot keeps a slow, human pace (one page at a time, about a minute between applications, 10 a day by default) and stops for 2 days if LinkedIn shows any warning. That lowers the risk but doesn't remove it. It's your account, so decide if you're comfortable, and lower the limit anytime.
- **It only updates itself when you say yes.** Every week or so it may ask "Want me to install this update?" The daily search never changes its own instructions.
- **Your info stays on your computer.** Your resume, answers, and history live in your ApplyPilot folder, never in this repo.

---

## For AI agents

Start with [AGENTS.md](AGENTS.md) and [SKILL.md](SKILL.md). Quick-start setup: [references/quickstart-setup.md](references/quickstart-setup.md). Daily search and apply sessions: [references/daily-linkedin-run.md](references/daily-linkedin-run.md).

## Advanced

The full workflow covers multiple job sites, resume tailoring, Precision/Volume modes, and the full dashboard. See [docs/full-workflow.md](docs/full-workflow.md).

## Credits

Based on [ApplyPilot](https://github.com/yvonnehe772/applypilot) by Yvonne He. MIT License.
