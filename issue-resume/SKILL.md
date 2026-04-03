---
name: issue-resume
description: Resumes work on a Git forge issue (GitHub, GitLab, etc.) by reading the existing analysis in .issues/<hub>/<repo>/<number>/, fetching the latest issue state, checking git status, and updating the analysis with new information. Use when returning to work on a previously analyzed issue.
compatibility: For GitHub issues, requires gh CLI. For GitLab, requires glab CLI. Other forges may require direct API access or web fetching.
allowed-tools: Bash Read Grep Glob Write Edit
---

# Issue Resume

You are helping the user pick up where they left off on an issue from a Git forge.

## Invocation

The user provides an issue URL as the argument: `$ARGUMENTS`

## Steps

### 1. Parse the Issue URL

From `$ARGUMENTS`, identify the forge, hub handle, repository name, and issue number. Determine the issue directory path using the same convention as `issue-analyze`:
- GitHub: `.issues/<org>/<repo>/<number>/`
- Other forges: `.issues/<hostname>/<org>/<repo>/<number>/`

### 2. Read Existing Analysis

Check whether the issue directory exists and read all files present, particularly `issue-<number>-analysis.md`. Note all other resources in the directory — any file placed there, whether by the AI agent in a prior session or by the human user, may be relevant.

If no analysis directory exists, inform the user and suggest running `/issue-analyze <url>` first.

### 3. Fetch Latest Issue State

Fetch the current issue state including new comments, label/assignee/status changes, and any linked PRs or cross-references:
- **GitHub**: `gh issue view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab issue view <number> --repo <org>/<repo>`
- **Other forges**: Fetch the issue URL directly or use the forge's API

### 4. Check Git State

- Run `git status` to see current working changes
- Run `git log --oneline -10` to see recent commits
- Note whether there is an active branch related to this issue

### 5. Update the Analysis

Update `issue-<number>-analysis.md` to reflect:
- Date of this resume session
- New information from the issue (new comments, status changes, linked PRs)
- Current git state (branch, relevant recent commits, WIP changes)
- Revised understanding of the problem or fix approach, if applicable
- Updated next steps

### 6. Brief the User

Provide a concise summary covering:
- What the issue is about
- What has been done so far
- Current state (branch, uncommitted changes)
- What the logical next steps are

### 7. Ongoing Updates

Continue updating the analysis file throughout the conversation as new information is discovered, decisions are made, or the approach evolves. Any files relevant to the issue can be added to the issue directory at any time — by the AI agent or the human user.
