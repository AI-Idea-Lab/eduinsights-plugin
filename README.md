# EduInsights agent plugin

Install EduInsights workflows and its read-only MCP server in Claude Code or Codex.

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
/eduinsights:audit-eduinsights-evidence
/eduinsights:draft-eduinsights-brief
```

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

Connect `https://mcp.eduinsights.ai/mcp` when your client supports MCP but not plugins. This exposes the tools and MCP prompts, not the complete skill instructions.

## Install in Codex from a local checkout

In the EduInsights monorepo, add its local catalog and install the plugin:

```bash
codex plugin marketplace add /absolute/path/to/eduinsights
codex plugin add eduinsights@eduinsights
```

Start a new Codex task after installation so it loads the plugin's skills and MCP server.
