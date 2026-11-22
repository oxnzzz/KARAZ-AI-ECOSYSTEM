# KARAZ AI ECOSYSTEM — Evidence and Provenance Model

## 1. Purpose

This document defines how evidence is recorded in this repository: what must be written down when someone says an evaluation, scan, or test was performed, who did it, and how a person confirms it.

The repository's rule is that no capability is installed, activated, validated, or released without evidence (`docs/ARCHITECTURE.md` §7, `docs/IMPORT-AND-VERIFICATION.md` §2). This document makes "evidence" concrete so that a claim can be traced to its source, and so that the same record format works for any assistant or platform.

It describes records and rules only. It does not add a schema, a runtime, or a learning system.

## 2. Three separate levels

A registry flag such as `recorded` is a declaration. A declaration is never proof that an evaluation happened or passed. The model keeps three things apart.

| Level | What it is | Done by | What it shows | What it does not show |
|---|---|---|---|---|
| 1. Structural validation | The KARAZ validator and the `validate` workflow | Automation | A declared value is legal and a declared file exists | That anything was run, passed, or is true |
| 2. Evidence of performance | An evidence record (§3) with a preserved artifact | The executor | That a named executor claims to have performed the work, with traceable provenance | That the record is authentic or that the result is good |
| 3. Human verification | A reviewer checks the preserved artifact against the record and writes down what was checked | A human who is not the executor | Authenticity and quality, as judged by that reviewer on that date | Anything the reviewer did not examine |

Passing level 1 never replaces level 2 or level 3. A skill is `validated` or `released` only on the basis of levels 2 and 3 (`docs/SKILL-STANDARD.md` §14).

## 3. Evidence record

A record is a Markdown file that begins with these fields, in this order. The template is documentation. No JSON Schema defines it in this release.

```text
id:            EV-<yyyymmdd>-<nn>
type:          behavioral-scenario-evaluation | repository-security-scan
subject:       <skill-id>@<version> | repository@<commit-sha>
scope:         what was covered, and what was not
source:        what was run or supplied (path and commit, or supplied text and by whom)
executor:
  kind:        human | ai-assistant | automated-tool | workflow
  name:        platform, model, or tool name
  version:     exact version or model identifier, or "unknown"
  operator:    who ran or triggered it
performed_at:  UTC, ISO 8601
independence:  self | supplied-text | independent | automated
preserved_at:  repository path to a sanitized artifact | external reference | "not preserved"
result:        pass | fail | partial | inconclusive, followed by a one-line summary
limitations:   what the result does not establish
verification:
  status:      unreviewed | reviewed | disputed | superseded
  reviewer:    a named human | none
  reviewed_at: UTC, ISO 8601 | none
  basis:       what the reviewer checked
visibility:    public
supersedes:    <id of an earlier record> (optional)
```

The record body may add detail after the fields. It follows the rules in §5 to §8.

### 3.1 Field meanings

| Field | Requirement |
|---|---|
| `id` | Unique. Never reused. |
| `type` | One of the types in §4. |
| `subject` | The one thing the evidence is about. A skill is named with its exact version. A repository is named with the exact commit. |
| `scope` | States what was covered and what was not. A scope that cannot be stated makes the record `inconclusive`. |
| `source` | Where the input came from, so a reader can tell whether the executor saw the real repository or text a person supplied. |
| `executor` | The attribution of the work to an AI system, a platform, a tool, a workflow, or a person. It is factual provenance. It does not assign an identity or personality to anyone. |
| `performed_at` | When the work ran, in UTC. A record without it cannot be reviewed. |
| `independence` | §5. |
| `preserved_at` | Where a reader can find the artifact that backs the record. A sanitized artifact is stored in the repository. An external reference must state how long it is expected to remain available. `not preserved` is allowed and is limited by §6. |
| `result` | The outcome of this run. Failures and inconclusive runs are recorded like passes. |
| `limitations` | Mandatory. A record with no limitations written states "none known" and says what that rests on. |
| `verification` | §6. |
| `visibility` | `public` is the only value allowed in this repository (§8). |
| `supersedes` | Used to correct or replace an earlier record (§7). |

## 4. Evidence types

| Type | What it records | Status |
|---|---|---|
| `behavioral-scenario-evaluation` | Scenario tests of how a skill behaves, run by an assistant or a person | Defined |
| `repository-security-scan` | A scan of repository content for secrets, run by a tool (§9) | Defined |
| `platform-adoption` | The five adoption stages of `docs/IMPORT-AND-VERIFICATION.md` §3, reported separately for one platform | Reserved. Defined when adapters have real platform evidence (roadmap Phase 4) |
| `knowledge-verification` | Verification of a piece of shared knowledge before reuse | Reserved. Not defined or used in this release |

A behavioral scenario evaluation is never evidence that a security scan was run, and a security scan is never evidence of correct behavior. Each is reported separately.

## 5. Independence levels

Independence is measured against the session or assistant that loaded the skill and produced the result being checked.

| Level | Meaning |
|---|---|
| `self` | The same session or assistant that loaded the skill ran the evaluation. |
| `supplied-text` | A different system ran it on text a person supplied. It did not retrieve the repository itself. |
| `independent` | A different executor retrieved the content from the repository and the output was preserved. |
| `automated` | A tool or workflow ran with a recorded version and command. |

