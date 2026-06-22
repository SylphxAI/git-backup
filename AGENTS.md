# GitBackup Agent Instructions

## Scope

This file is the repo-local operating policy for agents working in
`SylphxAI/git-backup`. Org-wide engineering doctrine is owned by
`SylphxAI/doctrine`; `PROJECT.md` and `.doctrine/project.json` own this
repository's local identity, lifecycle, boundary, and delivery facts.

GitBackup is a local CLI for backing up GitHub repositories that a user-selected
GitHub Personal Access Token can access. The tool may clone repositories and may
force-update local backup directories to match remotes, so repository selection,
credential handling, and destructive local filesystem behavior must stay explicit
and observable.

## Read First

Before proposing or implementing changes, read the smallest relevant set of
these source-of-truth documents:

1. `PROJECT.md` and `.doctrine/project.json` — project goal, lifecycle,
   boundaries, public surfaces, delivery proof, and adoption gaps.
2. `README.md` — user-facing behavior, PAT setup, Docker/cron usage, and the
   documented warning that updates may discard local changes in backup dirs.
3. `memory-bank/projectbrief.md` — product goal, core features, target user, and
   non-goals.
4. `memory-bank/systemPatterns.md` — authentication, Octokit, simple-git,
   configuration, scheduling, and ESM patterns.
5. `packages/gitbackup-cli/package.json` and `packages/gitbackup-cli/src/main.ts`
   before changing CLI behavior.
6. Root `package.json`, `turbo.json`, and validation config before changing
   build/test/lint workflows.

## Non-Negotiables

- Do not commit GitHub PATs, tokens, `.env` files, backup contents, private repo
  URLs beyond user-provided examples, or generated credentials.
- Keep backup-destination behavior explicit. Any operation that can overwrite or
  discard local files must be documented, test-covered where possible, and gated
  by clear user intent.
- Do not broaden GitHub token scopes, organization access, or repository
  selection semantics silently.
- Keep GitHub API access behind the CLI auth/config boundary; do not add hidden
  service-side collection, hosted retention, or telemetry for repository data.
- Preserve the default local/offline backup model. Scheduled execution belongs to
  OS schedulers or a future explicit scheduler design, not hidden background
  behavior.
- Treat Docker volume paths and backup directories as user-owned data; never
  delete outside the configured backup root.
- Use branch → commit → PR. Do not push directly to `master`, force-push, merge,
  publish, or release without manager-visible evidence and required gates.

## Workflow

1. Identify the owning boundary: GitHub API discovery, auth/config, git clone or
   reset behavior, backup root/path safety, CLI UX, Docker packaging, scheduling
   docs, or validation workflow.
2. Check open PRs/issues for the same boundary before editing.
3. Prefer small, evidence-backed slices with docs and tests for behavior changes.
4. For destructive/local-filesystem behavior changes, include before/after
   examples and failure-mode coverage.
5. Never paste real tokens into issues, PRs, docs, logs, tests, or fixtures.

## Validation

Use the narrowest meaningful validation first, then broaden as needed:

- `npm run build`
- `npm run test`
- `npm run lint`
- `npm run validate`

Docs-only boundary changes may be validated by reviewing the diff and checking
referenced files exist. CLI behavior changes need targeted tests and, where
safe, a dry-run or fixture-backed smoke test that does not touch real repos.

## Reporting

When reporting completed work, include changed files, boundaries read, validation
run, PR/issue links, and residual risk. Be explicit when no runtime behavior,
credential handling, filesystem mutation, Docker behavior, npm package, or
release state changed.
