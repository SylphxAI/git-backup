# GitBackup Project

GitBackup is a local developer CLI for backing up GitHub repositories that a
user-provided Personal Access Token can access. It discovers repositories,
clones missing backups, and force-updates existing backup directories to match
their remote default branches.

## Lifecycle

- Lifecycle: `active`
- Layer: `tooling`
- Doctrine source of truth: [SylphxAI/doctrine](https://github.com/SylphxAI/doctrine)
- Machine manifest: `.doctrine/project.json`

## Goals

- Provide a simple local backup CLI for personal or organization GitHub repos.
- Keep repository discovery, PAT handling, backup-root selection, clone, fetch,
  and force-update behavior explicit and observable.
- Preserve Docker and scheduler-friendly operation without hidden hosted
  collection or background sync.

## Non-Goals

- Do not become a hosted backup service or telemetry collector.
- Do not support non-GitHub providers without a new public contract decision.
- Do not silently broaden token scopes, repository selection, or destructive
  filesystem behavior.

## Boundaries

GitBackup owns GitHub repository discovery through the CLI auth boundary, local
backup directory management, Docker packaging, and user-facing backup docs. It
does not own GitHub account policy, remote repository state, credential
issuance, OS scheduler configuration, or files outside the configured backup
root.

## Public Surfaces

- CLI package: `packages/gitbackup-cli/package.json`
- CLI entry point: `packages/gitbackup-cli/src/main.ts`
- User docs: `README.md`
- Docker packaging: `Dockerfile`

## Delivery

At adoption time this repository has no GitHub Actions workflow on the branch.
Local validation commands are documented in `AGENTS.md`; durable CI and package
publishing remain adoption gaps before this repo can claim full doctrine
adoption or a production package-release path.

