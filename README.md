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
- `ai-mkt-info` - Displays the current marketplace and `project-document`
  plugin versions.
- `email-to-action` - Turns a specified email and optional attachment into
  evidence-backed analysis and supporting artifacts.
- Vendored from [Matt Pocock's skills](https://github.com/mattpocock/skills):
  `grill-me`, `grilling`, `grill-with-docs`, `handoff`, `ask-matt`, `implement`,
  `to-spec`, `setup-matt-pocock-skills`, `triage`, `retro`, `to-tickets`,
  `implement-spec`, and `domain-modeling`.
- Supporting upstream skills: `tdd` and `code-review` (used by `implement`
  and `implement-spec`), `codebase-design` (used by `tdd` when designing
  interfaces), and `writing-for-agents` (used by `retro`).

## Included plugins

- `project-document` - Packages the `project-document`, `ai-mkt-info`, and
  `email-to-action` skills as an installable Agent Plugin.
- `mattpocock-skills` - Packages the selected upstream skills and their
  supporting dependencies as a separate installable Agent Plugin.

Install the marketplace and plugin locally with:

```bash
copilot plugin marketplace add .
copilot plugin install project-document@ai-marketplace
copilot plugin install mattpocock-skills@ai-marketplace
```

## Vendored Matt Pocock skills

The standalone copies live in `skills/<name>/`; the plugin copies live in
`plugins/mattpocock-skills/skills/<name>/`. Both include complete upstream skill
directories, including reference documents, setup templates, and agent metadata.
Vendoring does not install or invoke the skills in the current session.

The files are vendored from upstream version `1.3.1`, pinned to commit
[`49dd158d1076134a641b33efb035946536778336`](https://github.com/mattpocock/skills/tree/49dd158d1076134a641b33efb035946536778336).
Source paths and the selected dependency set are recorded in
[`plugins/mattpocock-skills/UPSTREAM.json`](plugins/mattpocock-skills/UPSTREAM.json).
The upstream MIT license and copyright notice are retained in
[`skills/LICENSE.mattpocock`](skills/LICENSE.mattpocock) and
[`plugins/mattpocock-skills/LICENSE`](plugins/mattpocock-skills/LICENSE).

Local modifications enable model invocation for `implement` and `implement-spec`
in both destinations: `disable-model-invocation: false` in `SKILL.md` and
`policy.allow_implicit_invocation: true` in `agents/openai.yaml`. All other
vendored files remain unchanged from the pinned upstream revision.

Run `/setup-matt-pocock-skills` in the repository where you intend to use the
engineering flows to configure its issue tracker and domain documentation.
Setup is not run as part of vendoring.

`domain-modeling` was already bundled as a dependency of `grill-with-docs` and
is also available directly. `triage` is the upstream skill name (not `trage`).
When adding `triage` to a repository that was already configured, run
`/setup-matt-pocock-skills` again to configure the triage label vocabulary.

`ask-matt` retains the upstream overview of the full collection. Optional flows
it references, such as `prototype`, `pr`, `wayfinder`, and `diagnosing-bugs`,
are not included here; install them separately before using those routes.
The bundled skills remain subject to the host's tool availability, permissions,
and repository instructions. In particular, upstream `implement` requests a
commit and `implement-spec` creates branches and worktrees; these do not override
host or repository rules requiring explicit user approval for those operations.

To refresh the vendored collection, fetch an upstream snapshot at a specific
commit, replace each mapped directory in both destinations, reapply the local
modifications recorded in `UPSTREAM.json`, retain both license copies, and
update `UPSTREAM.json`. Keep the standalone and plugin copies byte-for-byte
identical; do not edit only one copy.