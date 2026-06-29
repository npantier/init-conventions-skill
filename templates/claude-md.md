# CLAUDE.md

Guidance for working in this repo. Keep it current as the architecture evolves.

## What this is

<one-paragraph description of the project>

## Conventions

- **Code style:** <arrow functions / named exports / early returns / no `any` —
  fill from the chosen Domain 1 decisions>
- **Formatting:** <prettier — `pnpm format` / `pnpm format:check`, or "deferred">
- **Linting:** <eslint — `pnpm lint`, or "deferred">
- **Git hooks:** <lefthook: pre-commit … / pre-push … , or "deferred">
- **Tests:** <framework + `__tests__/` vs colocated, or "deferred">
- **Component workbench:** <storybook — `pnpm storybook`, or "n/a" / "deferred">

## Decision records

<Include this block ONLY if the ADR practice was adopted:>
When a major or architectural decision is made, capture it as an ADR in
`docs/adr/` using the template there. Mundane config choices live here, not in an
ADR.

## Deferred decisions

<Anything the dev punted during setup — list each so it resurfaces:>
- [ ] <domain>: <what's still undecided>

## Commands

```bash
<the project's real dev / build / test / lint / format commands>
```
