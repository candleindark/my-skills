---
name: issue-analyze
description: Analyzes an issue from any Git forge (GitHub, GitLab, etc.) by creating .issues/<hub>/<repo>/<number>/ in the project root, fetching the issue, investigating the codebase, and writing a detailed analysis markdown file. If the issue has been analyzed previously, resumes work by refreshing issue state, checking git status, and updating the existing analysis. Use when starting — or returning to — work on an issue tracker issue.
compatibility: For GitHub issues, requires gh CLI. For GitLab, requires glab CLI. Other forges may require direct API access or web fetching.
allowed-tools: Bash Read Grep Glob Write Edit
---

# Issue Analyze

You are helping the user investigate and deeply understand an issue from a Git forge. If the issue has been analyzed in a prior session, you are instead helping them pick up where they left off.

## Invocation

The user provides an issue URL as the argument: `$ARGUMENTS`

## 1. Parse the Issue URL

From `$ARGUMENTS`, identify:

- The forge and hub handle. For GitHub (`github.com`), the hub handle is the org or user (e.g., `dandi` from `github.com/dandi/dandi-cli`). For other forges, use the full hostname as a prefix to avoid collisions (e.g., `hub.psychoinformatics.de/org`).
- Repository name (e.g., `dandi-cli`)
- Issue number (e.g., `1606`)

Directory path convention:

- GitHub: `.issues/<org>/<repo>/<number>/` (e.g., `.issues/dandi/dandi-cli/1606/`)
- Other forges: `.issues/<hostname>/<org>/<repo>/<number>/` (e.g., `.issues/hub.psychoinformatics.de/org/repo/42/`)

## 2. Choose the Path

Check whether the issue directory already exists and contains an `issue-<number>-analysis.md`.

- **If it does not exist** → this is a fresh analysis. Read [references/fresh-analysis.md](references/fresh-analysis.md) and follow its steps.
- **If it exists** → this is a resume. Read [references/resume.md](references/resume.md) and follow its steps.

Only load the reference file for the path you are actually taking.

## 3. Ongoing Updates

Throughout the rest of this conversation, whenever new understanding is gained — through code investigation, test results, user discussion, or implementation decisions — update the analysis file to reflect the current state. The file should always represent the most complete and accurate picture of the issue.

Any files relevant to the issue can be placed in the issue directory alongside the analysis — including but not limited to test scripts, example data, reproduction cases, diagrams, reference material, and any other supporting material. Both the AI agent and the human user may add files to this directory at any time during the investigation.