`self` evidence is allowed and is not hidden. It must state that it is `self`, and it carries less weight than `independent` evidence. The evidence recorded for Quality Guardian before this model existed is `self` and `supplied-text` (§10).

## 6. Verification rules

* A record starts `unreviewed`.
* A record becomes `reviewed` only when a human reviewer, who is not the executor, writes the reviewer's name, the date, and the basis: what was compared against what.
* A record that is `not preserved` can never be `reviewed`. There is nothing for the reviewer to check.
* Merging a pull request is not by itself a review. The review is written in the record.
* A reviewer may mark a record `disputed` with the reason in the basis.
* An assistant may prepare a record and may not review its own work. If an AI system executed the work, the reviewer is a human.
* A skill status of `validated` or `released` depends on records whose verification status is `reviewed` (`docs/SKILL-STANDARD.md` §14).

## 7. Preservation and correction

* A record is never silently edited after it is reviewed. A correction or replacement is a new record that names the older one in `supersedes`. The older record's status becomes `superseded` in that same change, with a pointer to the newer one.
* Preserved artifacts are sanitized. They contain no secret, credential, private user information, or other personal data. Raw scanner output is not committed, because it can contain the secret it found. A sanitized summary is committed instead.
* A preserved artifact that was edited after the record's `performed_at` says so.

## 8. Placement and privacy

| Scope of the evidence | Location |
|---|---|
| A skill | `<skill-directory>/evidence/<id>.md` |
| The repository | `docs/evidence/<id>.md` |

* Directories are created when the first real record is added.
* Only `public` evidence is stored here. This repository is public. Information that is private, user-specific, or tied to one person's account does not belong in it, in any record or artifact.
* If an evaluation depended on private context, the record states only that private context was used and nothing else about it.

## 9. Security-scan evidence

A security scan record states the tool, the exact tool version, the exact command, the commit scanned, and the scope (§3.1). It lists findings by tool fingerprint and disposition, never by secret value.

**A scan qualifies for the release gate only if it covers all of the following:**

1. The full Git history of all branches.
2. The current working tree at the commit named in `subject`.
3. A documented scope and documented limitations.

A narrower scan, for example the working tree only, may be recorded as evidence. It is marked as narrower and does not qualify for `repository_security_scan: "recorded"` in `registry/skills.yaml`.

One repository-wide scan can support several skills if each skill's directory is unchanged between the commit scanned and the release commit. The release pull request shows that with the output of `git diff <scanned-commit> <release-commit> -- <skill-directory>`.

An exception for a deliberate test fixture, such as the synthetic credentials in the Quality Guardian tests, is recorded as an exact finding. It is never a pattern, a path wildcard, or a change to the fixture. If a scan finds a real credential, the finding is not committed. The owner decides on revocation. Git history is not rewritten automatically.

## 10. Evidence recorded before this model

The Quality Guardian records in `core/quality-guardian/tests/test-cases.md` and `core/quality-guardian/tests/test-results.md` predate this model. They are behavioral scenario evaluations, run in the same session that loaded the skill (ChatGPT) or on text supplied by a person (Gemini). Complete responses are not preserved in the repository. They are not converted into records and are not edited by this document. They include no repository security scan; the first security-scan record is `docs/evidence/EV-20261121-01.md`.

Describing that evidence in this model's terms does not upgrade it.

## 11. Room for later work

The model is deliberately small. Nothing in this section is implemented, enforced, or required by this release. It records constraints so that later work does not need to replace the model.

| A later need | How the model already leaves room |
|---|---|
| Validating a skill on several platforms | Several records can have the same `subject`, told apart by `executor` and `independence` |
| Attributing evidence to the AI system, platform, or workflow that produced it | The `executor` fields |
| Provenance of knowledge learned while working with one assistant | `source`, and the reserved type `knowledge-verification` |
| Verifying shared knowledge before it is reused | `verification.status`, with a person as the reviewer |
| Telling verified knowledge from unverified observations, hypotheses, platform-specific information, obsolete information, and conflicting information | Reserved. A knowledge status can later be derived from evidence, review, `disputed`, `superseded`, and the platform in `executor`. It is not defined here. |
| Keeping private or user-specific information out of shared knowledge | `visibility` allows only `public`. Private information stays outside this repository. |
| Permission checks before knowledge is shared | Reserved. Not defined here. |
| Other systems reading evidence | Records are plain files. They do not depend on a runtime, a platform, or any one assistant. |

Each assistant keeps its own identity, personality, policies, tools, permissions, and memory. A record names the executor as a fact. It does not describe or define any assistant.

## 12. Current state

* One evidence record in the format of §3 exists: `docs/evidence/EV-20261121-01.md`, a repository security scan (Gitleaks 8.30.1, 0 findings). Its verification status is `unreviewed`. Its limitations include that the scanner binary's build provenance could not be tied to its source commit.
* `registry/skills.yaml` records `behavioral_scenario_evaluation: "recorded"` for `quality-guardian` on the basis of the earlier evidence in §10, and `repository_security_scan: "not_run"`.
* The validator checks that declared values are legal and that declared files exist. For `validated` and `released` skills it also checks the declarations required by EV-1 and EV-2 (KZ-G001, KZ-G002). It does not read evidence records. Requirements for `validated` and `released` skills are in `docs/SKILL-STANDARD.md` §14.
