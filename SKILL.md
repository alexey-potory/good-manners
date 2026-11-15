---
name: good-manners
description: Apply shared development conventions when implementing, reviewing, or refactoring project code, tests, database schemas, or developer tooling, or setting up this skill's dependencies for a selected stack. Load only the task-relevant modules.
metadata:
  version: "0.1.0"
  short-description: "Shared coding conventions with task-specific modules"
---

# Good Manners

These rules apply across languages and frameworks. Load technology-specific supplements only when their conditions match the task.

Apply these conventions when this skill is active. Explicit user requirements and project-specific instructions take precedence over skill defaults. Supporting modules inherit the common rules and add technology-specific requirements without repeating or silently weakening them.

For dependency audits or installation requests, start with the [Installation module](references/installation.md). This setup workflow can run before missing dependencies are installed; it does not require loading every technology module.

## Language and communication

- Write documentation, code identifiers, comments, commit messages, and application UI text in English. Preserve established public API names and explicitly required localization.
- Reply in the user's language unless requested otherwise.
- Ask questions in plain-text chat, without question forms or user-input tools.
- Report the result, relevant checks, and remaining limitations. State explicitly when checks were not run.

## Skills

- On first use in a chat, read and apply `ponytail:ponytail` and `caveman` at `full` level unless the user has already changed their level or disabled them. Keep them active until the user changes their level or disables them. Caveman applies to chat responses; code and documentation use normal English.
- Resolve skills by name through the installed skill catalog and read their instructions before applying them. Keep skill references portable: use names, not absolute filesystem paths.
- Load task-specific skills when their task conditions match, rather than loading every skill listed in an index. Apply engine-, framework-, and provider-specific instructions only to the relevant technology.
- If a required skill is missing, use the [Installation module](references/installation.md) to identify its source and installation route. Report the gap; if the task depends on the missing capability, stop the dependent work and explain what is needed. A missing skill does not itself authorize installing it.
- Prefer a connected service's relevant tools when they support the task. If they cannot perform the required operation, explain the limitation and use a suitable fallback.

| Task                                                           | Skills to read and apply               |
| -------------------------------------------------------------- | -------------------------------------- |
| Writing agent instructions or reusable rules                   | `mattpocock-skills:writing-for-agents` |
| Reviewing changes against a commit, branch, tag, or merge base | `mattpocock-skills:code-review`        |
| Diagnosing a difficult bug or performance regression           | `mattpocock-skills:diagnosing-bugs`    |
| Writing a pull request description                             | `mattpocock-skills:pr`                 |

## Task modules

- Before working on HTML, CSS, browser-side JavaScript or TypeScript, browser/WebView UI, or web-project localization, theming, testing, formatting, or lint configuration, read and apply [Web rules](references/web.md).
- Before working on React code or React-specific integrations and tooling, read and apply [React rules](references/react.md).
- Before working on Rust code or tooling, read and apply [Rust rules](references/rust.md).
- Before working on C# code or tooling, read and apply [C# rules](references/csharp.md).
- Before designing, reviewing, or changing database schemas, queries, indexes, permissions, or migrations, read and apply [Database rules](references/database.md).
- Read only modules whose conditions match the current task; a change spanning technologies may require several. Module links back to this file identify inherited rules rather than starting another loading pass.
- Transfer the entire skill directory together. Resolve supporting links relative to the file containing them.

## Scope and implementation

- Read the relevant code and project instructions before editing. Identify affected callers, contracts, tests, fixtures, configuration, and exports.
- Preserve configured toolchain and language versions, supported targets, test frameworks, and project settings unless the task requires changing them.
- Check compatibility, data retention, and security consequences before changing behavior.
- Make the smallest change that fully solves the task. Complete all affected integration points.
- Reuse existing project code and conventions first, platform or standard-library features second, and installed dependencies third. Add a dependency only when the requirement justifies it.
- Introduce abstractions, options, and shared modules when concrete requirements or real consumers justify them.
- For a bug fix, inspect every caller of the affected function and fix the root cause in the shared implementation.
- Preserve validation and error handling when moving or consolidating code. Propagate recoverable failures with context to the responsible caller.
- Comments explain non-obvious reasons and constraints. Record an intentional shortcut as `shortcut: <limit>, <when to upgrade>`.

## Structure and file size

- Group modules by application capability. Keep related UI, state, logic, and tests together; extract shared code when it has real consumers.
- Follow the project's architecture. Add layers and directories when they have a concrete responsibility and content.
- Use consistent domain terminology. Distinguish identity, external address, processing state, and user decisions.
- Whenever adding or modifying a manually maintained source or documentation file, review its structure if the resulting file exceeds 300 lines.
- Treat 300 lines as a review threshold, not a hard limit. Assess cohesion, distinct responsibilities, navigation, and readability.
- Keep a larger file intact when one cohesive responsibility justifies its size. Otherwise split along clear responsibilities and update imports, exports, callers, and tests.
- Exclude lockfiles, dependencies, generated files, and build artifacts from this threshold. Fix generators rather than hand-editing their output.

## Function and method layout

