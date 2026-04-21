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

Copy [`assets/analysis-template.md`](../assets/analysis-template.md) to `issue-<number>-analysis.md` in the issue directory, then fill in each section based on the issue content and your investigation. Leave the **Session Log** section empty — it is populated on subsequent resume sessions.
