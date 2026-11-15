# C# Rules

Apply this module to C# code and tooling. The [shared development rules](../SKILL.md) remain in effect.

## Skills

| Task                                  | Skills to read and apply |
| ------------------------------------- | ------------------------ |
| Reviewing C# code                     | `csharp-code-review`     |
| Writing or changing async code        | `csharp-async`           |
| Writing or updating XML documentation | `csharp-docs`            |
| Writing or reviewing MSTest tests     | `csharp-mstest`          |
| Writing or reviewing NUnit tests      | `csharp-nunit`           |
| Writing or reviewing TUnit tests      | `csharp-tunit`           |
| Writing or reviewing xUnit tests      | `csharp-xunit`           |

## Implementation

- Respect nullable-reference settings and resource-lifetime conventions for the project's .NET or runtime-specific APIs.

## Formatting and static analysis

- Ensure the configured SDK provides `dotnet format` and compatible .NET analyzers.
- Keep formatting, naming, code-style rules, and diagnostic severities in `.editorconfig`.
- Enable SDK-provided .NET analyzers during build. In supported SDK-style projects, set `EnableNETAnalyzers` to `true` when needed. Prefer SDK-provided analyzers; add analyzer packages only for required capabilities they do not supply.
- Set `EnforceCodeStyleInBuild` to `true` in supported projects and configure required code-style diagnostics as warnings or errors. Choose an explicit analysis level compatible with the configured SDK and project policy.
- Centralize shared analyzer settings in an existing `Directory.Build.props` when appropriate; keep project-specific exceptions in their project configuration.
- A clean formatting check does not replace build-time analysis: some analyzer diagnostics have no automatic fix.

| Purpose                                                                    | Baseline command                                          |
| -------------------------------------------------------------------------- | --------------------------------------------------------- |
| Check formatting and available style/analyzer fixes without changing files | `dotnet format <solution-or-project> --verify-no-changes` |
| Apply formatting and available style/analyzer fixes                        | `dotnet format <solution-or-project>`                     |
| Build with analyzers and treat warnings as errors                          | `dotnet build <solution-or-project> -warnaserror`         |

- Replace command placeholders with the actual solution or project and use the required build configuration and target frameworks.

References: [dotnet format](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-format), [.NET analysis settings](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#code-analysis-properties), [MSBuild command-line options](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-command-line-reference).
