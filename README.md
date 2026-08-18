# EduInsights agent plugin

Install EduInsights workflows and its read-only MCP server in Claude Code, ChatGPT desktop, or Codex.

The plugin researches U.S. colleges, reported fields, careers, workforce measures, accreditation, and observed AI use. Every answer can show its sources.

## Install in Claude Code

Add the public plugin repository as a custom catalog. No marketplace approval is required.

```bash
claude plugin marketplace add AI-Idea-Lab/eduinsights-plugin
claude plugin install eduinsights@eduinsights
```

Start a new Claude Code session after installation. You can inspect the installed components at any time.

```bash
claude plugin details eduinsights@eduinsights
```

Claude can select a workflow from your request. You can also invoke one directly:

```text
/eduinsights:research-with-eduinsights
/eduinsights:understand-eduinsights-ontology
/eduinsights:audit-eduinsights-evidence
/eduinsights:draft-eduinsights-brief
```

## Install in ChatGPT desktop or Codex

Add the public plugin repository as a marketplace, then install the plugin:

```bash
codex plugin marketplace add AI-Idea-Lab/eduinsights-plugin
codex plugin add eduinsights@eduinsights
```

Start a new ChatGPT desktop conversation or Codex task after installation. The plugin loads the skills and MCP server together.

## Update an installed plugin

Treat Codex updates as manual. Refresh the marketplace, install the latest published version, and verify the result:

```bash
codex plugin marketplace upgrade eduinsights
codex plugin add eduinsights@eduinsights
codex plugin list --json
```

Start a new ChatGPT desktop conversation or Codex task after the update.

Claude Code can update third-party plugins automatically after you enable the marketplace setting. It is off by default for third-party marketplaces.

Run `/plugin`, open **Marketplaces**, select `eduinsights`, and choose **Enable auto-update**. For a manual update, run:

```bash
claude plugin marketplace update eduinsights
claude plugin update eduinsights@eduinsights
```

Run `/reload-plugins` inside an open Claude Code session, or start a new session.

## Test the plugin

Follow the [dated testing guide](./docs/testing.md) to test the published plugin, connect only the MCP server, or load a local checkout.

The guide records what is ready on each surface, the prompts to test, and the official OpenAI and Anthropic references used to verify the steps.

## Test a local checkout

Load the plugin for one Claude Code session without installing it:

```bash
claude --plugin-dir .
```

Validate the plugin and catalog before publishing changes:

```bash
claude plugin validate . --strict
```

## Connect only the MCP server

Connect `https://mcp.eduinsights.ai/mcp` when your client supports MCP but not plugins. This fallback exposes the tools and MCP prompts, not the complete skill instructions.

## Test a local checkout in Codex

In the EduInsights monorepo, add its local catalog and install the plugin:

```bash
codex plugin marketplace add /absolute/path/to/eduinsights
codex plugin add eduinsights@eduinsights
```

Start a new Codex task after installation so it loads the plugin's skills and MCP server.
