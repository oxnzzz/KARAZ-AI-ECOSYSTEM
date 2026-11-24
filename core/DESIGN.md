# Core Layer Design

## 1. Purpose and authority

This document organizes the Core layer described in [README.md](README.md). It sets out who is responsible for what, how the ten areas relate, which of them become registered skills, and how a skill refers to a standard without copying it.

It is a draft normative design. It does not replace anything in `core/README.md`. Where this document and `core/README.md` disagree, `core/README.md` governs, and in particular its §5 (Instruction and Conflict Principles) governs precedence.

This document does not register a skill, change a registry, or create evidence. Quality Guardian is not changed by it.

## 2. Three different things

| Kind | What it is | Where it lives | Registered in `registry/skills.yaml`? |
|---|---|---|---|
| **Normative standard or document** | Binding rules, precedence, and contracts. It contains no procedure that an assistant performs on its own. | `docs/`, `core/README.md`, this document | No. It is referenced by section and link. |
| **Operational capability** | A procedure an assistant performs, with inputs, steps, outputs, and failure behavior | A skill directory with a `SKILL.md` | Only when it passes the test below |
| **Registered skill** | An operational capability that has its own version, lifecycle, tests, and compatibility record | `registry/skills.yaml` | Yes |

**Registration test.** An area is registered as a skill only if all four hold:

1. **Procedure.** An assistant performs an observable procedure.
2. **Testable.** Scenario tests with pass criteria can check it.
3. **Lifecycle.** It has a version and lifecycle that can change independently of other areas.
4. **Referenceable.** Another skill might depend on it by id.

The ten areas of `core/README.md` §3 are ten areas of responsibility. They are not ten skills, and none of them has to become one. An area that does not pass the test is documented as a normative standard. A skill is registered together with its own specification, never in advance of it.

## 3. Responsibility matrix

Each area is defined by the one question it answers.

| Area | Question it owns | Does not own (see) |
|---|---|---|
| Quality Guardian | Is this work correct, complete, evidenced, and safe to report as ready? | Resource optimization (Cost Controller); dedicated security handling rules (Privacy and Security); domain expertise (specialist skills) |
| Cost Controller | Is the way of doing this proportionate in tools, time, and money? | Correctness (Quality Guardian); the content of a plan (Task Understanding and Planning) |
| Privacy and Security | What must be protected, and what may be done with sensitive data, credentials, and untrusted content? | Checking a deliverable (Quality Guardian); authorization of tools (Tool and Permission Discipline) |
| Task Understanding and Planning | What is being asked, under what constraints, and how will it be done? | Using tools (Tool and Permission Discipline); recording progress (Execution Tracking) |
| Tool and Permission Discipline | Which tools and permissions are actually available and authorized, and is this action within them? | Rules for sensitive data (Privacy and Security); handling a failure (Fallback and Error Management) |
| Fallback and Error Management | What now that something failed, is unavailable, or is incomplete? | The status vocabulary (Execution Tracking); checking availability (Tool and Permission Discipline) |
| Execution Tracking | What is planned, attempted, completed, verified, or failed, reported truthfully? | Deciding correctness (Quality Guardian); platform adoption stages (`docs/IMPORT-AND-VERIFICATION.md` §3) |
| Capability Registry and Routing | Which registered capability's documented responsibility applies, and how is work handed off? | Acquiring capabilities (deferred); orchestration or scheduling; checking availability (Tool and Permission Discipline) |
| Conflict Resolution | When requirements or capabilities disagree, which governs, and was the conflict disclosed? | Defining the precedence rules (`core/README.md` §5) |
| Regression and Consistency Testing | Did a change silently break previously verified behavior? | Repository-level test and regression infrastructure (roadmap Phases 5 and 6) |

### 3.1 Overlaps and their owners

