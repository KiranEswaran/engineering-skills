# Agent Audit Requirements

These rules apply to AI agents operating in workspaces. They are designed to make work **reversible**, **verifiable**, and **auditable**.

## 1. Remote Writes: Default Deny

By default, AI agents MUST NOT modify any remote state.

If (and only if) the user explicitly authorizes a specific remote write, the agent may execute only the authorized command(s) and MUST record:
- The exact authorization (who/when/what)
- The exact commands executed
- The results and exit codes

**Disallowed operations include:**
- `git push`, forced updates, tag pushes
- Creating/merging PRs, pushing branches
- `terraform apply`, `kubectl apply`, `helm upgrade`
- Database migrations against non-local DBs
- Any command that changes remote resources

**Commits are local-only** and allowed only after explicit user authorization.

## 2. Local Branch Discipline

All work MUST be performed on the **current branch**.

Requirements:
- Record the baseline branch name and `HEAD` SHA in the audit report
- Create a local backup branch before making changes:
  - `DTTM="$(date +%Y%m%d_%H%M%S)"`
  - `git branch "checkout-point/${DTTM}" HEAD`
- Do not fast-forward, rebase, or otherwise rewrite shared history unless explicitly authorized

## 3. Backups & Checkpoints (Mandatory)

Before making changes, create **identifiable, reversible checkpoints**:

**Minimum checkpoints:**
- **Baseline identifier:** Record `HEAD` SHA and current branch name
- **Working tree backup:** Create stash with untracked files: `git stash push -u -m "checkpoint/<id>"`
- **Rollback anchor:** Create local checkpoint: `git branch "checkout-point/<id>" <baseline-sha>`

**Checkpoint rules:**
- Each checkpoint MUST have a unique `<id>` (recommend `YYYYMMDD_HHMMSS`)
- Every checkpoint MUST be recorded with timestamp in the audit report

## 4. Local Verification Gate (Mandatory)

Before proposing any commit messages, run local verification as applicable:
- Unit/integration tests
- Lint
- Formatting
- Type checks
- Build/package step

If verification cannot run, state:
- The exact reason
- The closest substitute run instead
- What evidence was relied on

## 5. Verifiable Audit Report (Mandatory)

For every non-trivial task, write one report file:

**Path rules:**
- If repo has `extras/` folder that is gitignored: `extras/agent_reports/agent_report_<repo>_<branch>_<timestamp>.md`
- Otherwise: `/tmp/agent_report_<repo>_<branch>_<timestamp>.md`

**Required contents:**
- Start and end timestamps (local time and UTC)
- Repo name, branch name, HEAD SHA
- Every command executed (copy-pasteable) with exit codes
- Summary of changes: `git status`, `git diff --stat`, changed files
- Verification outputs
- Proposed commit messages
- Checkpoints/backups section with timestamps
- Any authorized remote-write operations

## 6. GitHub CLI Read-Only Mode

**Allowed:**
- `gh repo view`
- `gh issue list` / `gh issue view`
- `gh pr list` / `gh pr view`
- `gh api` GET requests only

**Disallowed:**
- `gh pr create`, `gh pr merge`
- `gh repo fork`
- Any `gh api` mutation (POST/PATCH/PUT/DELETE)

## 7. Required Sequence

```
Understand task → propose plan → create minimal change set → take checkpoints →
run verification → produce audit report → propose commit(s) → wait for approval
```

