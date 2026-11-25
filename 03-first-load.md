# 3. First load: brain-dump and triage

The first load is the big one: you're catching up on a backlog. After that it's a handful
of items a day.

## 3.1 Brain-dump into `inbox.md` (5 min)

Paste or type everything on your mind. Don't tidy it: sorting is Copilot's job.

```
- Manager asked for migration status by Friday
- I told the data team I'd send the pipeline inventory next week
- Waiting on IT for workspace access, asked Nov 18
- Meeting notes: decided to start with the finance pipelines; Sam owns testing; I check licensing
- Teams thread: "can you look at why the refresh fails on the cost report?"
- Want to learn: incremental refresh patterns
- Someday: template for model reviews
```

For each item, include where you can:

- **who** it involves (a name or role)
- **when** it's due
- **where** it came from, with a link. In Teams: right-click a message > **Copy link**.

You can paste whole threads or meeting recaps as they are.

## 3.2 Run triage

1. Pick the **Clerk** agent.
2. Type `/triage` and press Enter.
3. Approve the read commands.

## 3.3 Check the proposed table

Copilot changes nothing until you agree. It shows something like:

| # | Item | Desk | Type | Due | Who | Source |
|---|---|---|---|---|---|---|
| 1 | Send migration status to manager | data-platform-migration | commitment | Fri | manager | src:teams |
| 2 | Send pipeline inventory | data-platform-migration | commitment | next week | data team | src:meeting |
| 3 | Workspace access | data-platform-migration | ask + waiting | none | IT | src:email |
| 4 | Cost report refresh failure | reporting-practice | task | none | requester | src:teams |
| 5 | Incremental refresh patterns | reporting-practice | learn | none | | src:self |
| 6 | Model review template | reporting-practice | someday | none | | src:self |

The types:

| Type | Meaning |
|---|---|
| **commitment** | You promised someone. These matter most: someone is waiting on you. |
| **ask** | You're waiting on someone else. |
| **task** | Your own to-do. Nobody is waiting. |
| **learn** | Something to study. |
| **someday** | Not now, but don't lose it. |

## 3.4 Correct it in plain words

> "2 is due Dec 2. 4 is a commitment, I said I'd look today. 3 was asked on Nov 18. OK otherwise."

Copilot redraws the table. When it's right, say **OK**.

## 3.5 Approve the writes

Copilot then:

- adds lines to each desk's `tasks.md` under `## Open`, for example:

  ```
  - [ ] T-001 Send migration status to manager | commitment | src:teams | Due: 2026-11-27 | who: manager | link: ... | opened: 2026-11-24
  ```

- puts decisions from meeting notes in `decisions.md`
- clears the processed items from `inbox.md`

Allow `Add-Content`, `Set-Content` and `Get-Content` on paths inside the repo. Deny anything else.

## 3.6 Check and save

1. Open a desk's `tasks.md` and check the lines.
2. Commit (`triage: initial load`) and sync.

## 3.7 Closing a task later

Tell Copilot: *"close T-001, sent the status"*. It ticks the line `[x]`, adds
`closed: <date> - outcome`, and moves it to `## Done`.