---
docType: agent-contract
scope: repo
status: current
authoritative: true
owner: wiki
language: en
whenToUse: "Before changing Wiki CLI, indexing, daemon, dashboard, MCP or packaged instructions."
whenToUpdate: "When governance, repository boundaries or validation commands change."
checkPaths:
  - .docpact/config.yaml
  - .github/workflows/docpact.yml
  - package.json
  - src/**
  - dashboard/**
  - mcp-server/**
lastReviewedAt: 2026-09-19
lastReviewedCommit: ff89b20ff975aa283c53fb5b6f15c397b029dda4
---

# Tiangong Wiki Agent Contract

This repository owns the local-first Wiki CLI, index/query engine, daemon,
dashboard and MCP adapter distributed together as `@tiangong-ai/wiki`.

## Read before changing

1. Read this file and `.docpact/config.yaml` (`layout: repo`).
2. Use docpact 0.1.9: `cargo install docpact --version 0.1.9 --locked`.
3. Run `docpact route --root . --paths <changed-paths> --format json` and read
   the recommended existing references. Use `README.md` / `README.zh-CN.md`
   for user-facing setup and `references/cli-interface.md` for CLI behavior.
4. Read `package.json` and the affected implementation before editing.

## Boundaries

- User Wiki pages, vault files, databases, credentials and `.wiki.env` are
  external runtime data. Do not treat them as repository fixtures or rewrite
  them during validation.
- Keep CLI, daemon, dashboard and MCP authentication/discovery contracts aligned
  with `references/centralized-service-deployment.md` and troubleshooting.
- Preserve packaged `SKILL.md`, `agents`, `assets` and references semantics;
  package validation must use the allowlist in package.json.
- Use the existing npm lockfile and scripts. Do not duplicate parent workspace
  Project, branch or integration policy in this standalone repository.

## Validation and delivery

Run `docpact validate-config --root . --strict` and
`docpact lint --root . --staged --mode enforce` (stage new files explicitly).
For committed changes use `--base <sha> --head <sha>` instead of `--staged`.
Record genuine document review with `docpact review mark --root . --path <doc>`.
GitHub PRs enforce the same pinned CLI without needing the parent workspace.

For implementation/build changes run `npm ci` and `npm test`; existing CI
also builds/packs/installs the package on its configured OS matrix. Do not
publish or tag for documentation-only onboarding. Child merges still need an
explicit parent integration update when used by a workspace; a local checkout
or green child PR does not automatically update a parent gitlink.
