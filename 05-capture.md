# 5. Capture from Teams and Outlook with Power Automate

Don't capture everything. Most traffic doesn't involve you. Capture what you **promised**,
what you're **waiting on**, what's **asked of you**, and **decisions**.

Every flow below appends one line to the same OneDrive file, `desk-inbox.txt`. Each morning
you copy that file into `inbox.md`, clear it, and run `/triage`.

## 5.1 Two kinds of lines

| Starts with | Meaning | Triage does |
|---|---|---|
| `>>` | You classified it when you captured it | Files it directly, no questions |
| `??` | Captured automatically, not classified | Shows it in the table for you to decide |

Add this to `.github/prompts/triage.prompt.md` in your work repo:

```
Lines starting with ">>" are already classified by me. File them directly with the
type, desk and due given; do not ask about them. Lines starting with "??" are captured
but unclassified: include them in the table. Show a one-line count of what you filed.
```

## 5.2 Which flows to build

| Flow | What it catches | Build |
|---|---|---|
| A. Click to capture | Any Teams message you pick, already tagged | First |
| B. Flagged email | Emails you flag | First |
| C. @mentions | Teams posts that mention you | After a week, if you miss things |
| D. Morning sweep | High-importance mail and keyword posts | Only if A-C still miss things |

All of them use standard connectors: Microsoft Teams, Office 365 Outlook, OneDrive for
Business and Content Conversion. If your organisation's data policy blocks a combination,
don't work around it.

The flows run as you, so they only see what you can already see.

Before you start, create an empty `desk-inbox.txt` in your OneDrive.

---

## Flow A: Click to capture (Teams)

The flow doesn't go looking for messages. When you click **... > More actions > your flow**
on a message, Teams sends that one message to the flow, with your answers from a small card.

**1. Trigger:** Microsoft Teams > **For a selected message (V2)**. Click **Create Adaptive
Card** and add these inputs (the IDs in brackets are used below):

- **Type** (`type`), choice: commitment, ask, task, learn, someday, decision
- **Desk** (`desk`), choice: your desk folder names
- **Due** (`due`), date, optional
- **Note** (`note`), text, optional

**2. Html to text** (Content Conversion). Content: the message body.

**3. Compose**, renamed `snippet`:

```
take(replace(replace(body('Html_to_text'), decodeUriComponent('%0A'), ' '), decodeUriComponent('%0D'), ''), 200)
```

**4. Compose**, renamed `line`:

```
concat('>> ', triggerBody()?['cardOutputs']?['type'], ' | ', triggerBody()?['cardOutputs']?['desk'],
' | Due: ', coalesce(triggerBody()?['cardOutputs']?['due'], 'none'),
' | who: ', triggerBody()?['messagePayload']?['from']?['user']?['displayName'],
' | note: ', coalesce(triggerBody()?['cardOutputs']?['note'], outputs('snippet')),
' | link: ', triggerBody()?['messagePayload']?['linkToMessage'],
' | src:teams | ', formatDateTime(triggerBody()?['messagePayload']?['createdDateTime'], 'yyyy-MM-dd'),
decodeUriComponent('%0A'))
```

**5. Append to file** (OneDrive for Business) on `desk-inbox.txt`, content: `line`. If your
tenant has no append action, use **Get file content**, then **Update file** with the old
content followed by `line`.

**6. Optional:** **Respond to the user** with "Captured".

What the trigger gives you, if a dynamic-content name differs from the expressions above:

| You want | Expression |
|---|---|
| Message text (HTML) | `triggerBody()?['messagePayload']?['body']?['content']` |
| Who wrote it | `triggerBody()?['messagePayload']?['from']?['user']?['displayName']` |
| When | `triggerBody()?['messagePayload']?['createdDateTime']` |
| Link back | `triggerBody()?['messagePayload']?['linkToMessage']` |
| Card answers | `triggerBody()?['cardOutputs']?['<input id>']` |

Works on channel posts, chats and meeting chats. For a reply in a thread, click the reply
itself. Spoken meeting content isn't covered: see 5.3.

## Flow B: Flagged email (Outlook)

Flagging already means "this needs action", so only flagged mail is captured.

**1. Trigger:** Office 365 Outlook > **When an email is flagged** (latest version). Folder: Inbox.

**2. Compose**, renamed `line`:

