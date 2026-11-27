# Core Operating Standards

## 1. Purpose

The Core layer defines the foundational operating standards and cross-domain capabilities used throughout KARAZ AI ECOSYSTEM.

Its purpose is to establish consistent quality, safety, execution discipline, and interoperability without imposing a shared identity or personality on participating AI assistants.

Core capabilities are reusable standards. They are not a replacement for specialist expertise.

## 2. Architectural Position

The Core layer supports the entire ecosystem:

* Meta-Skills reference Core standards when coordinating domain capabilities.
* Specialist Skills follow applicable Core requirements while retaining their specialized workflows.
* Platform Adapters explain how Core standards can be applied within each platform’s actual capabilities.
* The Registry records relevant capabilities, dependencies, and lifecycle states.

## 3. Core Capability Areas

The following ten capability areas define the planned Core scope. This is the authoritative list; `docs/DEVELOPMENT-ROADMAP.md` Phase 1 follows it.

### 3.1 Quality Guardian

Establishes standards for accuracy, evidence, completeness, verification, and deliverable quality.

### 3.2 Cost Controller

Promotes efficient use of tools, APIs, subscriptions, time, and other resources while protecting output quality.

### 3.3 Privacy and Security

Defines rules for sensitive information, credentials, external content, permissions, and safe handling of data.

### 3.4 Task Understanding and Planning

Establishes methods for interpreting requests, identifying constraints, defining outcomes, and planning execution.

### 3.5 Tool and Permission Discipline

Ensures that tools are selected appropriately, permissions are respected, and unsupported actions are never claimed.

### 3.6 Fallback and Error Management

Defines how to handle unavailable tools, failed operations, incomplete information, and alternative execution paths.

### 3.7 Execution Tracking

Distinguishes planned, attempted, completed, verified, and failed work.

### 3.8 Capability Registry and Routing

Defines how capabilities are discovered, selected, and handed off according to their documented responsibilities and actual availability.

### 3.9 Conflict Resolution

Provides a method for identifying and resolving conflicting instructions, overlapping responsibilities, and incompatible requirements.

### 3.10 Regression and Consistency Testing

Establishes methods for checking that changes do not silently break previously verified behavior.

These capability areas are planned components. Their individual specifications and validation status must be maintained separately.

The ten areas are areas of responsibility. They are not ten registered skills. [DESIGN.md](DESIGN.md) sets out the responsibility of each area, how the areas relate, and how each is classified as a normative standard, an operational capability, or a registered skill.

## 4. Responsibility Boundaries

Core standards must:

* Define reusable operating requirements.
* Avoid duplicating specialist domain knowledge.
* Avoid embedding platform-specific assumptions into generic standards.
* Avoid defining the identity, personality, or communication style of any AI assistant.
* Avoid claiming capabilities that a platform or assistant does not actually support.

Meta-Skills and Specialist Skills may extend Core standards with domain-specific requirements, provided they do not contradict mandatory security and quality rules.

## 5. Instruction and Conflict Principles

When requirements conflict:

1. Applicable platform policies and safety requirements remain binding.
2. Security and privacy requirements must not be bypassed.
3. Explicit task constraints must be respected when compatible with higher-priority requirements.
4. Specialized instructions govern their own domain within their documented scope.
5. Unresolved conflicts must be disclosed rather than silently ignored.

The repository provides standards and guidance; each AI platform remains responsible for applying them within its own instruction hierarchy and capabilities.

## 6. Implementation Rules

Every Core capability must have:

* A unique identifier.
* A clearly defined purpose.
* Explicit scope and non-goals.
* Inputs and expected outputs where applicable.
* Dependencies and interaction boundaries.
* Operational procedures.
* Failure and fallback behavior where relevant.
* Verification criteria.
* Version and lifecycle status.

## 7. Planned Structure

The Core layer is expected to grow into independently documented components, for example:

* quality-guardian/
* cost-controller/
* privacy-and-security/
* task-planning/
* tool-discipline/
* fallback-manager/
* execution-tracker/
* capability-routing/
* conflict-resolver/
* regression-testing/

These directories should be created only when their actual documentation and implementation are ready. The list names candidate directories. A directory is created only for an area that is classified as an operational capability and registered as a skill ([DESIGN.md](DESIGN.md) §5). An area classified as a normative standard is documented in [DESIGN.md](DESIGN.md) and is not given a directory. Specifications follow [SPEC-TEMPLATE.md](SPEC-TEMPLATE.md).

## 8. Current Status

The Core layer has entered its specification phase.

No individual Core capability should be considered implemented, validated, or released merely because it appears in this document.

Quality Guardian (`quality-guardian/`) has a draft specification, version 0.3.0, with scenario-evaluation records. It is not validated or released. See `registry/skills.yaml`. Privacy and Security (`privacy-and-security/`) has a draft specification, version 0.1.0, with scenario test specifications that have not been executed. It is not validated or released. Tool and Permission Discipline (`tool-discipline/`, with its normative model in `PERMISSION-MODEL.md`) has a draft specification, version 0.1.0, with scenario test specifications that have not been executed. It is not validated or released. Fallback and Error Management (`fallback-manager/`) has a draft specification, version 0.1.0, with scenario test specifications that have not been executed. It is not validated or released. Task Understanding and Planning (`task-planning/`) has a draft specification, version 0.1.0, with scenario test specifications that have not been executed. It is not validated or released. Execution Tracking (`execution-tracker/`) has a draft specification, version 0.1.0, with scenario test specifications that have not been executed. It is not validated or released. Cost Controller is a candidate and is not committed, and Regression and Consistency Testing is provisional. Neither has a registered specification. Capability Registry and Routing and Conflict Resolution are normative contracts ([DESIGN.md](DESIGN.md) §4.1 and §4.2) and are not registered skills. Their Phase 1 closeout evaluation is still pending. Phase 1 is not complete.

The Core layer design ([DESIGN.md](DESIGN.md)) and the specification template ([SPEC-TEMPLATE.md](SPEC-TEMPLATE.md)) exist as draft normative documents. They register no skill and change no registry.

Each capability will be developed and reviewed independently.
