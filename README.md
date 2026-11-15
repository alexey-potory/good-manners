# Good Manners

<img src="assets/good-manners.png" alt="Good Manners icon" width="128" height="128">

Shared development conventions for Codex, with task-specific guidance for Web, React, Rust, C#, and databases.

Version: **0.1.0**. Skill name: **`good-manners`**.

## Installation

Clone or download this repository into a directory named `good-manners` inside your Codex skills directory. Use `$CODEX_HOME/skills` when configured, or the default `~/.codex/skills` location.

Keep `SKILL.md`, `agents/`, `references/`, and `assets/` together. All internal links and icon paths are relative.

To install from GitHub through `skill-installer`, provide the actual URL of this repository after publishing it. Copying Good Manners does not automatically install other skills, plugins, or application packages.

## Usage

Invoke the skill as `$good-manners`, followed by your task. Automatic skill selection remains enabled.

```text
Use $good-manners to review the changes in this project.
Use $good-manners to audit missing skills for React and PostgreSQL.
Use $good-manners to install only the missing skills for Rust unsafe-code work.
```

User requirements and project-specific instructions take precedence over skill defaults.

## Modules

| Module                                     | Purpose                                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------ |
| [Shared conventions](SKILL.md)             | Scope, language, code layout, review, verification, and Git conventions  |
| [Installation](references/installation.md) | Dependency inventory and selective setup by stack and capability         |
| [Web](references/web.md)                   | Browser guidance, themes, localization, formatting, linting, and testing |
| [React](references/react.md)               | React-specific components, bindings, linting, and tests                  |
| [Rust](references/rust.md)                 | Rust skills, formatting, and Clippy                                      |
| [C#](references/csharp.md)                 | C# skills, formatting, and analyzers                                     |
| [Databases](references/database.md)        | PostgreSQL and Supabase access, migrations, and permissions              |

The main skill loads only the modules relevant to a task. These modules are references inside one skill, rather than separately registered skills.

## Dependencies

Good Manners references external skills and plugins without bundling them. Use the [installation module](references/installation.md) to inspect installed dependencies, select stacks, and install only what the requested work needs.

Application tools such as Vitest, ESLint, or database drivers are separate from Codex skill dependencies.

## Maintenance

Edit the common conventions in `SKILL.md` and keep technology-specific additions in their modules. Avoid duplicating rules between files.

Validate the repository root with the `skill-creator` skill's `quick_validate.py` helper. Check Markdown/YAML formatting and relative links before publishing changes. This repository contains instructions and assets, not a standalone application.

The icon was generated with the built-in image generation tool; its [generation prompt](assets/good-manners.prompt.txt) is included.

## License

A distribution license has not been selected for this repository.
