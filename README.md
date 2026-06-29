# init-conventions

A [Claude Code](https://claude.com/claude-code) skill that establishes and scaffolds foundational repo conventions **before** you start brainstorming or writing feature code.

It's easy to jump straight into building without first deciding the conventions you want — code style, formatter, linter, git hooks, test layout, component workbench, ADR practice, AI-agent config. Backfilling those later churns code and decisions. This skill closes that gap with a guided setup run up front.

## What it sets up

- **Code style** — arrow functions vs. declarations, named vs. default exports
- **Formatter** — Prettier (with bring-your-own-config support)
- **Linter** — ESLint (with bring-your-own-config support)
- **Git hooks** — Lefthook
- **Test layout** — `__tests__/` vs. co-located `.test.` files
- **Component workbench** — Storybook
- **ADR practice** — `docs/adr/` with a MADR-lite template
- **AI-agent config** — a `CLAUDE.md` recording the chosen conventions and any deferred decisions
- **`.gitignore`** — AI-artifact entries

## Operating principles

- **Recommend, don't impose.** Each decision shows a default flagged as a recommendation. The developer (or the app's nature) decides — defaults are never auto-applied.
- **Detect, don't ask.** It inspects the repo first and never re-asks what's already configured.
- **Never silently rewrite.** When the repo already uses a different convention, it surfaces the conflict and offers explicit paths (migrate retroactively, adopt going-forward, or keep existing).
- **Defer is always allowed.** Any decision can be punted; deferred items are recorded in `CLAUDE.md` so they resurface instead of vanishing.
- **Verify green.** After scaffolding, it runs the toolchain and reports real output.

## How it works

1. **Detect repo state** — package manager, language/framework, existing configs, and current code-style/test-layout signals.
2. **Walk the decision domains** — one at a time, skipping anything already set or clearly N/A.
3. **Record decisions + scaffold** — write/augment `CLAUDE.md`, optionally scaffold ADRs, update `.gitignore`, and install + configure tooling.
4. **Verify** — run format/lint/compile/test and report real output.
5. **Hand off** — point you at feature brainstorming with the groundwork in place.

## Installation

Clone into your Claude Code skills directory:

```bash
git clone https://github.com/npantier/init-conventions-skill.git ~/.claude/skills/init-conventions
```

Then invoke it at the start of work in a new or young repo — e.g. "set up conventions", "init the project", or "before we start building".

## Layout

```
SKILL.md                          # skill instructions
references/decision-domains.md    # the domains: question, options, default, conflict notes
references/scaffolding.md         # per-tool install + config recipes, keyed by package manager
templates/adr.md                  # MADR-lite ADR template
templates/claude-md.md            # CLAUDE.md shell template
evals/trigger-eval.json           # trigger evaluation cases
```
