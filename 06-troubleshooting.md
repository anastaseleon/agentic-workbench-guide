# 6. Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| No **Agent** option in Copilot Chat | Old VS Code, setting off, or org policy | VS Code 1.99+, tick `chat.agent.enabled`, update Copilot Chat. Still missing: ask your admin. ([1.4](01-setup.md#14-turn-on-agent-mode)) |
| Asked to sign in to GitHub every time | No credential store | `git config --global credential.helper manager` and `credential.credentialStore wincredman`. ([1.3](01-setup.md#13-stop-signing-in-every-time)) |
| "Open in VS Code" - does that mean cloning? | Yes | Cloning puts a local copy Copilot can work on. ([1.2](01-setup.md#12-clone-it-in-vs-code)) |
| `/new-project` runs but nothing is created | Copilot has no write tool, or it only described the steps | Use the Clerk agent in Agent mode with the terminal tool ticked, then approve the commands. ([1.5](01-setup.md#15-give-the-clerk-agent-its-tools)) |
| "The available file tool cannot copy or write" | No edit tool on this build | Tick **run in terminal** and add the terminal rule. ([1.6](01-setup.md#16-add-the-terminal-rule)) |
| Does Copilot remember what we talked about? | Only within the chat, and only what's in the repo | Put decisions in the desk files. Copilot rereads them every time. |
| Changes don't appear on github.com | Not committed or not synced | Source Control > Commit > Sync Changes. ([1.8](01-setup.md#18-saving-your-work)) |
| Push to `main` rejected | Branch protection | Work on a `desk` branch. ([1.7](01-setup.md#17-work-on-a-branch-if-main-is-protected)) |
| Can't change the default branch | Organisation setting | Stay on `desk`. Use `/digest` instead of the scheduled Action. |
| `/digest` shows old tasks | Wrong branch checked out | Switch to `desk` (bottom left of VS Code). |
| Copilot can't create issues | No GitHub tool, no gh CLI | Use `tracker: files`, or create issues yourself with the **Task** form on github.com. |
| bootstrap-labels creates no desk labels | Ran on `main`, which has no desk folders | Set **Use workflow from** to your working branch. |
| Triage is overwhelming | Capturing too much, untagged | Tag at capture with Flow A (`>>` lines); click only what needs action. ([5](05-capture.md)) |
| A flow writes blank fields | Dynamic-content name differs | Check Run history, read the trigger's raw output, fix the expression. ([5.4](05-capture.md#54-test-each-flow)) |
| Copilot proposes a command with a URL, mail, or `Remove-Item` | Outside its job | Deny it. If it happens twice, add a rule. |