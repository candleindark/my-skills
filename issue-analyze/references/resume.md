# Resume Path

Follow these steps when a prior analysis file already exists for the issue.

## 1. Read Existing Analysis

Read all files present in the issue directory, particularly `issue-<number>-analysis.md`. Note all other resources in the directory — any file placed there, whether by the AI agent in a prior session or by the human user, may be relevant.

## 2. Fetch Latest Issue State

Fetch the current issue state including new comments, label/assignee/status changes, and any linked PRs or cross-references:

- **GitHub**: `gh issue view <number> --repo <org>/<repo> --comments`
- **GitLab**: `glab issue view <number> --repo <org>/<repo>`
- **Other forges**: Fetch the issue URL directly or use the forge's API

## 3. Check Git State

- Run `git status` to see current working changes
- Run `git log --oneline -10` to see recent commits
- Note whether there is an active branch related to this issue

## 4. Update the Analysis

Append a dated entry to the **Session Log** section of `issue-<number>-analysis.md` covering:

- New information from the issue (new comments, status changes, linked PRs)
- Current git state (branch, relevant recent commits, WIP changes)
- Updated next steps

Also revise the earlier sections (Problem Description, Code Investigation, Proposed Fix Approach, Open Questions) in place if the understanding of the issue or fix has changed — the body of the document should always represent the current best understanding, while the Session Log preserves the chronology.

If `issue-<number>-background.md` exists, treat it as an active companion: read it alongside the analysis, and update or extend it whenever new domain/protocol/reference knowledge surfaces (typically through user Q&A) that helps a reader understand the issue but is not specific to its resolution. If it does not yet exist and such information arises in this session, create it. Ensure the analysis file's "Companion files" section reflects the current state.

## 5. Brief the User

Provide a concise summary covering:

- What the issue is about
- What has been done so far
- Current state (branch, uncommitted changes)
- What the logical next steps are
