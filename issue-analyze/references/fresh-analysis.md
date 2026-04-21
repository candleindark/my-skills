# Fresh Analysis Path

Follow these steps when no prior analysis exists for the issue.

## 1. Create the Issue Directory

Create the issue directory (determined in the main `SKILL.md`) relative to the current working directory (project root).

## 2. Fetch the Issue

Fetch the full issue including title, body, labels, assignees, and all comments using the appropriate method:

- **GitHub**: `gh issue view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab issue view <number> --repo <org>/<repo>`
- **Other forges**: Fetch the issue URL directly or use the forge's API

## 3. Investigate the Codebase

Before writing the analysis, actively investigate:

- Find the relevant files and functions related to the reported behavior
- Read and trace the relevant code paths
- Understand the logical flow that produces the reported problem
- Look for related tests

## 4. Create the Analysis File

Create `issue-<number>-analysis.md` in the issue directory with these sections:

- **Issue Summary**: Title, URL, author, labels, assignees, current state, date of analysis
- **Problem Description**: What the issue reports, including error output and reproduction steps from the issue body and comments
- **Additional Context**: Explanation of why the problem occurs and what the expected behavior should be
- **Code Investigation**: Relevant files and functions with paths and line numbers; explanation of current behavior and why it causes the problem
- **Proposed Fix Approach**: High-level description of how to address the issue
- **Open Questions**: Anything unclear that needs further investigation or discussion
