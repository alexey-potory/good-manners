# Web Rules

Apply this module to HTML, CSS, browser-side JavaScript or TypeScript, browser/WebView interfaces, and web-project localization, theming, testing, formatting, and lint configuration, regardless of framework. The [shared development rules](../SKILL.md) remain in effect.

## Modern Web Guidance

- Use the [Modern Web Guidance plugin](plugin://modern-web-guidance@openai-curated-remote) and its `modern-web-guidance` skill for the frontend workflow below.
- At the start of implementing a web feature and before creating a component, search the plugin's guidance with an action-oriented query describing the task. Retrieve and read the matching guides before choosing an implementation. If matches are missing or unclear, browse the guide list as instructed by the skill.
- Apply the retrieved guidance to the project's framework, browser targets, and runtime. Follow the guides' compatibility and fallback recommendations unless the project has an explicit browser-support policy that changes them.
- Before handing off code, check it against the applicable guides and required fallbacks.
- Use this workflow for frontend features, layout, styling, browser APIs, accessibility, and web performance. Backend-only, database-only, CI, Git, and lint-configuration tasks do not trigger it.

## Architecture

- When establishing a new frontend structure, use moderate Feature-Sliced Design: introduce `app`, `features`, and `shared` as needed; add further layers only for concrete requirements.

## Components and styling

- In projects using Tailwind CSS, prefer utilities and existing theme tokens. Add custom CSS only when the requirement cannot be met otherwise; keep required Tailwind and theme setup in the existing global stylesheet.

## Themes

- Support three user-selectable modes: `Light`, `Dark`, and `System`. Default to `System` when no preference has been saved.
- Persist the selected mode using the project's preference storage and restore it on startup. Preserve `System` as a preference rather than replacing it with the currently resolved light or dark theme.
- In `System` mode, follow the operating system's color preference and react to changes while the application is open. An explicit `Light` or `Dark` selection takes precedence over system changes.
- Apply the resolved theme through semantic theme tokens consistently to all views, overlays, and supported native controls; keep text, focus indicators, and interactive states readable in both themes.
- Apply the initial theme before displaying the interface to avoid a flash of the wrong theme. Keep theme resolution centralized rather than duplicating it in individual components.
- Verify all three modes, preference persistence, and live system-theme changes, including that explicit selections remain unchanged when the system preference changes.

## Internationalization

- Use `i18next` for UI localization. Reuse an existing compatible i18n integration rather than introducing a second translation system.
- Keep English as the source and default fallback language. Add other languages when required by the product.
- Store translation resources in dedicated localization files, separate from application code, for example `locales/<language>/<namespace>.json`. Keep initialization and resource-loading code separate from the translation dictionaries; do not embed dictionaries in UI components or other application source files.
- Reference stable translation keys for user-facing labels, messages, placeholders, tooltips, and accessibility text. Keep translated text in localization files, including fallback text, rather than hardcoding it in templates or translation calls.
- Use interpolation and pluralization supported by the i18n library instead of concatenating translated sentence fragments. Group resources into namespaces by feature or shared purpose when needed.
- Keep keys consistent across supported languages and verify that new or changed UI text resolves correctly, including fallback and accessibility text.

Reference: [i18next namespaces](https://www.i18next.com/principles/namespaces).

## Formatting and linting

- Ensure the project has Prettier, ESLint, and `eslint-config-prettier` installed as development dependencies with lockfile entries.
- Use Prettier as the formatting authority.
- Configure ESLint for the project's JavaScript or TypeScript source. Enable TypeScript linting when the project uses TypeScript.
- Use `eslint-config-prettier` to disable lint rules that conflict with Prettier. Keep code-quality checks in ESLint and formatting in Prettier.
- Provide package scripts named `lint` and `format:check` for verification, and `lint:fix` and `format` for applying changes.
- Exclude dependencies, generated code, build output, and lockfiles from formatting; exclude dependencies, generated code, and build output from linting. Include manually maintained source and documentation in formatting coverage and relevant source files in lint coverage.

## Testing

- Use Vitest for new web test setups. Keep an existing test runner unless the task requires migration; add only the development dependencies needed by the tests being implemented.

| Test capability                       | Packages                                                                                        |
| ------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Unit and integration tests            | `vitest`                                                                                        |
| Node-based DOM tests                  | `jsdom`; reuse `happy-dom` instead when already configured and sufficient for the required APIs |
| DOM queries and user interactions     | `@testing-library/dom`, `@testing-library/user-event`                                           |
| DOM assertions in Node-based tests    | `@testing-library/jest-dom`                                                                     |
| HTTP request interception when needed | `msw`                                                                                           |
| Coverage when required                | `@vitest/coverage-v8`, or the project's existing compatible coverage provider                   |
| Real-browser tests when needed        | `@vitest/browser-playwright`, or the project's existing supported browser-testing tooling       |

- Run pure logic tests in the Node environment and DOM tests in the configured DOM environment. For Node-based DOM component tests, use a framework-specific Testing Library adapter; install only the adapter matching the framework under test.
- Configure test discovery, environments, and setup files through the existing Vite or Vitest configuration. Share the application's required aliases and framework transforms. Keep Vitest provider versions compatible with the runner.
- For Node-based DOM assertions, import `@testing-library/jest-dom/vitest` from a setup file registered with `setupFiles`; include the setup file in TypeScript configuration when needed. Browser Mode has its own browser APIs and assertions, so do not add a DOM emulator to it.
- Prefer accessible roles, labels, and names for queries. In Node-based DOM interaction tests, create a `userEvent.setup()` instance for each test and await its actions and asynchronous UI results rather than using arbitrary delays.
- Ensure DOM cleanup runs between tests and restore any changed timers, mocks, browser storage, and request handlers. When using MSW, make unexpected requests fail rather than allowing accidental live-network access.
- Use real-browser tests for behavior that depends on layout, CSS rendering, or browser-only APIs; a passing jsdom test does not verify those features. Add browser tooling only when the scenario needs it. For Vitest Browser Mode, configure a provider and browser instances separately from Node-based test environments.
- Provide `test` as a single-run command using `vitest run` and `test:watch` using `vitest`. When coverage is required, provide `test:coverage` using `vitest run --coverage` and preserve the project's coverage policy. CI must use the single-run command.

References: [Vitest environments](https://vitest.dev/guide/environment.html), [Vitest coverage](https://vitest.dev/guide/coverage.html), [Vitest request mocking](https://vitest.dev/guide/mocking/requests), [Vitest Browser Mode](https://vitest.dev/guide/browser/), [user-event](https://testing-library.com/docs/user-event/intro/), [jest-dom with Vitest](https://github.com/testing-library/jest-dom#with-vitest).