```
concat('?? email | who: ', triggerOutputs()?['body/from'],
' | subject: ', triggerOutputs()?['body/subject'],
' | preview: ', take(replace(triggerOutputs()?['body/bodyPreview'], decodeUriComponent('%0D%0A'), ' '), 150),
' | src:email | ', formatDateTime(triggerOutputs()?['body/receivedDateTime'], 'yyyy-MM-dd'),
decodeUriComponent('%0A'))
```

If the trigger offers a web link field, add `' | link: ', <web link>` before `src:email`.
Otherwise the subject and date are enough to find the email again.

**3. Append to file** `desk-inbox.txt`, content: `line`.

**Optional, tag at capture:** create Outlook categories named `commitment`, `ask` and
`task`. Add a **Condition**: if the email has one of them, start the line with
`>> <category> |` instead of `??`. Triage then files it without asking.

## Flow C: When you're @mentioned (Teams)

**1. Trigger:** Microsoft Teams > **When I am @mentioned** (latest version). Pick the team and
channel. Make one copy per important channel.

**2. Html to text** on the message body.

**3. Compose**, renamed `line`:

```
concat('?? teams | who: ', triggerBody()?['from']?['user']?['displayName'],
' | text: ', take(replace(body('Html_to_text'), decodeUriComponent('%0A'), ' '), 200),
' | link: ', triggerBody()?['webUrl'],
' | src:teams | ', formatDateTime(triggerBody()?['createdDateTime'], 'yyyy-MM-dd'),
decodeUriComponent('%0A'))
```

**4. Append to file** `desk-inbox.txt`.

## Flow D: Morning sweep (scheduled)

Catches what you didn't flag or click.

**1. Trigger:** **Recurrence**, daily at 7:00, weekdays, your time zone.

**2. Outlook branch:**

- **Get emails** (latest version). Folder: Inbox. Top: 50. Search query:
  `received>=yesterday AND to:me AND importance:high`
- **Apply to each** email: Compose a `??` line like Flow B, then **Append to file**.

**3. Teams branch,** one per key channel:

- **Get messages** (team, channel).
- **Filter array**, advanced mode:

  ```
  @and(
    greater(item()?['createdDateTime'], addDays(utcNow(), -1)),
    or(
      contains(toLower(item()?['body']?['content']), 'platform migration'),
      contains(toLower(item()?['body']?['content']), 'your name')
    )
  )
  ```

- **Apply to each** result: Html to text, Compose a `??` line like Flow C, then **Append to file**.

Keep the keyword list short: your name, your desk names, "deadline". A broad filter brings
the volume back.

## 5.3 Meetings

Flows can't capture what was said. Use the best of these you have:

- **Microsoft 365 Copilot:** in Copilot Chat (Work), ask: *"List everything from my chats,
  channels, meetings and email since yesterday 8am that involves me: commitments, things
  asked of me, things I'm waiting on, decisions. One line each with who, due date and a
  link. Skip FYIs."* Paste the answer into `inbox.md`.
- **Meeting recap:** in the Teams calendar, open the meeting > **Recap** > AI notes or
  follow-up tasks, and copy the action items.
- **Transcript only:** copy the parts with actions, decisions and owners from the Recap tab.
- **Nothing:** keep three lines during the meeting (decided / I owe / they owe).

## 5.4 Test each flow

1. Trigger it once: click a test message, flag a test email, or run the sweep manually.
2. **My flows > the flow > Run history**, open the latest run, and check each step's
   inputs and outputs. A blank field means the expression's name differs: look at the
   trigger's raw output and adjust.
3. Check `desk-inbox.txt` has one clean line.
4. After the first full day, if the file has more than about 15 lines, tighten filters
   before adding another flow.

## 5.5 Your morning with the flows

1. Open `desk-inbox.txt`, copy everything into `inbox.md`, and clear the file.
2. `/triage`: it files the `>>` lines and shows a table for the `??` lines.
3. Say "drop 3, 5 and 7" for noise, approve, then commit and sync.
4. `/digest`.

## 5.6 Rules

- Store a snippet (150-200 characters) and a link, never full bodies.
- Only click what needs action from you or someone else. FYIs don't go in.
- Use **decision** for things decided in chat: they go to `decisions.md`, not to tasks.
- Never capture HR, salary or other confidential material.