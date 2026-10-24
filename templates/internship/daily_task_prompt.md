# Daily Task Prompt

During setup, the agent fills in the two paths below and uses the text in the box as the prompt for the Codex scheduled task. Keep it short: the real instructions live in the repo, so repo updates take effect without editing the task.

```text
Run my ApplyPilot daily job search.

1. Read {{REPO_PATH}}/references/daily-linkedin-run.md and follow the "Daily run" section exactly.
2. My private ApplyPilot files are in {{WORKSPACE_PATH}}.
3. Only submit an application if the "Auto-apply" step's rules allow it: auto_submit is on, every answer on the form is one I already confirmed, and today's cap isn't reached. Otherwise leave it for me.
4. Finish by showing me today's report.
```
