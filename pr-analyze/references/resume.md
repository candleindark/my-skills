# Resume Path

Follow these steps when a prior analysis file already exists for the PR.

## 1. Read Existing Analysis

Read all files present in the PR directory, particularly `pr-<number>-analysis.md`. Note all other resources in the directory — any file placed there, whether by the AI agent in a prior session or by the human user, may be relevant.

## 2. Fetch Latest PR State

Fetch the current PR state including new commits, new comments and review comments, label/reviewer/status changes, CI status, and any linked issues:

- **GitHub**: `gh pr view <number> --repo <org>/<repo> --comments` and `gh pr checks <number> --repo <org>/<repo>`
- **GitLab**: `glab mr view <number> --repo <org>/<repo> --comments`
- **Other forges**: Fetch the PR URL directly or use the forge's API

## 3. Re-run the Built-in Review

If the PR has new commits since the prior analysis, re-invoke the built-in `/review` skill via the Skill tool (skill name: `review`, args: the PR URL or number) so the analysis reflects the current diff. If the PR is unchanged, skip this step.

## 4. Check Git State

- Run `git status` to see current working changes
- Run `git log --oneline -10` to see recent commits
- Note whether the local branch corresponds to this PR's head

## 5. Update the Analysis

Append a dated entry to the **Session Log** section of `pr-<number>-analysis.md` covering:

- New information from the PR (new commits, new comments, status/CI changes, linked issues)
- Findings from the re-run review, if any
- Current git state (branch, relevant recent commits, WIP changes)
- Updated next steps

Also revise the earlier sections (Summary of Changes, Review Findings, Recommendation, Open Questions) in place if the understanding of the PR has changed — the body of the document should always represent the current best understanding, while the Session Log preserves the chronology.

If `pr-<number>-background.md` exists, treat it as an active companion: read it alongside the analysis, and update or extend it whenever new domain/protocol/reference knowledge surfaces (typically through user Q&A) that helps a reader understand the PR but is not specific to its changes. If it does not yet exist and such information arises in this session, create it. Ensure the analysis file's "Companion files" section reflects the current state.

## 6. Brief the User

Provide a concise summary covering:

- What the PR is about
- What the review surfaced (key findings, blockers, nits)
- Current state (CI, branch, uncommitted changes)
- What the logical next steps are
