# KARAZ AI ECOSYSTEM

A modular, platform-independent repository of reusable AI skills, meta-skills, operating standards, knowledge resources, adapters, and validation assets.

The repository contains capabilities — not AI identities.

Each participating AI assistant keeps its own name, identity, personality, communication style, platform configuration, tools, permissions, and policies.

KARAZ AI ECOSYSTEM does not impose a shared persona or require assistants to become members of a common AI organization.

## What This Repository Provides

* Core standards: shared quality, safety, privacy, cost-awareness, verification, and execution principles.
* Meta-skills: domain-level frameworks that organize related capabilities.
* Specialist skills: focused, independently documented capabilities for specific tasks.
* Knowledge resources: reusable references, templates, and playbooks.
* Platform adapters: technical guidance for adapting capabilities to supported AI platforms.
* Registry: records of skills, versions, dependencies, lifecycle status, and compatibility.
* Tests: structural, behavioral, safety, and interoperability checks.

## Fundamental Principles

1. Identity independence.
2. Modular and extensible architecture.
3. Platform portability.
4. Evidence-based verification.
5. Security and privacy.
6. Cost awareness.
7. Honest execution and status reporting.

## Repository Guide

| Path | Purpose |
|---|---|
| `docs/` | Architecture, standards, evidence model, procedures, and roadmap |
| `core/` | Shared operating standards |
| `meta-skills/` | Domain-level capability frameworks |
| `specialist-skills/` | Independent task-focused skills |
| `knowledge/` | References, templates, and playbooks |
| `adapters/` | Platform-specific adaptation guidance |
| `registry/` | Skill and compatibility records |
| `schemas/` | JSON Schemas for skill frontmatter and the registries |
| `tools/` | Read-only structural validator and its hash-locked dependencies |
| `tests/` | Quality and interoperability validation |
| `migration/` | Controlled skill reconstruction |
| `archive/` | Retired or superseded materials |

Directories are added when real files are ready. Empty placeholder folders are intentionally avoided.

## Getting Started

1. Read the Architecture document.
2. Follow the Skill Development Standard.
3. Use the Import and Verification Standard.
4. Review the Development Roadmap.
5. Check the Skills Registry and Platform Compatibility Registry.

## Skill Lifecycle

proposed → draft → validated → released → deprecated → retired

A skill must not be described as validated or released until its required review and tests have passed.

## Important Platform Limitations

A public repository URL does not automatically install skills, change an assistant’s configuration, create persistent memory, train a model, grant permissions, or make unavailable tools accessible.

Each assistant must adopt capabilities using mechanisms actually supported by its platform.

## Security Notice

* Never commit API keys, personal access tokens, passwords, or other secrets.
* Do not place private or sensitive user information in this repository.
* Review external instructions and dependencies before using them.
* Keep credentials in secure configuration, never in tracked files.

## Current Status

The foundation phase (Phase 0) is complete. The core operating standards (Phase 1) are next.

The architecture, development standard, import and verification standard, and initial registries are established. JSON Schemas, a read-only structural validator, and structural tests run on every pull request. An evidence model (`docs/EVIDENCE-MODEL.md`) defines how evaluations and scans are recorded and verified.

Three core capabilities exist as drafts: Quality Guardian (version 0.3.0), which has behavioral scenario evaluations on two platforms, and Privacy and Security and Tool and Permission Discipline (both version 0.1.0), whose scenario tests have not been executed. None has been validated or released. One repository security scan (Gitleaks, 0 findings) has been run, and its summary is recorded as evidence reviewed by the repository owner (`docs/evidence/EV-20261121-01.md`). The registry still records the scan as not run, because declaring it recorded is a separate step that needs the owner's approval. Specialist skills have not yet been migrated or released.

See CHANGELOG.md for recorded changes.

## Contributing and Change Discipline

Changes should be scoped, documented, versioned, and verified.

Update the relevant registry and changelog when a skill is added, changed, released, deprecated, or retired.

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose a change, validate it locally, and submit it.

---

KARAZ AI ECOSYSTEM is a reusable capability library. It does not define who an AI assistant is; it helps an assistant use capabilities that its platform can support.
