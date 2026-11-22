# Core Specification Template

## 1. Purpose

This is the minimum structure for the specification of a Core operational capability. It refines `docs/SKILL-STANDARD.md` §4 and §5 for the Core layer. It does not replace them, and it does not change the frontmatter.

It applies only to an area that passes the registration test in [DESIGN.md](DESIGN.md) §2. An area that is a normative standard is written as a document, not with this template.

This file is a template. It is not a skill and is not registered.

## 2. Rules

* The frontmatter has the 14 fields of `docs/SKILL-STANDARD.md` §4, unchanged. The field shapes are defined by `schemas/skill-frontmatter.schema.json`.
* The body has the nine sections of §4. A length of about 150 to 250 lines is a guideline. It is not a requirement, and no check enforces it.
* The specification states its own procedure. It cites normative rules by section and link and does not copy them ([DESIGN.md](DESIGN.md) §6).
* Cooperation with other areas is written as soft interactions. `dependencies` in the registry normally stays `[]`.
* The specification contains no identity, persona, voice, or platform-specific mechanism, and makes no claim of installation, activation, or compatibility.
* A new specification has the status `draft` and declares `behavioral_scenario_evaluation` and `repository_security_scan` as `"not_run"`.
* Scenario specifications go in a sibling `tests/test-cases.md`. No placeholder `test-results.md` is created. An evidence record is created only when a scenario is actually run (`docs/EVIDENCE-MODEL.md`).

## 3. Frontmatter skeleton

```yaml
---
id: <kebab-case id, equal to the directory name>
name: <human-readable name>
type: core
version: 0.1.0
status: draft
purpose: <the one question this capability owns, from DESIGN.md section 3>
scope:
  - <responsibility covered>
non_goals:
  - <responsibility excluded, with the owner named in the body>
inputs:
  - <input>
outputs:
  - <output>
dependencies:
  - <normative document, if any; registered skill ids only if a hard dependency exists>
platform_requirements:
  - <abstract capability needed, never a named platform feature>
safety:
  - <safety or privacy requirement>
verification:
  - <how correct execution is judged>
---
```

## 4. Body skeleton

| # | Section | Content |
|---|---|---|
| 1 | Role and purpose | The question this area owns ([DESIGN.md](DESIGN.md) §3), in two or three sentences. |
| 2 | Activation | When the capability applies and when it does not. |
| 3 | Boundaries and interactions | What it owns, what it does not own and who does, the soft inputs it may request, how it behaves when other areas are absent, and its position in the precedence model ([DESIGN.md](DESIGN.md) §4). |
| 4 | Procedure | Numbered steps and decision rules. |
| 5 | Inputs and outputs | The shape of the output and the honest-reporting rules. |
| 6 | Failure and fallback | What it does when its own steps fail or cannot be completed. |
| 7 | Safety and privacy | Specific notes, citing Privacy and Security by link once it exists. |
| 8 | Verification criteria | Measurable statements of what correct execution looks like. |
| 9 | Current status | The status and version, with no claim beyond the evidence. |

## 5. Test specification skeleton

`tests/test-cases.md` lists scenarios. Each scenario has:

```text
id:        <prefix>-NNN, for example PS-001
input:     <the scenario, with synthetic data only>
expected:  <the behavior that is expected>
pass:      <the observable criteria that decide pass or fail>
```

Scenarios use synthetic data. They contain no real credentials and no private user information.

## 6. Review checklist

* The question in section 1 matches the matrix in [DESIGN.md](DESIGN.md) §3.
* The boundaries name an owner for everything the capability does not own.
* No normative text is copied. Rules are cited by section and link.
* The capability works when the other areas are absent.
* No identity, persona, or platform-specific assumption appears.
* No claim of installation, activation, compatibility, validation, or release appears.
* The frontmatter passes the validator, and the registry entry (if registered) has `dependencies: []` unless a hard dependency is justified.
* Scenario data is synthetic.
