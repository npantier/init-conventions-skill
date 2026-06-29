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
pnpm add -D eslint @eslint/js typescript-eslint eslint-plugin-import
```

`eslint.config.js` (enforces Domain 1: arrow functions + named exports):

```js
import js from '@eslint/js';
import tseslint from 'typescript-eslint';
import importPlugin from 'eslint-plugin-import';

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.recommended,
  {
    plugins: { import: importPlugin },
    rules: {
      'func-style': ['error', 'expression', { allowArrowFunctions: true }],
      'import/no-default-export': 'error',
    },
  },
  {
    // framework files that REQUIRE a default export
    files: ['**/entrypoints/**', '**/*.stories.tsx', '**/*.config.*'],
    rules: { 'import/no-default-export': 'off' },
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

After init, verify `.storybook/` exists and `pnpm storybook` boots. Add a
`*.stories.tsx` exemption to the eslint default-export rule (already in the
eslint config above).

## test layout (`__tests__/`)

- **New repo / going-forward:** set the test runner's include glob to
  `**/__tests__/**/*.{test,spec}.{ts,tsx}` and place new tests under `__tests__/`.
  Example vitest config: `test: { include: ['**/__tests__/**/*.test.{ts,tsx}'] }`.
- **Migrate retroactively (only if chosen):** for each `src/.../foo.test.ts`,
  `git mv` it to `src/.../__tests__/foo.test.ts`, fix the relative import depth
  (`./foo` → `../foo`), then run the suite to confirm green.

## tsconfig strict flags

Ensure `compilerOptions` includes: `"strict": true`,
`"noUncheckedIndexedAccess": true`, `"noImplicitOverride": true`. Add a
`"compile": "tsc --noEmit"` script if absent.
