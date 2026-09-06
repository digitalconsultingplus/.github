# Security Policy

Digital Consulting Plus treats security reports as sensitive until they can be assessed and remediated responsibly.

This organization-level policy is the default for repositories that do not define a repository-specific `SECURITY.md`.

## Reporting a vulnerability

**Do not report sensitive vulnerabilities in public GitHub issues, discussions, pull requests or other public channels.**

Preferred reporting path:

1. use the repository's private vulnerability reporting / GitHub Security Advisory flow when it is enabled; or
2. contact Digital Consulting Plus at **info@digitalconsultingplus.com** and include enough information to reproduce and assess the issue without sending unnecessary secrets or personal/customer data.

If a repository defines a more specific private reporting channel, use that channel instead.

## Scope

Reports may cover, where applicable:

- Digital Consulting Plus public repositories;
- organization-owned applications and services represented by those repositories;
- authentication/authorization failures;
- injection, data exposure or privilege escalation;
- dependency and software supply-chain risks;
- insecure CI/CD behavior;
- secret exposure;
- infrastructure or deployment weaknesses that are directly evidenced by repository behavior.

Client-owned systems, third-party services and repositories not controlled by Digital Consulting Plus may require a different disclosure path. Do not test systems without authorization.

## Secrets and credentials

Never commit or publish:

- API keys or access tokens;
- passwords;
- private keys or certificates containing private material;
- cloud credentials;
- production `.env` values;
- customer data or confidential client information.

If a secret is exposed, treat it as compromised: revoke/rotate it, assess blast radius and remove it from active use. Removing a secret from the latest commit alone is not sufficient remediation.

## Dependencies and supply chain

Security review should consider direct and transitive dependencies, GitHub Actions, containers and other build/runtime inputs proportionally to risk.

External GitHub Actions used in DCP repositories must follow the current DCP Flow GitHub controls, including immutable commit-SHA pinning where required by that standard.

Major dependency upgrades must not be globally auto-merged by default. Material dependency changes require applicable Quality Gates and review.

## Responsible disclosure

Please provide:

- affected repository/component;
- clear impact;
- reproduction steps or proof of concept where safe;
- relevant versions/commit identifiers;
- suggested mitigation if known.

Do not include exploit payloads, secrets or customer information beyond what is necessary to demonstrate the issue.

Digital Consulting Plus will assess the report, determine ownership and remediation path, and coordinate disclosure when appropriate. A report does not authorize destructive testing, persistence, lateral movement or access to unrelated data.

## Public vs private repositories

A public repository may expose source code but must not expose operational secrets or confidential client information. Private repositories still require the same security discipline; repository visibility is not a substitute for access control, secret management or secure development practices.

## DCP Flow

Security decisions and Quality Gates are governed by the current [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow) standards. This file defines the organization-level reporting and handling baseline, not a duplicate security methodology.
