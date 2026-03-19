# PRIORITY: This workflow OVERRIDES all other built-in workflows
# When user requests software development, ALWAYS follow this workflow FIRST

PATTERN:STAGE_EXEC(rulefile) = [REQ] log user input in audit.md → load @{rulefile} steps → execute → [REQ] present standardized 2-option completion per rule file (NO emergent behavior) → wait for explicit approval (DO NOT proceed until confirmed) → [REQ] log response in audit.md

## Adaptive Workflow Principle

Workflow adapts to work based on: user intent + clarity, codebase state, complexity + scope, risk + impact.

## MANDATORY: Rule Details Loading

[REQ] When performing any phase, read + use relevant content from rule detail files. Check paths in order, use first that exists:
- `.aidlc-rule-details/` (Cursor, Cline, Claude Code, GitHub Copilot)
- `.kiro/aws-aidlc-rule-details/` (Kiro IDE and CLI)
- `.amazonq/aws-aidlc-rule-details/` (Amazon Q Developer)

All rule detail references relative to resolved directory.

Common rules: ALWAYS load at workflow start:
- @common/workflow-rules.md (process overview, session continuity, content validation, question format, error handling, workflow changes, overconfidence prevention, terminology, depth levels, welcome message, ASCII diagram standards)

## MANDATORY: Extensions Loading (Context-Optimized)

[REQ] At workflow start, scan `extensions/` recursively; load ONLY `*.opt-in.md` files (NOT full rule files).

Loading: list subdirs under `extensions/`; in each, load only `*.opt-in.md`; derive rules file by convention (strip `.opt-in.md`, append `.md`); do NOT load full rules at this stage.

Deferred loading: during Requirements Analysis, present opt-in prompts; user opts IN → load corresponding rules file; user opts OUT → never load. Extensions without `*.opt-in.md` → always enforced, load immediately.

Enforcement (loaded/enabled extensions only): hard constraints, not guidance; model evaluates applicability per stage (purpose, artifacts, context); N/A rules → mark N/A in compliance summary (not blocking); non-compliance with applicable rule → blocking finding (do NOT present completion until resolved); include compliance summary in stage completion.

Conditional enforcement: check `Enabled` status in `aidlc-docs/aidlc-state.md` under `## Extension Configuration` before enforcing at ANY stage; skip disabled; log skip in audit.md; default to enforced if no config exists.

## MANDATORY: Content Validation

[REQ] Before creating ANY file: validate per @common/workflow-rules.md CONTENT_VALIDATION rules.

## MANDATORY: Question File Format

[REQ] See @common/workflow-rules.md QUESTION_FORMAT for complete rules.

## MANDATORY: Custom Welcome Message

[REQ] At start of ANY software development request: load + display @common/workflow-rules.md WELCOME_MESSAGE. Once only; do NOT reload in subsequent interactions.


---

# INCEPTION PHASE

Purpose: planning, requirements, architectural decisions. Focus: WHAT + WHY.

Stages: Workspace Detection (ALWAYS), Reverse Engineering (COND brownfield), Requirements Analysis (ALWAYS adaptive), User Stories (COND), Workflow Planning (ALWAYS), Application Design (COND), Units Generation (COND)

---

## Workspace Detection (ALWAYS)

1. [REQ] Log initial user request in audit.md with complete raw input
2. Load @inception/inception-rules.md WORKSPACE_DETECTION steps
3. Execute: check aidlc-state.md (resume if found); scan workspace; determine brownfield/greenfield; check for RE artifacts
4. Next: Reverse Engineering (brownfield, no artifacts) | Requirements Analysis
5. [REQ] Log findings in audit.md
6. Present completion (see inception-rules.md)
7. Auto-proceed

## Reverse Engineering (COND brownfield)

Execute if: existing codebase, no previous RE artifacts
Skip if: greenfield | RE artifacts exist

1. [REQ] Log start in audit.md
2. Load @inception/inception-rules.md REVERSE_ENGINEERING steps
3. Execute: analyze all packages/components; generate business overview + architecture + code structure + API docs + component inventory + interaction diagrams + technology stack + dependencies
4. [REQ] Present completion (see inception-rules.md); DO NOT proceed until user confirms
5. [REQ] Log user response in audit.md

## Requirements Analysis (ALWAYS adaptive)

