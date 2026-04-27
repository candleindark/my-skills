# Fresh Analysis Path

Follow these steps when no prior analysis exists for the PR.

## 1. Create the PR Directory

Create the PR directory (determined in the main `SKILL.md`) relative to the current working directory (project root).

## 2. Fetch the PR

Fetch the full PR including title, body, author, labels, reviewers, state, base/head branches, and all comments and review comments using the appropriate method:

- **GitHub**: `gh pr view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab mr view <number> --repo <org>/<repo> --comments`
- **Other forges**: Fetch the PR URL directly or use the forge's API

## 3. Run the Built-in Review

Invoke the built-in `/review` skill on this PR via the Skill tool (skill name: `review`, args: the PR URL or number). Capture its output — this is the core technical review of the changes.

## 4. Create the Analysis File

Copy [`assets/analysis-template.md`](../assets/analysis-template.md) to `pr-<number>-analysis.md` in the PR directory, then fill in each section based on the PR metadata, the `/review` output, and any further investigation. Leave the **Session Log** section empty — it is populated on subsequent resume sessions.
