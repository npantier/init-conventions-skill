# Scaffolding recipes

Commands shown for **pnpm** (primary). For npm use `npm i -D` / `npm run`; for
yarn `yarn add -D` / `yarn`. Always wire matching `package.json` scripts.

## prettier

```bash
pnpm add -D prettier
```

`.prettierrc.json` (default — replace verbatim if the dev brought their own):

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

`.prettierignore`: `node_modules`, build output (`.output`, `dist`), lockfiles.

`package.json` scripts:

```json
"format": "prettier --write .",
"format:check": "prettier --check ."
```

## eslint (flat config)

```bash
pnpm add -D eslint @eslint/js typescript-eslint eslint-plugin-import-x
```

`eslint-plugin-import-x` is the maintained, flat-config-native fork of
`eslint-plugin-import` — fewer ESLint 9 / flat-config compatibility issues.

`eslint.config.js` (enforces Domain 1: arrow functions + named exports):

```js
import js from '@eslint/js';
import tseslint from 'typescript-eslint';
import importX from 'eslint-plugin-import-x';

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommended,
  {
    plugins: { 'import-x': importX },
    rules: {
      'func-style': ['error', 'expression'],
      'import-x/no-default-export': 'error',
    },
  },
  {
    // framework files that REQUIRE a default export
    files: ['**/entrypoints/**', '**/*.stories.tsx', '**/.storybook/**', '**/*.config.*'],
    rules: { 'import-x/no-default-export': 'off' },
  },
);
```

`package.json` script: `"lint": "eslint ."`

## lefthook

```bash
pnpm add -D lefthook
pnpm lefthook install
```

`lefthook.yml`:

```yaml
pre-commit:
  parallel: true
  commands:
    format:
      glob: '*.{ts,tsx,js,jsx,json,md}'
      run: pnpm prettier --check {staged_files}
    lint:
      glob: '*.{ts,tsx,js,jsx}'
      run: pnpm eslint {staged_files}
pre-push:
  commands:
    typecheck:
      run: pnpm run compile
    test:
      run: pnpm test
```

Adjust `compile`/`test` to the repo's actual script names.

## storybook

Storybook's initializer is interactive and writes many files. Confirm with the
dev before running, then:

```bash
pnpm dlx storybook@latest init
```

After init, verify `.storybook/` exists and `pnpm storybook` boots. The eslint
config above already exempts `*.stories.tsx` and `.storybook/**` (Storybook's
`main.ts` / `preview.ts` use default exports) from the default-export rule.

## test layout (`__tests__/`)

- **New repo / going-forward:** set the test runner's include glob to
  `**/__tests__/**/*.{test,spec}.{ts,tsx}` and place new tests under `__tests__/`.
  Example vitest config: `test: { include: ['**/__tests__/**/*.test.{ts,tsx}'] }`.
- **Migrate retroactively (only if chosen):** for each `src/.../foo.test.ts`,
  `git mv` it to `src/.../__tests__/foo.test.ts`, fix the relative import depth
  (`./foo` → `../foo`), then run the suite to confirm green.

## tsconfig strict flags

Apply only the flags the dev opted into in Domain 7 — do not force the full set.
Candidates: `"strict": true` (baseline recommendation),
`"noUncheckedIndexedAccess": true`, `"noImplicitOverride": true`. Leave any the dev
declined as-is. Add a `"compile": "tsc --noEmit"` script if absent.
