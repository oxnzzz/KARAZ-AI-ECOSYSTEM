# Changelog

All notable changes to KARAZ AI ECOSYSTEM are documented here.

## Versioning

The repository version (`ecosystem.version` in `registry/skills.yaml`) and each skill's version (in its `SKILL.md` frontmatter and registry entry) are independent. Both follow Semantic Versioning (see `docs/SKILL-STANDARD.md`). A skill's version changes when that skill changes. The repository version changes only at a repository release; none has been made since 0.1.0.

## [Unreleased]

### Added

* Core operating standards specification (`core/README.md`).
* `quality-guardian` core skill, draft. Version history: 0.1.0 (initial specification), 0.2.0 (security and secret-exposure review), 0.3.0 (credential authenticity, scanning-tool guidance, machine-readable reports).
* Quality Guardian test specifications (QG-001 to QG-013) and scenario-evaluation records.
* Development roadmap (`docs/DEVELOPMENT-ROADMAP.md`).
* Registry entry and platform compatibility record for `quality-guardian` (draft; platform compatibility unverified).
* JSON Schemas (draft 2020-12, schema version 1.0) for skill frontmatter, the skills registry, and the platform compatibility registry (`schemas/`).
* Portable format and normative requirements NR-1 to NR-8 (`docs/SKILL-STANDARD.md` §13), with the existing rules they build on listed separately.
* Adapter contract, including adapter status values and the authoritative wording of NR-5 (`adapters/README.md`).
* Read-only structural validator (`tools/karaz_validate/`, run as `python -m tools.karaz_validate [--root PATH] [--format text|json]`) with 28 checks classified as existing rules, new normative requirements, or diagnostics. Exit 0 means no ERROR, 1 means an ERROR, 2 means the validator could not run; WARNING and INFO never fail a run. Files larger than 1 MiB are rejected (KZ-L001).
* Hash-locked dependency install for the validator (`tools/requirements.in`, `tools/requirements.txt`; PyYAML and jsonschema).
* Structural test suite (`tests/structure/`, standard-library `unittest`) with a canonical fixture and a pinned backward-compatibility baseline of the real records (`tests/structure/fixtures/baseline-1.0/`).
* `.gitignore` for Python cache files and virtual environments.
* Pull-request workflow (`.github/workflows/validate.yml`) that installs the hash-locked dependencies, runs the structural validator, and runs the structural tests. It runs on `pull_request` and `workflow_dispatch`, with read-only token permissions, no secrets, and third-party actions pinned by commit SHA.
* Contributor guide (`CONTRIBUTING.md`).
* Evidence and provenance model (`docs/EVIDENCE-MODEL.md`): three separate levels (structural validation, evidence of performance, human verification), an evidence record template, independence levels, verification rules, and the qualification rule for security-scan evidence. Documentation only; no schema is added.
* Evidence requirements EV-1 and EV-2 for the `validated` and `released` statuses (`docs/SKILL-STANDARD.md` §14). The validator checks the declarations (`KZ-G001`, `KZ-G002`); that a reviewed, qualifying evidence record exists is enforced by pull request review.
* Validator checks `KZ-G001` (a `validated` or `released` skill declares `validation.behavioral_scenario_evaluation: "recorded"`) and `KZ-G002` (a `released` skill declares `validation.repository_security_scan: "recorded"`), both at ERROR level, with tests (`tests/structure/test_lifecycle_gate.py`). The validator now has 30 checks; the 28 earlier checks and the frozen baseline are unchanged. The checks read declarations only.
* Future source-material research list, with the license, attribution, and provenance principle (`docs/DEVELOPMENT-ROADMAP.md` §7). Research sources only; none is a dependency and nothing was imported.

* First evidence record, `docs/evidence/EV-20261121-01.md`: a repository security scan with Gitleaks 8.30.1 (official release binary, default ruleset, offline, redacted). It covered the full history of all 13 refs of a fresh mirror clone (43 commits) and the working tree at `main` commit `87eb795480c8768e1ffe8e8cb97dc5b042c30d2b`. Result: 0 findings in two runs each, byte-identical. The record is `unreviewed`, and states that the binary contains no embedded source-revision metadata, so its build provenance could not be tied to its source commit. The registry is unchanged: `repository_security_scan` stays `not_run`.

### Changed

* Foundation documents converted to Markdown (formatting only; wording unchanged).
* Test records reconciled: `test-results.md` is the single source of truth for execution status, and the skill version each test ran against is recorded.
* Adapter layout standardized to `adapters/generic/` and `adapters/<platform>/`.
* Status text in `README.md`, `core/README.md`, and the roadmap aligned with the repository's actual state.
* `docs/SKILL-STANDARD.md` §4: the `type` field now names the registry values `core`, `meta`, and `specialist`.
* `docs/ARCHITECTURE.md` §5: the proposed repository tree lists `schemas/`, `tools/`, and `adapters/README.md`.
* `docs/ARCHITECTURE.md` §5: the tree also lists `CONTRIBUTING.md`, `.github/workflows/`, `docs/EVIDENCE-MODEL.md`, and `docs/DEVELOPMENT-ROADMAP.md`.
* Setup of three existing registry tests and one helper (`tests/structure/test_registry_checks.py`) now declares `behavioral_scenario_evaluation` and `repository_security_scan` as `recorded`, so that they keep testing KZ-R006 and KZ-R008 only. Their assertions are unchanged.
* `README.md` status and repository guide, `CONTRIBUTING.md` (new Validation and evidence section), `docs/IMPORT-AND-VERIFICATION.md` §6 (cross-reference), and `docs/DEVELOPMENT-ROADMAP.md` §7 (P2 status) updated.

### Notes

* No skill is validated or released. `quality-guardian` remains a draft.
* Quality Guardian results are behavioral scenario evaluations only. A repository security scan has been run (`docs/evidence/EV-20261121-01.md`) but is unreviewed and not declared in the registry.
* The structural validator checks structure only. A passing run, locally or in the `validate` workflow, does not validate or release any skill. No registry file or skill file was changed.
* The evidence model is documentation, and the gates check declarations only. One evidence record in its format exists (the unreviewed security scan above); no record has been reviewed.

## [0.1.0] - Initial Foundation

### Added

* Initial repository structure and architectural direction.
* Platform-independent skill ecosystem principles.
* Separation between AI identity and reusable capabilities.
* Skill lifecycle and quality standards.

### Planned

* Core operating standards.
* Meta-skills framework.
* Specialist skills migration and improvement.
* Platform adapters.
* Validation and regression testing.

### Notes

No specialist skills have been migrated or released in this version.