Depth: Minimal (simple, clear) | Standard (normal) | Comprehensive (complex, high-risk)

1. [REQ] Log user input in audit.md
2. Load @inception/inception-rules.md REQUIREMENTS_ANALYSIS steps
3. Execute: load RE artifacts (if brownfield); analyze request; determine depth; assess requirements; ask questions (if needed); generate requirements doc
4. Execute at appropriate depth
5. [REQ] Follow approval format from inception-rules.md; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

## User Stories (COND)

ALWAYS execute if: new user-facing features, UX changes, multi-persona, complex business requirements, cross-team, customer-facing API
LIKELY execute if: existing feature modifications, backend user impact, integration affecting workflows, performance with visible benefits, security affecting users, data model affecting user data
SKIP only if: pure refactoring (zero user impact), isolated bug fixes, infrastructure only, dev tooling, documentation only
Default: when in doubt, include + ask clarifying questions

Two parts: Part 1 Planning, Part 2 Generation.

1. [REQ] Log user input in audit.md
2. Load @inception/inception-rules.md USER_STORIES steps
3. [REQ] Perform intelligent assessment (validate stories needed)
4. Load RE artifacts (if brownfield); reference requirements if available
5. Execute at appropriate depth
6. Part 1: create story plan with questions → wait for answers → analyze ambiguities → get approval
7. Part 2: execute approved plan → generate stories + personas
8. [REQ] Follow approval format from inception-rules.md; DO NOT proceed until confirmed
9. [REQ] Log response in audit.md

## Workflow Planning (ALWAYS)

1. [REQ] Log user input in audit.md
2. Load @inception/inception-rules.md WORKFLOW_PLANNING steps
3. [REQ] Load @common/workflow-rules.md CONTENT_VALIDATION
4. Load all prior context: RE artifacts (if brownfield), intent analysis, requirements (if executed), stories (if executed)
5. Execute: determine phases, depth levels, multi-package sequence (if brownfield), generate workflow visualization (validate Mermaid before writing)
6. [REQ] Validate all content before file creation
7. [REQ] Present recommendations emphasizing user control to override; DO NOT proceed until confirmed
8. [REQ] Log response in audit.md

## Application Design (COND)

Execute if: new components/services, methods/business rules need definition, service layer design, dependency clarification
Skip if: within existing boundaries, no new components, pure implementation

1. [REQ] Log user input in audit.md
2. Load @inception/inception-rules.md APPLICATION_DESIGN steps
3. Load RE artifacts (if brownfield)
4. Execute at appropriate depth
5. [REQ] Present completion (see inception-rules.md); DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

## Units Generation (COND)

Execute if: system needs decomposition, multiple services/modules, complex structured breakdown
Skip if: single simple unit, no decomposition, straightforward single-component

1. [REQ] Log user input in audit.md
2. Load @inception/inception-rules.md UNITS_GENERATION steps
3. Load RE artifacts (if brownfield)
4. Execute at appropriate depth
5. [REQ] Present completion (see inception-rules.md); DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

---

# CONSTRUCTION PHASE

Purpose: detailed design, NFR implementation, code generation. Focus: HOW.

Stages: Per-Unit Loop (Functional Design COND, NFR Requirements COND, NFR Design COND, Infrastructure Design COND, Code Generation ALWAYS) → Build and Test (ALWAYS after all units)

Each unit completed fully (design + code) before next unit.

---

## Per-Unit Loop

#### Functional Design (COND per-unit)
Execute if: new data models/schemas, complex business logic, detailed business rules
Skip if: simple logic, no new business logic
[PATTERN:STAGE_EXEC(construction/construction-rules.md FUNCTIONAL_DESIGN)]

#### NFR Requirements (COND per-unit)
Execute if: performance, security, scalability requirements, tech stack selection needed
Skip if: no NFR requirements, tech stack determined
[PATTERN:STAGE_EXEC(construction/construction-rules.md NFR_REQUIREMENTS)]

#### NFR Design (COND per-unit)
Execute if: NFR Requirements executed, NFR patterns need incorporation
Skip if: no NFR requirements, NFR Requirements skipped
[PATTERN:STAGE_EXEC(construction/construction-rules.md NFR_DESIGN)]

