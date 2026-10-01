# AI Marketplace

A workspace for building and publishing AI plugins, skills, agents, prompts,
instructions, MCP servers, and supporting tools.

## Structure

```text
.
├── .github/
│   ├── agents/          # Repository-level custom agents
│   ├── instructions/    # Path-specific Copilot instructions
│   └── prompts/         # Reusable prompt files
├── docs/                # Marketplace and authoring documentation
├── examples/            # Example integrations and usage
├── mcp-servers/         # MCP server implementations
├── packages/            # Shared libraries and reusable code
├── plugins/             # Installable plugin packages
├── scripts/             # Development and release automation
├── skills/              # Standalone skills
├── tests/               # Marketplace-wide validation
└── marketplace.json     # Copilot CLI marketplace catalog
```

Each plugin should live in its own directory under `plugins/` and contain a
`plugin.json` manifest. Agent Plugins can bundle portable skills under
`skills/` and Copilot-specific components under `com.github.copilot/`.

## Included skills

- `project-document` - Creates or formats a project metadata document as
  `PROJECT.md`.

## Included plugins

- `project-document` - Packages the `project-document` skill as an installable
  Agent Plugin.

Install the marketplace and plugin locally with:

```bash
copilot plugin marketplace add .
copilot plugin install project-document@ai-marketplace
```