# humanizeai agent guide

Humanize AI Claude plugin: detect AI-writing tells, rewrite, and audit while keeping facts, names, numbers and quotes intact.

**Structure:** the shipped plugin is `adapters/claude/` (`skills/` with `humanizeai`, `humanizeai-detect`, `humanizeai-rewrite`, `humanizeai-audit`; `knowledge/` patterns by source; `evals/`; `prompts/`). `dev/` holds maintainer-only scripts (not shipped). Root: marketplace, docs, CI.

## Commands
- First clone: follow `adapters/claude/skills/humanizeai/references/customizations-setup.md` (personal customizations file; update steps); `dev/scripts/setup.sh` and `update.sh` are the maintainer originals.
- Validate: `claude plugin validate adapters/claude --strict` and `claude plugin validate . --strict`
- Directory gate: `python <aiaddon>/dev/aiaddon-review/scripts/validate_extension.py adapters/claude --strict --target directory --require-meta author,company`; router: `route_check.py adapters/claude`; secrets: `scan_secrets.py . --history` (false positives only in `.public-safety-allow`)
- Evals (real model calls): `claude plugin eval adapters/claude --trust-plugin --ablation none --scaffold --allow-tools Read Glob Grep Skill -j 4 --no-publish`. Rebuild cases with `python <owner-tools>/plugin-evals/regen_evals.py <plugin> <router> <fixture ids>` (owner's local tools folder, not in this repo). Write results outside the repo (`<owner-outputs>/<plugin>/eval-results`, a local folder outside the repo); the packager includes repo files.

## Workflow
1. Read the skill or script you change, plus its neighbours.
2. Make the smallest change; run the closest check, then the full gate.
3. Update `CHANGELOG.md` for behavior changes; log ideas in `IMPROVEMENTS.md`.

## Rules
- **Directory compliance:** the shipped plugin has no hooks, no scripts or other non-text files, and text-only frontmatter. Script-like features are recipes in `skills/*/references/`; `dev/` is maintainer tooling only.
- **Never publish secrets or private data** (keys, tokens, personal contact data, client names, internal URLs, local paths); the secret and public-safety scans block every commit and release.
- Run the closest check and the full gate before committing. Never delete or weaken tests or evals. Report failed, skipped or timed-out runs honestly; NOT RUN is never a pass.
- Follow neighbouring files. Keep each `SKILL.md` under 500 lines; move detail into `references/`.
- On release, bump `version` in `adapters/claude/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` together (skills carry no `metadata` block: directory policy wants text-only frontmatter); add a `CHANGELOG.md` entry per behavior change.
- No commits, pushes or PRs unless asked. Work on a branch and open a PR; never push to main.
- No new dependencies without asking (scripts stay on the Python standard library).
- Never commit secrets. Never pick names or slugs without user confirmation.
- Keep em dashes and en dashes out of skills, docs and examples, except where a rule names the characters.
- A fact that changes during a rewrite is a high-priority bug.
- Edit the knowledge file that owns a rule; never restate it in a second file. Update its "Last verified" date when you check a source. Paraphrase sources; cite new ones in `NOTICE.md`.
- Skills that run scripts copy them once per session into the working folder (e.g. `./tools/`) and run from there; never put a `/root/.claude` or `~/.claude` path in a shell command.
- Version bumps: patch for a fix, minor for a new pattern, file or source.

## Model routing
Opus plans, decides, writes self-contained briefs with a report word limit, and reviews every result. Haiku for search, reading and mechanical edits. Sonnet for coding, tests and multi-step work. Do 1-2 tool calls inline.

## Docs
README.md, CONTRIBUTING.md, CHANGELOG.md, NOTICE.md; ideas and feedback in `IMPROVEMENTS.md`.
