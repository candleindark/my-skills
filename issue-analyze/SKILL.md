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

## Steps

### 1. Parse the Issue URL

From `$ARGUMENTS`, identify:
- The forge and hub handle. For GitHub (`github.com`), the hub handle is the org or user (e.g., `dandi` from `github.com/dandi/dandi-cli`). For other forges, use the full hostname as a prefix to avoid collisions (e.g., `hub.psychoinformatics.de/org`).
- Repository name (e.g., `dandi-cli`)
- Issue number (e.g., `1606`)

Directory path convention:
- GitHub: `.issues/<org>/<repo>/<number>/` (e.g., `.issues/dandi/dandi-cli/1606/`)
- Other forges: `.issues/<hostname>/<org>/<repo>/<number>/` (e.g., `.issues/hub.psychoinformatics.de/org/repo/42/`)

### 2. Determine Whether This Is a Fresh Analysis or a Resume

Check whether the issue directory already exists and contains an `issue-<number>-analysis.md`.

- **If it does not exist**: proceed with the **Fresh Analysis** path (steps 3a–6a).
- **If it exists**: proceed with the **Resume** path (steps 3b–7b).

---

## Fresh Analysis Path

### 3a. Create the Issue Directory

Create the appropriate directory relative to the current working directory (project root).

### 4a. Fetch the Issue

Fetch the full issue including title, body, labels, assignees, and all comments using the appropriate method:
- **GitHub**: `gh issue view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab issue view <number> --repo <org>/<repo>`
- **Other forges**: Fetch the issue URL directly or use the forge's API

### 5a. Investigate the Codebase

Before writing the analysis, actively investigate:
- Find the relevant files and functions related to the reported behavior
- Read and trace the relevant code paths
- Understand the logical flow that produces the reported problem
- Look for related tests

### 6a. Create the Analysis File

Create `issue-<number>-analysis.md` in the issue directory with these sections:

- **Issue Summary**: Title, URL, author, labels, assignees, current state, date of analysis
- **Problem Description**: What the issue reports, including error output and reproduction steps from the issue body and comments
- **Additional Context**: Explanation of why the problem occurs and what the expected behavior should be
- **Code Investigation**: Relevant files and functions with paths and line numbers; explanation of current behavior and why it causes the problem
- **Proposed Fix Approach**: High-level description of how to address the issue
- **Open Questions**: Anything unclear that needs further investigation or discussion

---

## Resume Path

### 3b. Read Existing Analysis

Read all files present in the issue directory, particularly `issue-<number>-analysis.md`. Note all other resources in the directory — any file placed there, whether by the AI agent in a prior session or by the human user, may be relevant.

### 4b. Fetch Latest Issue State

Fetch the current issue state including new comments, label/assignee/status changes, and any linked PRs or cross-references:
- **GitHub**: `gh issue view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab issue view <number> --repo <org>/<repo>`
- **Other forges**: Fetch the issue URL directly or use the forge's API

### 5b. Check Git State

- Run `git status` to see current working changes
- Run `git log --oneline -10` to see recent commits
- Note whether there is an active branch related to this issue

### 6b. Update the Analysis

Update `issue-<number>-analysis.md` to reflect:
- Date of this resume session
- New information from the issue (new comments, status changes, linked PRs)
- Current git state (branch, relevant recent commits, WIP changes)
- Revised understanding of the problem or fix approach, if applicable
- Updated next steps

### 7b. Brief the User

Provide a concise summary covering:
- What the issue is about
- What has been done so far
- Current state (branch, uncommitted changes)
- What the logical next steps are

---

## Ongoing Updates

Throughout the rest of this conversation, whenever new understanding is gained — through code investigation, test results, user discussion, or implementation decisions — update the analysis file to reflect the current state. The file should always represent the most complete and accurate picture of the issue.

Any files relevant to the issue can be placed in the issue directory alongside the analysis — including but not limited to test scripts, example data, reproduction cases, diagrams, reference material, and any other supporting material. Both the AI agent and the human user may add files to this directory at any time during the investigation.