| Overlap | Resolution |
|---|---|
| Quality Guardian's security and secret-exposure review and Privacy and Security | Privacy and Security owns the handling rules. Quality Guardian owns checking that a deliverable follows them. Quality Guardian 0.3.0 already contains detailed exposure procedures and stays unchanged for now. A Privacy and Security specification refers to those rules and does not duplicate them. Removing the duplication is left to a later Quality Guardian version. |
| Execution Tracking, readiness decisions, and adoption stages | These are three separate vocabularies: work status, readiness for use, and platform adoption. They are not merged. |
| Conflict Resolution and `core/README.md` §5 | The rules are in `core/README.md` §5. This area applies them and requires disclosure. It does not restate or replace them. |
| Regression and Consistency Testing and the repository's structural tests | The Core area covers the behavior of a skill or an assistant against a previously verified baseline. Structural validation, the test suite, and the workflow belong to the repository (roadmap Phases 5 and 6). Neither side duplicates the other. |
| Quality Guardian section 8 (Tool and Evidence Discipline) and Tool and Permission Discipline | Tool and Permission Discipline owns whether an action is available, authorized, and within scope ([PERMISSION-MODEL.md](PERMISSION-MODEL.md)). Quality Guardian 0.3.0 keeps its section 8, which states a shorter version of the same discipline, and stays unchanged for now. A Tool and Permission Discipline specification refers to those rules and does not duplicate them. Removing the duplication is left to a later Quality Guardian version. |
| Quality Guardian section 9 (Failure Handling) and Fallback and Error Management | Quality Guardian owns the consequence of missing evidence for a verdict: name the gap, do not claim verification, and let the readiness decision stand. Fallback and Error Management owns the operational response to the failed operation: whether to retry, use an alternative inside the existing permissions, degrade, stop, or ask. Neither relaxes the other. A substitute check is never reported as the original verification. Quality Guardian 0.3.0 keeps its section 9 and stays unchanged for now. A Fallback and Error Management specification refers to it and does not duplicate it. |

## 4. Boundaries and precedence

1. **`core/README.md` §5 governs.** Platform policies and safety first, then security and privacy, then explicit task constraints, then specialized instructions within their scope, and unresolved conflicts are disclosed. Nothing below changes that order.
2. **Restrictions are not optimizations.** Privacy and Security, Tool and Permission Discipline, and the honesty rules of Execution Tracking are limits. Cost Controller, Planning, and Fallback work inside those limits and cannot relax them.
3. **Verified means evidence.** Quality Guardian's evidence standard decides what may be called verified. No area marks work verified without evidence.
4. **Skipped verification.** If the user explicitly asks to skip verification and the work is otherwise permitted, the work may proceed where appropriate. It must never be represented as verified because the user asked to skip verification. The assistant reports the actual verification state and discloses the meaningful limitations.
5. **Ownership tie-break.** The area whose question (§3) is being asked decides. Other areas contribute input and do not override. If ownership is unclear, the conflict is disclosed (`core/README.md` §5, item 5).
6. **Handoff is declarative and soft.** An area may ask another for input. It never assumes the other is active, and it works without it. Every Core capability stands alone and degrades gracefully. A handoff is a documented input or output, not a runtime mechanism. No orchestrator is part of this design.
7. **Identity and platform neutrality.** No area defines an assistant's name, personality, voice, or communication style. No area assumes a platform feature, tool, or permission. Each platform's adoption is described only in its own adapter.

### 4.1 Conflict Resolution (normative)

Conflict Resolution is a normative area. It is `core/README.md` §5 together with §4 above. An assistant applying it:

* identifies the conflicting requirements and which area or source owns each;
* applies the order in `core/README.md` §5 and the rules in §4 above;
* states the result and the reason, and discloses any conflict it could not resolve.

### 4.2 Capability Registry and Routing (normative contract)

This is a contract, not a procedure and not an orchestration layer.

* A capability declares its responsibility in its frontmatter (`purpose`, `scope`, `non_goals`, `inputs`, `outputs`) and in `registry/skills.yaml`.
* An assistant selects a capability by its documented responsibility and by whether it is actually available on the assistant's platform. It does not assume a capability is installed or activated (`docs/IMPORT-AND-VERIFICATION.md` §2).
* A handoff states what was passed on, what was expected back, and what was actually received.
* The assistant, under its own platform's rules, decides what to use. Nothing in this repository schedules, sequences, or runs capabilities.
* Acquiring or discovering new capabilities is outside this contract (§7).

## 5. Classification

This classification is provisional where marked. It is applied to each area when its specification is written. An area is registered only when it passes the test in §2.

