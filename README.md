# my-skills

Personal [Agent Skills](https://agentskills.io/) for use with Claude Code.

## Skills

### `issue-analyze`

Analyzes an issue from any Git forge (GitHub, GitLab, etc.) by:

1. Creating a structured directory at `.issues/<hub>/<repo>/<number>/` in the project root
2. Fetching the full issue including all comments
3. Investigating the relevant code in the current codebase
4. Writing a detailed analysis markdown file (`issue-<number>-analysis.md`) covering the problem description, code investigation, proposed fix approach, and open questions

If the issue has already been analyzed in a prior session (the issue directory and analysis file already exist), the skill instead resumes work on it: it reads the existing analysis and any other files in the issue directory, fetches the latest issue state from the forge (new comments, status changes, linked PRs), checks the current git state (branch, recent commits, working changes), updates the analysis with the new information and revised next steps, and briefs you on what has been done and what comes next.

The analysis file is kept up to date throughout the conversation as new understanding is gained. Any supporting material — test scripts, example data, diagrams, reference files — can be placed in the issue directory by either the AI agent or the user.

**Usage:** `/issue-analyze <issue-url>`

**Example:** `/issue-analyze https://github.com/dandi/dandi-cli/issues/1606`

---

## Directory convention

Issue directories follow this structure:

| Forge | Path |
|-------|------|
| GitHub | `.issues/<org>/<repo>/<number>/` |
| GitLab and others | `.issues/<hostname>/<org>/<repo>/<number>/` |

## Installation

Symlink the skills you want into `~/.claude/skills/`:

```bash
ln -s /path/to/my-skills/issue-analyze ~/.claude/skills/issue-analyze
```
