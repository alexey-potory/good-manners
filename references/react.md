# React Rules

Apply this module to React code and React-specific integrations and tooling. The [shared development rules](../SKILL.md) remain in effect.

For browser or WebView interfaces, also read and apply the [Web rules](web.md).

## Skills

- Apply `vercel-react-best-practices` for components, hooks, state, data fetching, and performance work.

## Components and styling

- In projects using shadcn/ui, search available components and inspect suitable examples through Shadcn MCP before creating a component. Adapt or compose existing project and shadcn/ui components; write a custom component when these cannot meet the requirement.

## Internationalization

- When the project uses `i18next`, use `react-i18next` as its React binding.

Reference: [react-i18next](https://react.i18next.com/).

## Linting

- Add React Hooks rules to the project's ESLint configuration. Enable Fast Refresh checks when supported by the build tooling.

## Testing

- For Node-based DOM component tests, use `@testing-library/react` with its required peer dependencies, including `@testing-library/dom` where required by the installed version.
- Render components with the routing, theme, localization, or state providers the tested behavior requires. Reuse an existing render helper when available, and ensure React Testing Library cleanup runs between tests even when Vitest globals are disabled.

Reference: [React Testing Library setup](https://testing-library.com/docs/react-testing-library/setup/).
