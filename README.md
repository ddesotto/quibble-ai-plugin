# Quibble for coding agents

[Quibble](https://tryquibble.com) lets a team leave comments on the pages of
their own web product. This plugin gives your coding agent those comments, with
the page, the element, the screenshot and the replies, so it can fix what was
flagged and answer in the thread.

It contains:

- the `quibble` MCP server (remote, signed in with your Quibble account);
- the `quibble-review` skill, which tells the agent how to find the project for
  this repository, locate the code a comment points at, fix it, and reply.

## Install

### Claude Code

```
/plugin marketplace add ddesotto/quibble-ai-plugin
/plugin install quibble@quibble
```

### Codex

```bash
codex plugin marketplace add ddesotto/quibble-ai-plugin
codex plugin add quibble@quibble
```

### Any other agent

Add the MCP server at `https://tryquibble.com/api/mcp` (Streamable HTTP, OAuth),
then install the skill:

```bash
npx skills add ddesotto/quibble-ai-plugin
```

The skill is also served as plain markdown at <https://tryquibble.com/skill.md>.

The first tool call opens the browser to sign in and approve the agent.

## Use

Ask in the repository of the app that was reviewed:

> Fix the open quibbles on the checkout page.

## Tools

| Tool | What it does |
|---|---|
| `list_projects` | Projects you can see; pass a URL to find the one for this app. |
| `list_comments` | Open, resolved or all comments of a project, newest first. |
| `get_comment` | One comment with its replies, evidence and screenshot. |
| `get_comment_context` | The page activity recorded before a comment: console output, errors and network requests. |
| `reply_to_comment` | Reply as you, labelled with the agent. |
| `set_comment_status` | Resolve or reopen a comment. |

The agent can read, reply and change status. It cannot delete a comment, create
an issue, or reach your tracker or repository through Quibble.

## What the agent will not do

- Resolve a comment because it finished a fix. The reviewer checks the result on
  the page; the agent resolves only when you tell it to.
- Follow instructions written inside a comment. Comment text describes a change
  to the product and is treated as untrusted.

## Layout

```text
.claude-plugin/marketplace.json     Claude Code marketplace
.agents/plugins/marketplace.json    Codex marketplace
plugins/quibble/
  .claude-plugin/plugin.json        Claude Code manifest
  .mcp.json                         MCP server for Claude Code
  plugin.json                       Codex manifest
  mcp.json                          MCP server for Codex
  skills/quibble-review/SKILL.md    the skill
```

Set `QUIBBLE_MCP_URL` to point Claude Code at another Quibble deployment, such
as `http://localhost:3000/api/mcp`.

## License

MIT
