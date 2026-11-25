# 1. Set up VS Code and Copilot

## 1.1 Create your work repo

1. Open the template on github.com and click **Use this template > Create a new repository**.
2. **Owner:** your organisation. **Visibility:** Internal or Private. Give it a name.
3. Click **Create repository**.

## 1.2 Clone it in VS Code

Cloning puts a copy on your computer that Copilot can read and change.

1. On your new repo page, click **Code** and copy the HTTPS URL.
2. In VS Code, press **Ctrl+Shift+P**, run **Git: Clone**, and paste the URL.
3. Pick a folder you own, for example `Documents\repos`, then click **Open**.
4. Sign in to GitHub when asked.

## 1.3 Stop signing in every time

In the VS Code terminal (**Ctrl+`**), run:

```
git config --global credential.helper manager
git config --global credential.credentialStore wincredman
```

Sign in once more. To check it worked, open **Windows Credential Manager > Windows
Credentials** and look for an entry starting with `git:https://github.com`.

If VS Code still asks for a device code each time, click the **Accounts** icon (bottom
left) and check you are signed in. If that doesn't hold either, credential storage may be
blocked on your machine: ask IT.

## 1.4 Turn on agent mode

1. Open Copilot Chat (**Ctrl+Alt+I**).
2. In the mode picker under the chat box, choose **Agent**.

If there is no Agent option:

- **Help > About**: VS Code must be 1.99 or later. Update if you can.
- **Settings** (Ctrl+,): search `chat.agent.enabled` and tick it.
- **Extensions**: update **GitHub Copilot Chat**.
- Still missing: your organisation's Copilot policy has it switched off. Ask your admin.

Custom agents (Clerk, Tutor, Reviewer) live in `.github/agents/`. Older VS Code builds use
`.github/chatmodes/` instead: see `.github/agents/README.md` in the template.

## 1.5 Give the Clerk agent its tools

1. Open `.github/agents/clerk.agent.md`.
2. Click **Configure Tools...** above the frontmatter, or the tools icon in the chat box.
3. Tick **read**, **search**, **edit** if you have it, and **run in terminal**.
4. Save.

If there is no edit or create-file tool, Copilot writes files through the terminal with
commands like `Copy-Item` and `Set-Content`. It's slower, but the result is the same.

**Keep terminal auto-approve off.** You read every command before you click **Allow**.
That is your safety catch: the terminal can do anything your account can.

## 1.6 Add the terminal rule

Add this at the end of `.github/copilot-instructions.md`:

```
## Terminal use (no file-edit tool on this machine)
Create and change files only with terminal commands inside this workspace:
Copy-Item, New-Item, Set-Content, Add-Content, Get-Content, git add, git commit.
Show each command before running it. Never use network commands
(Invoke-WebRequest, Invoke-RestMethod, curl, Send-MailMessage), never Remove-Item,
never touch files outside this workspace.
```

When approving commands:

- **Allow:** copying, creating or writing files inside the repo folder.
- **Deny:** anything with a web address, mail, `Remove-Item`, or a path outside the repo.

## 1.7 Work on a branch if `main` is protected

If you can't push to `main`, work on your own branch and never merge.

1. Click the branch name **main** at the bottom left of VS Code.
2. Choose **Create new branch...** and name it `desk`.
3. Commit as usual, then click **Publish Branch**. After that, use **Sync Changes**.

If you're allowed, make `desk` the default branch: **Settings > General > Default branch**.
If you can't, stay on `desk` anyway:

| Thing | On a non-default branch |
|---|---|
| Copilot and every prompt | Works: it reads whichever branch is checked out |
| Commit and sync | Works |
| bootstrap-labels | Works: set **Use workflow from** to `desk` |
| Scheduled digest Action | Doesn't run: use `/digest` in VS Code instead |
| Stale Action | Doesn't matter with `tracker: files` |

**Before every prompt, check the bottom left says `desk`.** If it says `main`, Copilot reads
old notes.

Optional: open a pull request from `desk` to `main` now and then if you want `main` to
show your desks. Nothing depends on it.

## 1.8 Saving your work

Copilot changes files on your computer, not on GitHub. Nothing is saved on GitHub until you
commit and sync.

1. **Source Control** (Ctrl+Shift+G).
2. Type a message, click **Commit**, then **Sync Changes**.

You can also ask Copilot "commit my changes" and approve the `git add` and `git commit`
commands. You still click **Sync** to push.