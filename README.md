# Jurnie plugins

Connect Codex to your [Jurnie](https://www.jurnie.co/) account. The plugin combines a Jurnie workflow skill with Jurnie's OAuth-protected remote MCP server so Codex can help review personal and shared Journeys, plan your day, manage and assign tasks, track habits, set intentions, work with calendar events, and schedule follow-through.

The same Jurnie skill and MCP service also power the Claude and Cursor packages. Marketplace review is separate for each platform; a package in this repository does not mean its directory listing is approved.

## One source, three clients

```text
plugin-source/  (one skill, logo, metadata, and release version)
  -> npm run build
     -> plugins/jurnie         Codex and portable ChatGPT package
     -> plugins/jurnie-claude  Claude package
     -> plugins/jurnie-cursor  Cursor package

All clients -> https://www.jurnie.co/api/mcp (one hosted implementation)
```

Edit `plugin-source/`, then run `npm run build` and `npm run check` with Node.js 20 or newer. Commit the generated packages alongside their source so marketplaces can read them directly. Do not edit the generated package files. `npm run check` fails if their content drifts from the shared source. Change the release version once in `plugin-source/metadata.json`.

Platform differences stay in the small packaging script: Claude and Codex use native remote HTTP; Cursor uses a pinned `mcp-remote` bridge for OAuth callback compatibility. No credentials or account tokens are packaged. Marketplace approvals can land at different times even though all packages share the same source version.

## Claude and Cursor

- [Claude public setup](https://www.jurnie.co/help/mcp#claude)
- [Cursor public setup](https://www.jurnie.co/help/mcp#cursor)

For Claude Code, this repository also provides a marketplace manifest:

```text
/plugin marketplace add jirayu3141/jurnie-codex-plugin
/plugin install jurnie-goals@jurnie-plugins
```

These direct setup routes work independently of directory review.

Claude lists the package as **Jurnie Goals & Tasks** (`jurnie-goals`) to distinguish this jurnie.co service from the separate jurnie.io meeting-notes connector. This is a packaging identity difference; the skill, server, and release version remain shared.

## Install in Codex

Add this repository as a Codex marketplace, then install Jurnie:

```bash
codex plugin marketplace add jirayu3141/jurnie-codex-plugin
codex plugin add jurnie@jurnie-plugins
```

Start a new Codex task, invoke `$jurnie`, and complete the Jurnie OAuth consent flow when prompted.

## Example prompts

- `Use $jurnie to review my priorities and plan today.`
- `Use $jurnie to show overdue tasks and suggest my next three actions.`
- `Use $jurnie to create a task from this conversation.`
- `Use $jurnie to assign the launch task to Gitar.`

## Access and safety

Jurnie requests scoped permissions during OAuth authorization. Read and write capabilities are separated, destructive operations require an additional scope, and MCP tool calls are audited by Jurnie. The plugin contains no credentials.

- [Jurnie MCP setup](https://www.jurnie.co/help/mcp)
- [Privacy policy](https://www.jurnie.co/privacy)
- [Terms of service](https://www.jurnie.co/terms)

## License

MIT
