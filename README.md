# ApplyPilot

**English** · [中文](README.zh-CN.md)

An AI assistant that checks LinkedIn for new internships every day, picks the ones that fit you, and helps you apply in a few minutes.

## Start here (no tech skills needed)

**You need:** a Mac or Windows computer, Google Chrome, a LinkedIn account, and a ChatGPT plan that includes Codex (Plus or higher).

1. Download the **ChatGPT desktop app** from [chatgpt.com/download](https://chatgpt.com/download), sign in, and open **Codex**.
2. In Google Chrome, add the **[Codex extension](https://chromewebstore.google.com/detail/codex/hehggadaopoacecdllhhajmbjkdcmajg)** and make sure you're logged into **LinkedIn**.
3. In Codex, pick the model **GPT-6 Sol** and set the effort to **High**.
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

1. **It searches for you automatically.** At the time you pick, it checks LinkedIn for internships posted since the last search, skips the ones that don't fit, and sends you a short list.
2. **You apply in a few minutes.** Reply **apply**. It fills in each LinkedIn Easy Apply form, shows you a quick summary, and sends it when you say yes (up to 10 a day).
3. **It learns.** When a form asks something new, it asks you once and remembers the answer.

The list also includes a few great matches that aren't Easy Apply, with links, if you want to apply on the company's website yourself.

**Keep your laptop on with Chrome open around the scheduled time.** If it misses a day, it catches up on the next search. Most jobs are posted on weekday mornings, so a midday search catches them while they're fresh.

## Talking to it later

Open Codex and say things like:

- "ApplyPilot: apply"
- "ApplyPilot: also look for data analyst internships"
- "ApplyPilot: skip anything in Boston"
- "ApplyPilot: pause until Monday"
- "ApplyPilot: search at 7pm instead"
- "ApplyPilot: what did I apply to this week?"

Your records are in the **ApplyPilot** folder in your home folder. `tracker.csv` opens in Excel, Numbers, or Google Sheets.

## Good to know

- **It never makes things up.** Work authorization, pay, GPA, and dates come only from you. If it doesn't know, it asks.
- **Nothing is sent without your OK.** You approve every application.
- **It won't get past security checks for you.** CAPTCHAs, login codes, and "are you a robot" checks are yours. It stops and tells you.
- **Be gentle with LinkedIn.** LinkedIn doesn't like automated tools. ApplyPilot keeps a normal pace (10 applications a day by default) and pauses for 2 days if LinkedIn shows any warning. It's still your account, so you can lower the limit anytime.
- **Your info stays on your computer.** Your resume, answers, and history live in your ApplyPilot folder, never in this repo.

---

## For AI agents

Start with [AGENTS.md](AGENTS.md) and [SKILL.md](SKILL.md). Quick-start setup: [references/quickstart-setup.md](references/quickstart-setup.md). Daily search and apply sessions: [references/daily-linkedin-run.md](references/daily-linkedin-run.md).

## Advanced

The full workflow covers multiple job sites, resume tailoring, Precision/Volume modes, and the full dashboard. See [docs/full-workflow.md](docs/full-workflow.md).

## Credits

Based on [ApplyPilot](https://github.com/yvonnehe772/applypilot) by Yvonne He. MIT License.
