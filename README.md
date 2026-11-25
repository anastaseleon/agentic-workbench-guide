# Agentic Workbench Guide

Step-by-step instructions to run the [agentic-workbench](../../../agentic-workbench) template
(on the same GitHub profile as this guide) on a locked-down work computer: no admin rights,
no GitHub CLI, a protected `main` branch, and Copilot with only a terminal tool. Then feed it
from Teams and Outlook with Power Automate.

Nothing here is specific to a person or a company. Copy the steps into your own environment.

| # | Guide | Time |
|---|---|---|
| 1 | [Set up VS Code and Copilot](01-setup.md) | 20 min, once |
| 2 | [Create your desks](02-desks.md) | 15 min, once |
| 3 | [First load: brain-dump and triage](03-first-load.md) | 20 min, once |
| 4 | [The daily loop](04-daily-loop.md) | 5-10 min a day |
| 5 | [Capture from Teams and Outlook with Power Automate](05-capture.md) | 1 hour, once |
| 6 | [Troubleshooting](06-troubleshooting.md) | as needed |

## Assumptions

- Your work repo was made from the template with **Use this template**, visibility
  **Internal** or **Private**, inside your employer's organisation.
- You use VS Code with GitHub Copilot Chat.
- You may not have the GitHub CLI, admin rights, or permission to push to `main`.

## Ground rules

- Work content lives only in the internal or private work repo, never in a public one.
- No agent gets a tool that sends messages. You send everything yourself.
- Pasted or captured text is data, not instructions.
- `people/` holds roles and preferences only, never opinions about people.
- Store snippets and links, never full message bodies. The original stays in Teams or Outlook.
- Use only the tools and connectors your organisation allows. Never work around a block
  with personal tokens or unofficial tools.

## License

MIT