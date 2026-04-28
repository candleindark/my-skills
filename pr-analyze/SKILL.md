---
name: pr-analyze
description: Analyzes a pull/merge request from any Git forge (GitHub, GitLab, etc.) by creating .prs/<hub>/<repo>/<number>/ in the project root, running the built-in /review skill on it, and writing a detailed analysis markdown file. If the PR has been analyzed previously, resumes work by refreshing PR state, checking git status, and updating the existing analysis. Use when starting — or returning to — work on reviewing a pull request.
compatibility: For GitHub PRs, requires gh CLI. For GitLab MRs, requires glab CLI. Other forges may require direct API access or web fetching. Invokes the built-in /review skill.
allowed-tools: Bash Read Grep Glob Write Edit Skill
---

# PR Analyze

You are helping the user investigate and deeply understand a pull request (or merge request) from a Git forge. If the PR has been analyzed in a prior session, you are instead helping them pick up where they left off.

## Invocation

The user provides a PR URL as the argument: `$ARGUMENTS`

## 1. Parse the PR URL

From `$ARGUMENTS`, identify:

- The forge and hub handle. For GitHub (`github.com`), the hub handle is the org or user (e.g., `dandi` from `github.com/dandi/dandi-cli`). For other forges, use the full hostname as a prefix to avoid collisions (e.g., `hub.psychoinformatics.de/org`).
- Repository name (e.g., `dandi-cli`)
- PR/MR number (e.g., `1742`)

Directory path convention:

- GitHub: `.prs/<org>/<repo>/<number>/` (e.g., `.prs/dandi/dandi-cli/1742/`)
- Other forges: `.prs/<hostname>/<org>/<repo>/<number>/` (e.g., `.prs/hub.psychoinformatics.de/org/repo/42/`)

## 2. Choose the Path

Check whether the PR directory already exists and contains a `pr-<number>-analysis.md`.

- **If it does not exist** → this is a fresh analysis. Read [references/fresh-analysis.md](references/fresh-analysis.md) and follow its steps.
- **If it exists** → this is a resume. Read [references/resume.md](references/resume.md) and follow its steps.

Only load the reference file for the path you are actually taking.

## 3. Ongoing Updates

Throughout the rest of this conversation, whenever new understanding is gained — through code investigation, running the review, test results, or **dialog with the user** (facts they share, clarifications they request, constraints, decisions, references) — update the appropriate file to reflect the current state. Two files should be kept up-to-date:

- **`pr-<number>-analysis.md`** — review findings, recommendations, decisions, and anything else specific to *this PR*. Should always represent the most complete and accurate picture of the PR.
- **`pr-<number>-background.md`** — domain, protocol, and reference knowledge surfaced during the conversation that helps future readers (or future sessions) understand the PR but is not specific to its changes. Examples: how a relevant external system or storage format works, glossary of terms used in the PR thread, protocol-level details that explain why a constraint exists. Create this file lazily on the first occasion such information arises (typically through user Q&A, but also from code exploration or documentation lookups). If no such information ever arises, the file does not need to exist.

Keep the split clean: PR-specific findings go in the analysis file; reusable context goes in the background file. When background information is directly load-bearing for a finding, the analysis file should reference the background file rather than duplicating the explanation. Whenever the analysis file is created or updated, ensure it lists the companion background file (if it exists) in a short "Companion files" section near the top.

Any other files relevant to the PR can also be placed in the PR directory — including but not limited to test scripts, example data, reproduction cases, diagrams, and any other supporting material. Both the AI agent and the human user may add files to this directory at any time during the investigation.