| Area | Classification | Reasoning |
|---|---|---|
| Quality Guardian | Registered skill (exists, draft, 0.3.0) | Passes all four criteria. |
| Privacy and Security | Registered skill (draft, 0.1.0, registered in Phase 1 / WP-1.2) | It passes all four criteria: an observable procedure, testable, an independent lifecycle, and Quality Guardian already defers to it. |
| Tool and Permission Discipline | Registered skill (draft, 0.1.0, registered in Phase 1 / WP-1.3) | It passes all four criteria: an observable procedure before using a tool, testable, an independent lifecycle, and other capabilities rely on it. Its normative model is [PERMISSION-MODEL.md](PERMISSION-MODEL.md), which is a document and is not registered. |
| Execution Tracking | Candidate operational capability | An observable reporting procedure, testable, and other skills rely on its vocabulary. |
| Fallback and Error Management | Registered skill (draft, 0.1.0, registered in Phase 1 / WP-1.4) | It passes all four criteria: an observable procedure after something fails, testable, an independent lifecycle, and other capabilities refer to it for the choice of an alternative. It has no normative companion document. The rules of [PERMISSION-MODEL.md](PERMISSION-MODEL.md) for causes, retries, and remediation stay where they are and are cited. |
| Task Understanding and Planning | Candidate operational capability | An observable procedure at the start of work, and testable. |
| Cost Controller | Candidate operational capability. **Not committed.** | The procedure is observable and testable, but the criterion that another skill depends on it is weak. Its final classification depends on whether an independent procedure and lifecycle are demonstrated when it is specified. It is not forced into the design to reach a number of skills. |
| Regression and Consistency Testing | Provisional. Resolved when it is specified. | The Core area is a behavioral comparison against a verified baseline. Its boundary with repository-level testing (§3.1) is settled at that point, so that the two do not duplicate each other. |
| Capability Registry and Routing | Normative contract (§4.2) | It is a contract. Treating it as a procedure risks an orchestration layer. It is revisited as an operational capability only if a later phase shows a standalone procedure that passes the test in §2. |
| Conflict Resolution | Normative (§4.1 and `core/README.md` §5) | The rules exist and are normative. It has no independent lifecycle or dependents. |

## 6. How a skill refers to a standard

* A skill cites the rule it relies on by document, section, and relative link. It does not copy the text. A one-line restatement with the link is the only exception. Copying normative text is a defect because the copies drift apart.
* The frontmatter field `dependencies` may name a normative document, as Quality Guardian does with "Core operating standards".
* The registry field `dependencies` lists only registered skill ids (validator check KZ-R004). A Core skill normally uses `dependencies: []`.
* Cooperation between areas is written in the skill's "Boundaries and interactions" section as a soft interaction (§4, item 6). A registry dependency is added only for a genuine hard dependency, and not to represent conceptual cooperation. This also keeps the dependency graph free of cycles (KZ-R005).
* No schema or registry field is added for any of this.

## 7. Scope boundaries

This design and the Phase 1 work that follows it do not include:

* Skill Discovery & Acquisition, which is deferred;
* commercial licensing, subscriptions, or multi-customer deployment;
* customer-specific environments or cross-customer shared learning;
* unified multi-agent orchestration, or any orchestration layer;
* proactive or background agents;
* a personal AI profile;
* merging assistants' identities or personalities, or defining any;
* platform-specific mechanisms, which belong in adapters (roadmap Phase 4);
* rebuilding the validator, the schemas, the tests, the workflow, or the evidence model.

## 8. Room for later work

Nothing in this section is implemented, enforced, or required. It records how the design leaves room so that later work does not need to replace it.

| A later need | How the design leaves room |
|---|---|
| Several assistants using the same capabilities | Skills are written without identity or platform assumptions, and handoffs are documented inputs and outputs. |
| Orchestration across capabilities | The routing contract (§4.2) is declarative, so a later system could read it. This design adds no orchestrator. |
| Shared learning and verified knowledge | The evidence model (`docs/EVIDENCE-MODEL.md` §11) holds the provenance and verification fields. Core adds no learning mechanism. |
| Proactive or background operation | Activation conditions in each specification state what triggers a capability. This design implements no trigger. |
| Personal profile or customer-specific context | Public repository content stays free of private information (`docs/EVIDENCE-MODEL.md` §8). Nothing is implemented here. |
| New platforms | A platform is added through an adapter and a compatibility declaration. Core text does not change. |

## 9. Open items

* Cost Controller: final classification when it is specified.
* Regression and Consistency Testing: final classification and boundary when it is specified.
* Capability Registry and Routing: revisit only if a later phase shows a standalone procedure.
* Quality Guardian: removal of the overlap with Privacy and Security, in a later version.

Specifications of the areas follow [SPEC-TEMPLATE.md](SPEC-TEMPLATE.md).
