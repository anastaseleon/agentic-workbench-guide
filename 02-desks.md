# 2. Create your desks

A desk is a folder in `projects/`. A **project** ends (a migration, a rollout). A **practice**
doesn't (your role, a support area, a team standard).

## 2.1 Create a project desk

1. In Copilot Chat, pick the **Clerk** agent.
2. Type `/new-project` and answer: for example, *"Data Platform Migration, a project"*.
3. Approve the commands. Expect something like:

   ```
   Copy-Item -Recurse projects\_template-project projects\data-platform-migration
   Set-Content projects\data-platform-migration\README.md "kind: project ..."
   ```

4. Open the new `README.md`. The first two lines must be:

   ```
   kind: project
   tracker: files
   ```

Use `tracker: files` unless Copilot has a GitHub tool that can create issues. With files,
tasks are lines in the desk's `tasks.md`, which the terminal can write. You can switch to
`tracker: issues` later by changing that one line.

## 2.2 Fill in the README (5 min)

Write a few lines each for **Goal**, **Scope**, **Stakeholders** (roles only) and
**Done when**. Copilot answers from these, so concrete beats complete.

## 2.3 Create a practice desk

Run `/new-project` again, for example *"Reporting Practice, a practice"*. Fill in its
**Mandate**, **Stakeholders** and **Cadences**.

## 2.4 Save

Commit and sync (check you are on your `desk` branch if `main` is protected).

## 2.5 Labels (only for `tracker: issues`)

On github.com: **Actions > bootstrap-labels > Run workflow**. Set **Use workflow from** to
your working branch so it sees your desk folders. Run it again whenever you add a desk.

## 2.6 The example desk

`projects/example-atlas` is a fictional sample. `/digest` skips it. Read it once to see
the format, then delete it whenever you like.