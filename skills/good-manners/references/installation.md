# Installation and Dependency Profiles

Use this module to audit or install Good Manners dependencies for a selected stack. The [shared development rules](../SKILL.md) remain in effect. The inventory below supports installation planning; the linked task modules define when each dependency is used.

## Selective setup

1. Determine the requested stacks and capabilities from the user's request and project manifests. Combine matching profiles, deduplicate names, and omit disabled preferences. If no project or selection is available, ask which stacks to prepare.
2. Inspect the installed skill catalog and plugin state. Count plugin-provided skills as installed; do not install another copy just because a namespaced catalog name differs from a folder name.
3. Report a concrete plan grouped into installed, missing, and unresolved dependencies. Separate the baseline for the selected stacks from capabilities needed only for a particular task. Leave optional capabilities unselected unless the user requests them or the current task needs them.
4. For each missing dependency, resolve an actual source through the catalog, plugin directory, or a verified repository containing its `SKILL.md`. Skill names alone are not repository URLs or curated-list entries. If the source cannot be resolved, request its plugin or repository link instead of inventing an install command.
5. Install only when the user has requested installation, and only the missing dependencies covered by that request. An audit or a request to edit these instructions does not authorize installing dependencies. Preserve existing installations and user edits; do not overwrite or upgrade unrelated skills.
6. Recheck the installed catalog or installed files and report what became available, what remains missing, and any activation step required by the installer. A plugin suggestion or downloaded archive alone is not proof of installation.

If a plugin bundles extra skills, disclose that installation granularity in the plan. Prefer individual verified skill paths when supported and when they preserve the required behavior. Never install every language profile to satisfy one selected stack.

## Installer capabilities

- Use `skill-installer` for standalone skills from its curated list or verified GitHub paths. Resolve and read it through the catalog; use its bundled installer scripts rather than copying their implementation here. It is normally preinstalled as a system skill, so check before trying to install it.
- For plugin-managed dependencies, use the available Plugin Management capabilities and apply `plugin-management:plugin-management` when available. Search by the actual plugin or provider name and follow the supported installation flow; do not treat a standalone skill installer as a plugin installer.
- `skill-creator` is needed when creating or changing skill packages, not for ordinary application development. It is also normally preinstalled.
- Resolve installation destinations through the active Codex environment and installer defaults. Keep this package independent of machine-specific paths and cached plugin versions.

## Baseline and task-independent capabilities

| Selection                                                         | Skills                                 |
| ----------------------------------------------------------------- | -------------------------------------- |
| Good Manners baseline, unless the user disables these preferences | `ponytail:ponytail`, `caveman`         |
| Writing agent instructions or reusable rules                      | `mattpocock-skills:writing-for-agents` |
| Reviewing changes against a commit, branch, tag, or merge base    | `mattpocock-skills:code-review`        |
| Diagnosing a difficult bug or performance regression              | `mattpocock-skills:diagnosing-bugs`    |
| Writing a pull request description                                | `mattpocock-skills:pr`                 |

Only the first row is a default dependency for every selected stack. The remaining rows are installed when preparing those capabilities or when the current task requires them.

## Stack profiles

| Profile                   | Baseline for that profile                                                                                                                      | Conditional additions                                                                                                                                                     |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Web](web.md)             | For frontend features: `modern-web-guidance`, provided by the [Modern Web Guidance plugin](plugin://modern-web-guidance@openai-curated-remote) | No additional skill for formatting or lint configuration alone; see the Web module's trigger conditions                                                                   |
| [React](react.md)         | `vercel-react-best-practices`; include Web for browser or WebView interfaces                                                                   | Shadcn MCP capability only when working with shadcn/ui; this is a tool dependency, not another named skill                                                                |
| [Rust](rust.md)           | `rust-skills:rust-router`                                                                                                                      | `rust-skills:unsafe-checker` for unsafe code or FFI; resolve further router-selected skills only for the requested task                                                   |
| [C#](csharp.md)           | No additional unconditional skill                                                                                                              | `csharp-code-review` for review, `csharp-async` for async work, `csharp-docs` for XML documentation; select the test-framework skill below when relevant                  |
| [PostgreSQL](database.md) | `postgres`                                                                                                                                     | `design-postgres-tables` for table/schema design; `postgres-database-migration` for schema changes or migrations; specialist skills below                                 |
| [Supabase](database.md)   | `supabase:supabase`, provided by the [Supabase plugin](plugin://supabase@openai-curated-remote)                                                | Include PostgreSQL for database work; add `supabase:supabase-postgres-best-practices` for database design or optimization; plugin tools for supported Supabase operations |

For an audit of an existing project, only missing dependencies for current work are required. For an explicit request to prepare a whole stack, include its baseline without enabling every conditional capability. A Supabase task unrelated to PostgreSQL, such as Storage-only work, does not require installing unused database skills.

Rust's router can require additional language-mechanics or domain skills, such as `rust-skills:coding-guidelines` for formatting and Clippy work. Inspect the installed router's instructions and expand the plan for the actual task rather than copying an entire, potentially changing Rust skill family into this inventory.

### C# test frameworks

Select the framework already used by the affected test project. Multiple test projects may justify more than one row; a single framework does not require the other three.

| Framework | Skill           |
| --------- | --------------- |
| MSTest    | `csharp-mstest` |
| NUnit     | `csharp-nunit`  |
| TUnit     | `csharp-tunit`  |
| xUnit     | `csharp-xunit`  |

### PostgreSQL specializations

Add only the skills matching the requested database capability.

| Capability                                  | Skill                                    |
| ------------------------------------------- | ---------------------------------------- |
| Spatial or geographic data                  | `design-postgis-tables`                  |
| Embeddings, RAG, or semantic search         | `pgvector-semantic-search`               |
| Combined keyword and semantic search        | `postgres-hybrid-text-search`            |
| TimescaleDB schema or hypertable design     | `setup-timescaledb-hypertables`          |
| Evaluating tables for hypertable conversion | `find-hypertable-candidates`             |
| Converting tables to hypertables            | `migrate-postgres-tables-to-hypertables` |

## Boundaries and example requests

Prettier, ESLint, i18next, React bindings, Vitest and its testing companions, rustfmt, Clippy, .NET analyzers, database drivers, and SDKs are project tooling or packages, not Codex skills. Setting up skill dependencies does not authorize adding them to an application or changing its database, credentials, or CI. Select web testing packages from the [Web testing rules](web.md#testing) and, for React components, the [React testing rules](react.md#testing).

Example requests:

- `Use $good-manners to audit missing skill dependencies for a React and PostgreSQL project.`
- `Use $good-manners to install missing Rust skill dependencies, including unsafe-code support.`
- `Use $good-manners to prepare C# code review and xUnit testing skills.`