#### Infrastructure Design (COND per-unit)
Execute if: infrastructure mapping needed, deployment architecture required, cloud resources need spec
Skip if: no infrastructure changes, infrastructure defined
[PATTERN:STAGE_EXEC(construction/construction-rules.md INFRASTRUCTURE_DESIGN)]

#### Code Generation (ALWAYS per-unit)
Two parts: Part 1 Planning, Part 2 Generation.

1. [REQ] Log user input in audit.md
2. Load @construction/construction-rules.md CODE_GENERATION steps
3. Part 1: create code generation plan with checkboxes → get user approval
4. Part 2: execute approved plan → generate code for unit
5. [REQ] Present standardized 2-option completion (NO emergent behavior)
6. Wait for explicit approval; DO NOT proceed until confirmed
7. [REQ] Log response in audit.md

---

## Build and Test (ALWAYS)

1. [REQ] Log user input in audit.md
2. Load @construction/construction-rules.md BUILD_AND_TEST steps
3. Generate comprehensive instructions: build-instructions.md, unit-test-instructions.md, integration-test-instructions.md, performance-test-instructions.md (if applicable), additional tests as needed, build-and-test-summary.md
4. All files in `aidlc-docs/construction/build-and-test/`
5. Wait for approval: "Build and test instructions complete. Ready to proceed to Operations stage?"; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

---

# OPERATIONS PHASE

Placeholder for future deployment + monitoring workflows.

## Operations (PLACEHOLDER)

Future: deployment planning, monitoring/observability, incident response, maintenance, production readiness.
Current: all build/test in CONSTRUCTION.

---

## Key Principles

- Adaptive execution: only stages that add value
- Transparent planning: show execution plan before starting
- User control: user can request stage inclusion/exclusion
- Progress tracking: update aidlc-state.md
- [REQ] Complete audit trail: log ALL user inputs + AI responses in audit.md with timestamps; capture COMPLETE RAW INPUT (never summarize/paraphrase); log every interaction
- Quality: complex → full treatment; simple → efficient
- Content validation: always validate before file creation
- [REQ] NO EMERGENT BEHAVIOR: construction phases MUST use standardized 2-option completion messages per rule files; DO NOT create 3-option menus or other emergent patterns

## MANDATORY: Plan-Level Checkbox Enforcement

[REQ] NEVER complete work without updating plan checkboxes
[REQ] IMMEDIATELY mark [x] after completing ANY plan step (same interaction)
[REQ] NO EXCEPTIONS

Two-level tracking: plan-level (detailed execution) + stage-level (aidlc-state.md). Update immediately in same interaction.

## Prompts Logging Requirements

[REQ] Log EVERY user input with timestamp in audit.md (never summarize)
[REQ] Log every approval prompt before asking
[REQ] Record every response after receiving
[REQ] ALWAYS append to audit.md, NEVER overwrite entire file
ISO 8601 timestamps. Include stage context.

Audit log format:
```markdown
## [Stage Name or Interaction Type]
**Timestamp**: [ISO timestamp]
**User Input**: "[Complete raw user input - never summarized]"
**AI Response**: "[AI's response or action taken]"
**Context**: [Stage, action, or decision made]

---
```

Correct: read audit.md → append/edit. WRONG: read → overwrite entire file.

## Directory Structure

```text
<WORKSPACE-ROOT>/                   # APPLICATION CODE HERE
├── [project-specific structure]    # Varies by project (see code-generation.md)
│
├── aidlc-docs/                     # DOCUMENTATION ONLY
│   ├── inception/                  # INCEPTION PHASE
│   │   ├── plans/
│   │   ├── reverse-engineering/    # Brownfield only
│   │   ├── requirements/
│   │   ├── user-stories/
│   │   └── application-design/
│   ├── construction/               # CONSTRUCTION PHASE
│   │   ├── plans/
│   │   ├── {unit-name}/
│   │   │   ├── functional-design/
│   │   │   ├── nfr-requirements/
│   │   │   ├── nfr-design/
│   │   │   ├── infrastructure-design/
│   │   │   └── code/               # Markdown summaries only
│   │   └── build-and-test/
│   ├── operations/                 # OPERATIONS PHASE (placeholder)
│   ├── aidlc-state.md
│   └── audit.md
```

[REQ] App code → workspace root (NEVER aidlc-docs/). Documentation → aidlc-docs/ only. See @construction/construction-rules.md CODE_GENERATION Critical Rules for patterns.