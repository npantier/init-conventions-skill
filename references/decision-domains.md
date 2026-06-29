# Decision domains

Walk these one at a time (SKILL.md Step 2). For each: present options + the
recommended default (flagged, never auto-applied) + a "decide later" path. Skip
any already configured or clearly N/A.

## 1. Code style

- **arrow functions vs declarations** — recommend **arrow functions** for module
  functions/components. Enforced via eslint `func-style: ["error", "expression"]`
  (arrow functions allowed under `"expression"`). Framework-required exports (e.g.
  WXT `defineBackground`) stay as they are.
- **named vs default exports** — recommend **named exports**. Enforce via eslint
  `import-x/no-default-export` (or `no-restricted-syntax`), with an override for
  files the framework requires default exports (entrypoints, route files,
  `*.stories.tsx` if the tooling needs it).
- **naming** — descriptive names (`isUserAuthenticated`, not `auth`).
- **early returns** — prefer early returns over deep nesting.
- **`any` policy** — avoid `any`; narrow with a typed cast + a "why" comment.
- **Conflict:** repo using `export function` / `export default` → offer migrate /
  going-forward / keep.

## 2. Format (prettier) — config-bearing

- Recommend **prettier on**: config + a `format` and `format:check` script.
- **BYO config** offered. Default config is in `references/scaffolding.md`.

## 3. Lint (eslint) — config-bearing

- Recommend **eslint on** (flat config), wired to enforce the Domain 1 choices.
- Optional lint-staged scope (or leave linting to lefthook + scripts).
- **BYO config** offered.

## 4. Git hooks

- Recommend **lefthook** (over husky). Default hooks: pre-commit = format check +
  lint on staged files; pre-push = typecheck + test. Dev chooses what runs where.

## 5. Testing

- **framework** — match the repo (vitest/jest); recommend keeping what's present.
- **layout** — recommend **`__tests__/` folders**. Conflict path for repos using
  colocated `*.test.ts`: migrate / going-forward / keep.
- **coverage stance** — optional threshold; default off.
- **TDD** — note the team stance (recommend TDD via `superpowers:test-driven-
  development`); not enforced by config.

## 6. Component workbench (storybook)

- Recommend **storybook on** for component-bearing repos. Offer to skip for
  non-component libraries. Storybook's init is heavy/interactive — see scaffolding
  notes before running.

## 7. TypeScript strictness — config-bearing

- Recommend **`strict: true`** as the baseline. Present the stricter add-ons as
  separate opt-ins, each with a recommendation: **`noUncheckedIndexedAccess`**
  (recommend on — surfaces undefined from index access; can be noisy in array-heavy
  code) and **`noImplicitOverride`** (recommend on). Never auto-apply — surface the
  repo's current `compilerOptions` and let the dev decide each.
- **BYO / keep existing** offered: if the repo already sets these, note them as
  already set rather than rewriting.

## 8. Docs & ADR practice (opt-in)

- Single decision: **adopt an ADR practice?** Recommend **yes**.
- If yes: scaffold `docs/adr/` + the ADR template, optionally seed `0001`, and add
  the CLAUDE.md directive (major decisions → ADR). **No ADRs are minted for the
  setup choices themselves.**

## 9. AI-agent config

- **CLAUDE.md shell** — recommend creating/augmenting from the template.
- **gitignore AI artifacts** — recommend ignoring `.sdd`, `superpowers`, and
  spec/plan dirs if not already ignored.
