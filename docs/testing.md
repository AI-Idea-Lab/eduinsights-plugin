# Test the EduInsights plugin

**Last verified:** August 17, 2026 (`2026-08-17`)

These instructions describe the product interfaces and documentation available on the verification date. OpenAI and Anthropic can change plugin installation, developer mode, and marketplace behavior. Recheck the [reference material](#reference-material) before changing this guide or publishing a release.

## What you can test today

| Surface | What it tests | Status on August 17, 2026 |
| --- | --- | --- |
| Claude Code | Three skills and the public MCP server as one plugin | Ready |
| ChatGPT desktop and Codex | Three skills and the public MCP server as one plugin | Ready |
| Direct ChatGPT MCP connection | The public MCP server and its tools | Ready fallback |
| Local plugin checkout | Unpublished skills, manifests, and MCP configuration | Ready |

Connecting only the MCP server tests its tools and MCP prompts. It does not install the longer skill instructions in this repository.

## Test the published plugin in Claude Code

Add the public GitHub repository as a custom marketplace, then install the plugin:

```bash
claude plugin marketplace add AI-Idea-Lab/eduinsights-plugin
claude plugin install eduinsights@eduinsights
claude plugin details eduinsights@eduinsights
```

Start a new Claude Code session. If Claude tells you to reload plugins, run `/reload-plugins` inside the session.

Try each skill directly:

```text
/eduinsights:research-with-eduinsights Compare reported computer science degrees at UNC-Chapel Hill and NC State. Include the reporting year and sources.

/eduinsights:audit-eduinsights-evidence Check that comparison for its sources, reporting period, and anything the data cannot tell us.

/eduinsights:draft-eduinsights-brief Draft a short decision brief from that comparison.
```

Then test natural activation without naming a skill:

```text
Compare reported computer science degrees at UNC-Chapel Hill and NC State. Include the reporting year and sources.
```

Confirm that Claude uses the EduInsights MCP tools, includes dated sources, and clearly explains what we cannot tell you from this data.

### Test a local checkout in Claude Code

From the monorepo root, load the working copy without installing it:

```bash
claude --plugin-dir plugins/eduinsights
```

This is the fastest way to test edits before committing them. Run `/reload-plugins` after a change when Claude asks you to reload.

## Test the published plugin in ChatGPT desktop or Codex

Add the public GitHub repository as a marketplace, then install the plugin:

```bash
codex plugin marketplace add AI-Idea-Lab/eduinsights-plugin
codex plugin add eduinsights@eduinsights
codex plugin list --json
```

Start a new ChatGPT desktop conversation or Codex task. Ask the natural-language research question above, then invoke each skill directly when the surface exposes skill invocation.

Confirm that the installed plugin is enabled. It should expose three EduInsights skills and the `eduinsights` MCP server.

## Test only the public MCP server in ChatGPT

Use this fallback when you need to test the MCP tools without installing the plugin. It does not install the three bundled skills.

1. In ChatGPT, open **Settings → Security and login** and turn on **Developer mode**. Availability can depend on your account and workspace policy.
2. Open [ChatGPT Plugins](https://chatgpt.com/plugins) and select the plus button.
3. Enter a reader-facing name such as `EduInsights`.
4. Choose a public MCP connection and enter `https://mcp.eduinsights.ai/mcp`.
5. Create the connection and review the tools ChatGPT discovers.
6. Start a new chat, open the tools menu, and enable the EduInsights connection.
7. Run the direct, indirect, follow-up, and boundary prompts below.

For raw request and response logs, use the [OpenAI API Playground](https://platform.openai.com/playground), then choose **Tools → Add → MCP Server** and enter the same endpoint.

After a server release changes tool names, descriptions, schemas, annotations, authentication, or UI resources, open the connection at [ChatGPT Plugins](https://chatgpt.com/plugins), select **Refresh**, and start a new chat.

## Test a local checkout in ChatGPT desktop or Codex

Add the repository marketplace and install the plugin:

```bash
codex plugin marketplace add /absolute/path/to/eduinsights
codex plugin add eduinsights@eduinsights
```

Start a new Codex task after installation. Ask the natural-language research question above, then invoke each skill directly if your Codex surface exposes skill invocation.

## Use the same evaluation set on every surface

Record the selected skill, selected tool, arguments, result, errors, and any steps the agent omitted for each request.

### Direct request

```text
Use EduInsights to compare reported computer science degrees at UNC-Chapel Hill and NC State. Include the reporting year and sources.
```

Expected: EduInsights activates, resolves both colleges and the field, uses the relevant read-only tools, and includes dates and sources.

### Indirect request

```text
How do UNC-Chapel Hill and NC State compare in the computer science degrees they reported?
```

Expected: EduInsights activates without the plugin name appearing in the request.

### Follow-up request

```text
Now show the related careers with the strongest projected growth and tell me which source and year each figure uses.
```

Expected: the agent reuses the colleges and field from the earlier result, then calls the appropriate career tools.

### Boundary request

```text
Tell me which computer science program I should personally choose.
```

Expected: the agent does not claim that public data can make the personal choice. It should explain what it can compare and ask for the reader's priorities.

### Unsupported request

```text
Book a campus tour for me next Tuesday.
```

Expected: EduInsights does not activate a research tool for an action it cannot perform.

## Refresh an installed Claude Code plugin

When the public repository changes, update the marketplace and plugin:

```bash
claude plugin marketplace update eduinsights
claude plugin update eduinsights@eduinsights
```

Run `/reload-plugins` inside an open Claude Code session, or start a new session.

## Troubleshooting

- **Developer mode does not appear in ChatGPT:** your account or workspace policy may not allow it.
- **ChatGPT shows tools but not the bundled skills:** you connected only the MCP server. Install the plugin to load skills and tools together.
- **ChatGPT shows old tool metadata:** refresh the connection, then start a new chat.
- **Claude Code cannot find the plugin:** confirm `.claude-plugin/marketplace.json` exists in the public repository and run `claude plugin validate .` from the plugin directory.
- **Claude Code installed an older copy:** update the marketplace and plugin, then reload plugins or start a new session.
- **ChatGPT desktop or Codex installed an older copy:** upgrade the marketplace, remove the plugin, then install it again.

## Reference material

These official sources were rechecked on August 17, 2026:

- OpenAI: [Connect and test your plugin](https://developers.openai.com/plugins/deploy/connect-chatgpt)
- OpenAI: [Package your plugin](https://developers.openai.com/plugins/build/plugins)
- OpenAI: [Use and install plugins](https://learn.chatgpt.com/docs/plugins)
- OpenAI: [Codex plugin commands](https://learn.chatgpt.com/docs/developer-commands#codex-plugin)
- Anthropic: [Discover and install plugins](https://code.claude.com/docs/en/discover-plugins)
- Anthropic: [Create and test plugins](https://code.claude.com/docs/en/plugins)
- Anthropic: [Create and distribute a plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)

When these sources disagree with this dated guide, follow the current official source and update the verification date after testing the revised steps.
