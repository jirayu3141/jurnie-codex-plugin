# Jurnie for Codex

Connect Codex to your [Jurnie](https://www.jurnie.co/) account. The plugin combines a Jurnie workflow skill with Jurnie's OAuth-protected remote MCP server so Codex can help review Journeys, plan your day, manage tasks, track habits, set intentions, work with calendar events, and schedule follow-through.

## Install

Add this repository as a Codex marketplace, then install Jurnie:

```bash
codex plugin marketplace add jirayu3141/jurnie-codex-plugin
codex plugin add jurnie@jurnie
```

Start a new Codex task, invoke `$jurnie`, and complete the Jurnie OAuth consent flow when prompted.

## Example prompts

- `Use $jurnie to review my priorities and plan today.`
- `Use $jurnie to show overdue tasks and suggest my next three actions.`
- `Use $jurnie to create a task from this conversation.`

## Access and safety

Jurnie requests scoped permissions during OAuth authorization. Read and write capabilities are separated, destructive operations require an additional scope, and MCP tool calls are audited by Jurnie. The plugin contains no credentials.

- [Jurnie MCP setup](https://www.jurnie.co/help/mcp)
- [Privacy policy](https://www.jurnie.co/privacy)
- [Terms of service](https://www.jurnie.co/terms)

## License

MIT
