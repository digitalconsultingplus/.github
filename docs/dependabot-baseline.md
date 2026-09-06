# Dependabot baseline

Dependabot configuration is **repository-specific**. A `.github/dependabot.yml` placed in the organization `.github` repository does not automatically configure other repositories.

This document defines the organization baseline to apply locally where Dependabot is appropriate.

**Languages:** **English** · [Español](dependabot-baseline.es.md) · [Català](dependabot-baseline.ca.md)

## Separate dependency ecosystems

Configure only ecosystems actually present in the repository.

Typical categories:

1. **Application dependencies** — Composer, npm/pnpm/yarn, pip/Poetry, etc.
2. **GitHub Actions** — workflow Action references.
3. **Docker** — base images when the repository builds or operates containers.

Do not add package ecosystems that do not exist merely to satisfy a template.

## Update strategy

Recommended baseline:

- run dependency checks on a predictable cadence appropriate to the repository;
- group low-risk patch/minor updates only when it improves reviewability;
- keep major upgrades separate unless there is an explicit migration plan;
- never globally auto-merge major upgrades by default;
- review lockfile changes and transitive impact;
- run the repository's applicable Quality Gates before merge;
- treat CI Actions and container images as supply-chain dependencies, not only application libraries.

## Security updates

Security updates should be prioritized by exploitability, exposure, affected runtime and business impact rather than CVE score alone.

A security alert does not justify bypassing tests, migration review or human acceptance when the update itself is material. Emergency exceptions must follow the repository's break-glass process when one is required.

## GitHub Actions

DCP Flow requires external Actions to be pinned to immutable full commit SHAs. Dependabot may propose Action updates, but the resulting workflow must still satisfy that pinning policy and be reviewed as a CI dependency change.

## Docker

Use Docker update monitoring when the repository owns Dockerfiles or container-image references that materially affect runtime/build behavior.

Review:

- base-image provenance;
- digest/tag semantics;
- runtime compatibility;
- operating-system/package changes;
- image size/security impact;
- build and smoke evidence when applicable.

## Example shape

The following is illustrative only. Copy and adapt it inside the destination repository; do not assume these ecosystems or directories exist everywhere.

```yaml
version: 2
updates:
  - package-ecosystem: github-actions
    directory: /
    schedule:
      interval: weekly

  # Add only when the repository actually uses this ecosystem.
  # - package-ecosystem: composer
  #   directory: /
  #   schedule:
  #     interval: weekly

  # - package-ecosystem: npm
  #   directory: /
  #   schedule:
  #     interval: weekly

  # - package-ecosystem: docker
  #   directory: /
  #   schedule:
  #     interval: weekly
```

Repository maintainers should add ownership, labels, grouping, target branch and cadence only when those values are known and supported by repository truth.

## Why there is no organization-wide `dependabot.yml` here

Adding one would configure this `.github` repository itself, not the rest of the organization. Presenting it as inherited governance would be technically false. The correct model is a documented baseline plus repository-local configuration or future explicit automation that creates/maintains those files with evidence.
