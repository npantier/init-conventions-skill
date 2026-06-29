---
name: init-conventions
description: Use at the START of work in a new or young repo — BEFORE brainstorming or writing feature code — to establish and scaffold foundational conventions: code style (arrow fns, named exports), formatter, linter, git hooks, test layout, component workbench, an ADR practice, and AI-agent config (CLAUDE.md). Use whenever the user says "new repo", "set up conventions/standards", "init the project", "before we start building", or is about to brainstorm a feature in a repo that has no formatter / linter / test layout / CLAUDE.md yet. Recommends sensible defaults but the developer decides EVERY choice; detects and respects existing setup instead of overwriting it.
---

# Init Conventions

## Why this exists

It's easy to jump straight into brainstorming and code without first stating the
conventions you want — arrow functions, named exports, formatter, linter, hooks,
test layout, component workbench, AI-agent config. Backfilling those later churns
code and decisions. This skill closes that gap: a guided setup run **before**
feature brainstorming so the groundwork is decided and scaffolded up front.

Run it, then hand off to `superpowers:brainstorming` for the actual feature.

## Operating principles

- **Recommend, don't impose.** Each decision shows the author's default *flagged
  as a recommendation*. The developer (or the app's nature) decides. Never
  auto-apply a default.
- **Detect, don't ask.** Inspect the repo first. Never re-ask what's already
  configured — surface it as "already set".
- **Never silently rewrite.** When the repo already uses a different convention,
  surface the conflict and offer explicit paths.
- **Defer is always allowed.** Any decision can be punted; deferred items are
  recorded in CLAUDE.md so they resurface instead of vanishing.
- **Verify green.** After scaffolding, run the toolchain and report real output.

## Step 1 — Detect repo state

Build a snapshot before asking anything. Run (adapt to the repo):

```bash
ls -la                                  # top-level: configs present?
cat package.json 2>/dev/null            # scripts, deps, type:module
ls pnpm-lock.yaml package-lock.json yarn.lock 2>/dev/null   # package manager
cat .gitignore 2>/dev/null              # AI-artifact entries already present?
ls .prettierrc* prettier.config.* eslint.config.* .eslintrc* lefthook.yml .husky 2>/dev/null
ls .storybook 2>/dev/null               # storybook already set up?
ls CLAUDE.md docs/adr 2>/dev/null       # AI config + ADR practice already present?
```

Also sample the existing code style so you can detect conflicts later. Use plain
`grep` (not `git grep`) — a fresh repo often has no commits, and `git grep` only
searches tracked files, so it silently finds nothing in the greenfield case this
skill targets:

```bash
grep -rl "export default" src app 2>/dev/null                 # default exports in use?
grep -rlE "export (async )?function" src app 2>/dev/null      # function declarations?
grep -rl "__tests__" src app 2>/dev/null ; grep -rl "\.test\." src app 2>/dev/null  # test layout
```

Summarize the snapshot to the user: package manager, language/framework, what's
already configured (mark those "already set"), and the current code-style /
test-layout signals.

## Step 2 — Walk the decision domains

Read `references/decision-domains.md`. Walk the in-scope domains **one at a time**.
For each domain:

1. Skip it if detection shows it's already configured (note it as already set), or
   if it's clearly N/A for this project (offer to skip — e.g. storybook for a
   non-component library).
2. Present: the decision, the options, the **author default flagged as
   recommended**, and a **"decide later"** path.
3. If the tool is config-bearing (prettier, eslint, tsconfig), offer **bring-your-
   own-config**: use the recommended default, or paste / point to the dev's own
   config (used verbatim).
4. **Existing-convention conflict:** if the repo already uses a different
   convention than the dev picks, offer exactly three paths — never a silent
   rewrite:
   - **Migrate retroactively** — rewrite existing code to the new convention.
   - **Adopt going-forward** — new code follows it; existing left as-is.
   - **Keep existing** — override the recommendation.
5. Record the outcome (chosen / deferred / skipped / already-set).

## Step 3 — Record decisions + scaffold

Once choices are made, for each **chosen** domain:

- **CLAUDE.md** — create from `templates/claude-md.md`, or augment an existing
  CLAUDE.md. Record the chosen conventions, a **Deferred decisions** section for
  anything punted, and — only if the ADR practice was adopted — the directive:
  *"When a major / architectural decision is made, capture it as an ADR in
  `docs/adr/`."*
- **ADR practice (only if opted in)** — scaffold `docs/adr/` and copy
  `templates/adr.md` in as the template. Offer to seed
  `0001-record-architecture-decisions.md` (the meta "we use ADRs" record). Do
  **not** mint ADRs for the setup choices themselves.
- **.gitignore** — add AI-artifact entries (`.sdd`, `superpowers`, spec/plan dirs)
  if missing.
- **Tooling** — follow `references/scaffolding.md`, keyed to the detected package
  manager: install + configure prettier, eslint, lefthook, storybook, test layout,
  tsconfig strict flags, and the matching `package.json` scripts.

Apply file-mutating migrations (e.g. moving tests to `__tests__/`, rewriting
exports) **only** when the dev chose "migrate retroactively" in Step 2.

## Step 4 — Verify

Run the toolchain just scaffolded and report **real output**, e.g.:

```bash
<pm> run format:check
<pm> run lint
<pm> run compile   # or tsc --noEmit
<pm> test
```

If any gate fails, fix or report it honestly. No "should work" claims.

## Step 5 — Hand off

Conventions are set. Point the user at `superpowers:brainstorming` to brainstorm
the actual feature: "Groundwork's in place — let's brainstorm the feature."

## Reference files

- `references/decision-domains.md` — the domains: question, options, author
  default, config-bearing flag, conflict notes.
- `references/scaffolding.md` — per-tool install + config recipes, keyed by
  package manager.
- `templates/adr.md` — MADR-lite ADR template.
- `templates/claude-md.md` — CLAUDE.md shell template.
