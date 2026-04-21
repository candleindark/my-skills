# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repo is a collection of personal [Agent Skills](https://agentskills.io/) for Claude Code. Each top-level directory (other than metadata/config) is a single skill. There is no build, test, or lint step — skills are plain Markdown files consumed directly by Claude Code.

## Skill structure

Every skill lives in its own directory containing a `SKILL.md` with YAML frontmatter:

- `name` — must match the directory name
- `description` — used by Claude Code to decide when to trigger the skill; should describe both what the skill does and when to use it
- `allowed-tools` — space-separated list of tools the skill is permitted to use
- `compatibility` (optional) — external CLIs or dependencies required

The body of `SKILL.md` is the prompt Claude executes. User arguments from slash-command invocation are substituted as `$ARGUMENTS`.

## Installation model

Skills are made available to Claude Code by symlinking each skill directory into `~/.claude/skills/`:

```bash
ln -s /absolute/path/to/my-skills/<skill-name> ~/.claude/skills/<skill-name>
```

This means the directory name is the skill name users will invoke (`/issue-analyze`, `/issue-resume`, etc.). Keep directory names, the `name:` frontmatter field, and any cross-references in `README.md` consistent when renaming.

## Conventions used across skills

- **Issue directory layout**: skills that work with forge issues write to `.issues/<hub>/<repo>/<number>/` in the *consumer* project's root (not in this repo). GitHub uses `<org>` as the hub; non-GitHub forges use the full hostname to avoid collisions. Any new issue-related skill should follow this same layout so directories are shared across skills.
- **Forge-agnostic design**: skills branch on forge (`gh` for GitHub, `glab` for GitLab, direct URL/API fetch otherwise) rather than hardcoding GitHub.
- **Living analysis files**: `issue-analyze` creates `issue-<number>-analysis.md` and `issue-resume` updates it; both skills explicitly instruct the model to keep updating the file throughout the conversation rather than treating it as a one-shot artifact.

## When adding a new skill

1. Create `<skill-name>/SKILL.md` with the frontmatter fields above.
2. Add an entry to `README.md` under `## Skills` following the existing format (description, `**Usage:**`, `**Example:**`).
3. If the skill shares state with existing skills (e.g., reads/writes the `.issues/` tree), match the existing directory convention exactly.

## Keeping documentation in sync

Whenever anything about this repo changes — new information learned about a skill's behavior, a modification committed to `main`, a PR submitted, a new skill added, a convention updated — review and update the relevant documentation in the same change. That includes, at minimum:

- `README.md` (human-facing: skill list, usage examples, installation, directory conventions)
- `CLAUDE.md` (this file: architecture, conventions, installation model)
- The affected skill's `SKILL.md` frontmatter (`description`, `allowed-tools`, `compatibility`) and body

Documentation drift is a bug. If a commit or PR changes behavior without updating the docs that describe it, the change is incomplete.
