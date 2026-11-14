# Customizations: setup and update recipe

Recipe for a local clone of the repository (Claude Code with `--plugin-dir` or a git checkout). Installs from the plugin marketplace need none of this: put your rules in project instructions or paste them into the conversation. This plugin ships no scripts; you follow these steps with your own tools, and only when the person asks. Paths are relative to the plugin folder (the one that holds `skills/`).

Files:
- Template (tracked): `skills/humanizeai/assets/user-customizations.template.md`
- Personal copy (git-ignored): `skills/humanizeai/assets/user-customizations.md`

## Setup (once after cloning)

1. Check whether `skills/humanizeai/assets/user-customizations.md` exists.
2. If it exists, leave it alone and say: "user-customizations.md already exists; leaving it alone."
3. If it is absent, copy the template to that path (never overwrite) and say: "Created skills/humanizeai/assets/user-customizations.md. Edit that file, not the template."

## Update (pull the latest version of a clone)

Safe any time: the personal copy is git-ignored, so a pull never touches it. Run from the repository root of the clone.

1. Check whether `skills/humanizeai/assets/user-customizations.md` exists. If it does, say: "Found your customizations file. It's git-ignored, so this pull won't touch it." If not, say: "No customizations file yet. Run the setup steps afterwards if you want one."
2. Run `git pull --ff-only`. If it cannot fast-forward, stop and report; do not merge or reset.
3. Say "Done. Current version:" and report the first `"version"` value in `.claude-plugin/plugin.json` (in this repository it sits at `adapters/claude/.claude-plugin/plugin.json`).

Never write to plugin files other than the personal copy, and never during a humanize run.