- Organize function and method bodies into logical blocks. Each block performs one coherent step and should be understandable as a unit.
- Separate adjacent steps with exactly one blank line when their purpose changes, for example from validation to preparation, calculation, persistence, or result construction. Use only the steps the function actually needs.
- Keep tightly coupled statements together: a value and its immediate use, an operation and its error check, or a calculation and its direct return. Keep declarations near their use.
- Group related guard clauses at the start of the relevant scope and separate them from the main path with a blank line.
- Apply the same grouping inside longer branches, loops, and callbacks. Keep a short body performing one step compact; avoid blank lines between every statement or immediately inside braces.
- Choose boundaries by meaning rather than a fixed number of lines. Split a visually dense body where the reader moves to a different step; whitespace alone does not fix a function with unrelated responsibilities.
- Preserve meaningful blank lines when editing or refactoring. Concision means removing unnecessary code, not collapsing distinct steps into one uninterrupted block.
- Prefer clear names and spacing over comments labeling obvious stages. Extract a helper when a step merits its own responsibility or reuse, rather than solely to shorten the visible body.

Example in language-neutral pseudocode: validation, preparation, and saving form three blocks; saving and returning its result stay together.

```text
function saveEntry(input):
    if input is invalid:
        return validationError

    entry = normalize(input)
    entry.updatedAt = currentTime()

    savedEntry = repository.save(entry)
    return savedEntry
```

## Review

- Before handing off an implementation, review the complete final diff, including newly added files. Check for unrelated edits and accidental deletions.
- Review correctness, boundary cases, error handling, compatibility, data loss, security, accessibility, and concurrency where relevant.
- Review changed interfaces together with their callers. Check logical block boundaries in function bodies and ensure tests and documentation describe the resulting behavior.
- For a requested code review, establish the comparison base and inspect affected surrounding code. Report actionable findings with severity, file location, concrete trigger, and user-visible consequence.
- Distinguish confirmed defects from questions and assumptions. If no actionable findings remain, say so and identify material verification gaps.
- Avoid mixing unrelated refactoring into a fix. Raise separate improvements separately.

## UI

- Reuse the project's components, design tokens, and styling conventions before creating custom equivalents.
- Provide visible loading, empty, success, and failure states where the interaction requires them. Keep primary actions accessible by keyboard and assistive technology.
- Prevent duplicate submissions while saving. Preserve user input and selection when an operation fails.
- Keep long-running work independent of navigation and UI rendering. Switching views must preserve ongoing work when required.

## Data, security, and background work

- Validate external input at trust boundaries. Stored external content remains untrusted; sanitize it before rendering where necessary.
- Store credentials in environment variables or secret storage. Keep them out of source control, logs, client bundles, distributed binaries, and generated artifacts. Grant runtime identities only the permissions they need.
- Preserve external identifiers in types that cannot lose their value or precision.
- Preserve existing data; destructive changes and rollback need an explicit retention or recovery plan.
- Define temporary and durable data separately. Cleanup must affect only its intended ownership and retention scope.
- Apply related state changes atomically. File replacement must preserve the previous valid version if writing fails.
- Make repeatable operations safe against duplicate execution. Concurrent and late results must respect current ownership and terminal states.
- Bound concurrency, retry attempts, response sizes, and external-operation duration.
- Keep blocking work off UI threads and async executors. Propagate cancellation where supported and ensure background failures are observed within the owning operation's lifecycle.
- Persist progress when restart recovery is required. Report failures without presenting partial work as completed.
- Treat incomplete or contradictory external data as uncertainty. Do not infer a universal contract from one observed response.

## Verification

- Test behavior through public interfaces: provide inputs or perform actions, then assert observable outputs, state changes, side effects, and error handling.
- New non-trivial logic needs a focused behavioral test or executable self-check. For a bug fix, cover the regression when a meaningful test is feasible.
- File-existence and source-text assertions cannot substitute for behavior tests. File or text assertions are appropriate for outputs produced by executing behavior.
- Run checks appropriate to the change using the project's existing commands, including the checks required by matching modules: formatting, linting, type checking, compilation, and relevant tests as applicable. If a runtime cannot use a named command, document a supported equivalent or the verification gap.
- Keep integration tests isolated from user data. Cleanup must target only resources owned by the test.
- Broaden verification when failures or unresolved risks justify it.

## Formatting and analysis policy

- Keep formatter and analyzer configuration in version control so editors, command-line checks, and CI use the same settings. Task modules specify the tools and technology-specific settings.
- Separate verification from applying fixes. Check commands must report violations without rewriting maintained source files. Review automatic fixes before keeping them; change generators rather than their output.
- Make configured lint and analysis warnings fail validation. Keep suppressions and formatting skips narrowly scoped, with a reason for non-obvious exceptions; fix defects rather than weakening checks to make them pass.
- Include required formatting and analysis checks in an existing CI workflow. Editor integration supplements command-line verification rather than replacing it.

## Git

- Create or amend commits only when the user authorizes it. Review the staged changes before committing.
- Write English commit titles as `<type>(<scope>): <summary>`; omit scope for repository-wide changes.
- Use `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `chore`, `perf`, `style`, or `revert`. Keep titles within 72 characters, with an imperative summary and no trailing period.
- Separate title and body with a blank line. Include `Changes`, `Why`, and `Validation`; explicitly state when checks were not run.
- Preserve the configured primary author and existing co-authors. Add the contributing agent's co-author trailer once at the end, separated from the body by a blank line.
- For Codex contributions, use exactly `Co-authored-by: Codex <noreply@openai.com>`.
