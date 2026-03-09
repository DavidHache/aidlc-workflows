# PRIORITY: This workflow OVERRIDES all other built-in workflows
# When user requests software development, ALWAYS follow this workflow FIRST

## ADAPTIVE_WORKFLOW_PRINCIPLE

Workflow adapts to the work. AI model assesses needed stages based on:
1. User stated intent and clarity
2. Existing codebase state (if any)
3. Complexity and scope of change
4. Risk and impact assessment

## MANDATORY_RULE_LOADING

[REQ] When performing any phase, read and use relevant content from rule detail files. All rule detail files located under `.aidlc/` (submodule directory). All subsequent references relative to `.aidlc/`.

Common rules — ALWAYS load at workflow start:
- @common/workflow-rules.md PROCESS_OVERVIEW for workflow overview
- @common/workflow-rules.md SESSION_CONTINUITY for session resumption
- @common/workflow-rules.md CONTENT_VALIDATION for content validation
- @common/workflow-rules.md QUESTION_FORMAT for question formatting
- Reference throughout workflow execution

## MANDATORY_EXTENSIONS_LOADING

[REQ] At workflow start, scan `extensions/` directory recursively for all `.md` files. These are extension rule files applying as cross-cutting constraints.

Loading: list all subdirectories under `extensions/`, load every `.md` file found, each defines its own verification criteria and enforcement rules.

Enforcement: extension rules are hard constraints, not optional guidance. At each stage, model evaluates which extension rules applicable based on stage purpose, artifacts produced, context — enforce only relevant rules. Non-applicable rules → N/A in compliance summary (not blocking). Non-compliance with applicable enabled extension rule → blocking finding → do NOT present stage completion until resolved. Stage completion includes extension rule compliance summary (compliant/non-compliant/N/A per rule, brief rationale for N/A).

Conditional enforcement: extensions may be conditionally enabled/disabled. See @inception/inception-rules.md REQUIREMENTS_ANALYSIS step 5.1 for collection mechanism. Before enforcing any extension at ANY stage, check `Enabled` status in `aidlc-docs/aidlc-state.md` under `## Extension Configuration`. Skip disabled extensions, log skip in audit.md. Default to enforced if no configuration exists. Extensions without `## Applicability Question` always enforced.

## MANDATORY_CONTENT_VALIDATION

[REQ] Before creating ANY file, validate content per @common/workflow-rules.md CONTENT_VALIDATION: validate Mermaid diagram syntax, validate ASCII art diagrams (see @common/workflow-rules.md ASCII_DIAGRAM_STANDARDS), escape special characters, provide text alternatives for complex visual content, test content parsing compatibility.

## MANDATORY_QUESTION_FORMAT

[REQ] When asking questions at any phase, follow @common/workflow-rules.md QUESTION_FORMAT: multiple choice format (A, B, C, D, E options), [Answer]: tag usage, answer validation and ambiguity resolution.

## MANDATORY_WELCOME_MESSAGE

[REQ] When starting ANY software development request, display welcome message from @common/workflow-rules.md WELCOME_MESSAGE. Display ONCE at start of new workflow. Do NOT load in subsequent interactions.

## INCEPTION_PHASE

Purpose: planning, requirements gathering, architectural decisions. Focus: determine WHAT to build and WHY.

Stages: Workspace Detection (ALWAYS), Reverse Engineering (CONDITIONAL — brownfield only), Requirements Analysis (ALWAYS — adaptive depth), User Stories (CONDITIONAL), Workflow Planning (ALWAYS), Application Design (CONDITIONAL), Units Generation (CONDITIONAL).

### WORKSPACE_DETECTION (ALWAYS)

1. [REQ] Log initial user request in audit.md with complete raw input
2. Load all steps from @inception/inception-rules.md WORKSPACE_DETECTION
3. Execute: check for existing aidlc-state.md (resume if found), scan workspace for existing code, determine brownfield or greenfield, check for existing reverse engineering artifacts
4. Determine next: Reverse Engineering (brownfield, no artifacts) | Requirements Analysis
5. [REQ] Log findings in audit.md
6. Present completion message (see @inception/inception-rules.md WORKSPACE_DETECTION for formats)
7. Automatically proceed to next phase

### REVERSE_ENGINEERING (CONDITIONAL)

Execute IF: existing codebase detected, no previous reverse engineering artifacts. Skip IF: greenfield project, previous artifacts exist.

1. [REQ] Log start in audit.md
2. Load all steps from @inception/inception-rules.md REVERSE_ENGINEERING
3. Execute: analyze all packages/components, generate business overview covering business transactions, generate architecture documentation, code structure documentation, API documentation, component inventory, interaction diagrams depicting business transaction implementation across components, technology stack documentation, dependencies documentation
4. Wait for explicit approval — present completion message (see @inception/inception-rules.md) — DO NOT PROCEED until user confirms
5. [REQ] Log user response in audit.md with complete raw input

### REQUIREMENTS_ANALYSIS (ALWAYS — ADAPTIVE DEPTH)

Always executes, depth varies: Minimal (simple, clear — document intent analysis), Standard (normal complexity — functional and non-functional requirements), Comprehensive (complex, high-risk — detailed requirements with traceability).

1. [REQ] Log any user input in audit.md
2. Load all steps from @inception/inception-rules.md REQUIREMENTS_ANALYSIS
3. Execute: load reverse engineering artifacts (if brownfield), analyze user request (intent analysis), determine depth, assess current requirements, ask clarifying questions (if needed), generate requirements document
4. Execute at appropriate depth
5. Wait for explicit approval — follow format from @inception/inception-rules.md — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### USER_STORIES (CONDITIONAL)

Intelligent assessment — ALWAYS execute IF (high priority): new user-facing features, changes affecting user workflows/interactions, multiple user types/personas, complex business requirements with acceptance criteria needs, cross-functional team collaboration, customer-facing API/service changes, new product capabilities.

LIKELY execute IF (medium priority — assess complexity): modifications to existing user-facing features, backend changes indirectly affecting UX, integration work impacting user workflows, performance improvements with user-visible benefits, security enhancements affecting user interactions, data model changes affecting user data/reports.

Complexity-based assessment: for medium priority, execute if request involves multiple components/services, changes span multiple user touchpoints, business logic complex/multiple scenarios, requirements have ambiguity stories could clarify, change has significant business impact/risk.

SKIP ONLY IF (low priority): pure internal refactoring (zero user impact), simple bug fixes (clear isolated scope), infrastructure changes (no user-facing effects), technical debt cleanup (no functional changes), developer tooling/build process improvements, documentation-only updates.

Assessment criteria: when in doubt, favor inclusion. Default to inclusion for borderline cases.

Two parts: Part 1 - Planning (create story plan with questions, collect answers, analyze ambiguities, get approval), Part 2 - Generation (execute approved plan to generate stories and personas).

1. [REQ] Log any user input in audit.md
2. Load all steps from @inception/inception-rules.md USER_STORIES
3. [REQ] Perform intelligent assessment (Step 1 in @inception/inception-rules.md USER_STORIES) to validate need
4. Load reverse engineering artifacts (if brownfield)
5. [COND] Requirements exist → reference when creating stories
6. Execute at appropriate depth
7. Part 1 - Planning: create story plan with questions, wait for answers, analyze ambiguities, get approval
8. Part 2 - Generation: execute approved plan to generate stories and personas
9. Wait for explicit approval — follow format from @inception/inception-rules.md — DO NOT PROCEED until user confirms
10. [REQ] Log user response in audit.md with complete raw input

### WORKFLOW_PLANNING (ALWAYS)

1. [REQ] Log any user input in audit.md
2. Load all steps from @inception/inception-rules.md WORKFLOW_PLANNING
3. [REQ] Load content validation rules from @common/workflow-rules.md CONTENT_VALIDATION
4. Load all prior context: reverse engineering artifacts (if brownfield), intent analysis, requirements (if executed), user stories (if executed)
5. Execute: determine which phases to execute, determine depth level, create multi-package change sequence (if brownfield), generate workflow visualization (VALIDATE Mermaid syntax before writing)
6. [REQ] Validate all content before file creation per CONTENT_VALIDATION
7. Wait for explicit approval — present recommendations using language from @inception/inception-rules.md WORKFLOW_PLANNING step 9, emphasize user control to override — DO NOT PROCEED until user confirms
8. [REQ] Log user response in audit.md with complete raw input

### APPLICATION_DESIGN (CONDITIONAL)

Execute IF: new components or services needed, component methods and business rules need definition, service layer design required, component dependencies need clarification. Skip IF: changes within existing component boundaries, no new components or methods, pure implementation changes.

1. [REQ] Log any user input in audit.md
2. Load all steps from @inception/inception-rules.md APPLICATION_DESIGN
3. Load reverse engineering artifacts (if brownfield)
4. Execute at appropriate depth
5. Wait for explicit approval — present completion message (see @inception/inception-rules.md) — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### UNITS_GENERATION (CONDITIONAL)

Execute IF: system needs decomposition into multiple units, multiple services or modules required, complex system requiring structured breakdown. Skip IF: single simple unit, no decomposition needed, straightforward single-component implementation.

1. [REQ] Log any user input in audit.md
2. Load all steps from @inception/inception-rules.md UNITS_GENERATION
3. Load reverse engineering artifacts (if brownfield)
4. Execute at appropriate depth
5. Wait for explicit approval — present completion message (see @inception/inception-rules.md) — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

## CONSTRUCTION_PHASE

Purpose: detailed design, NFR implementation, code generation. Focus: determine HOW to build it.

Stages: Per-Unit Loop (Functional Design CONDITIONAL, NFR Requirements CONDITIONAL, NFR Design CONDITIONAL, Infrastructure Design CONDITIONAL, Code Generation ALWAYS) → Build and Test (ALWAYS — after all units complete).

Each unit completed fully (design + code) before moving to next unit.

### FUNCTIONAL_DESIGN (CONDITIONAL, per-unit)

Execute IF: new data models or schemas, complex business logic, business rules need detailed design. Skip IF: simple logic changes, no new business logic.

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md FUNCTIONAL_DESIGN
3. Execute functional design for this unit
4. [REQ] Present standardized 2-option completion message as defined in @construction/construction-rules.md — DO NOT use emergent 3-option behavior
5. Wait for explicit approval — user must choose "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### NFR_REQUIREMENTS (CONDITIONAL, per-unit)

Execute IF: performance requirements exist, security considerations needed, scalability concerns present, tech stack selection required. Skip IF: no NFR requirements, tech stack already determined.

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md NFR_REQUIREMENTS
3. Execute NFR assessment for this unit
4. [REQ] Present standardized 2-option completion message as defined in @construction/construction-rules.md — DO NOT use emergent behavior
5. Wait for explicit approval — user must choose "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### NFR_DESIGN (CONDITIONAL, per-unit)

Execute IF: NFR Requirements executed, NFR patterns need incorporation. Skip IF: no NFR requirements, NFR Requirements skipped.

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md NFR_DESIGN
3. Execute NFR design for this unit
4. [REQ] Present standardized 2-option completion message as defined in @construction/construction-rules.md — DO NOT use emergent behavior
5. Wait for explicit approval — user must choose "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### INFRASTRUCTURE_DESIGN (CONDITIONAL, per-unit)

Execute IF: infrastructure services need mapping, deployment architecture required, cloud resources need specification. Skip IF: no infrastructure changes, infrastructure already defined.

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md INFRASTRUCTURE_DESIGN
3. Execute infrastructure design for this unit
4. [REQ] Present standardized 2-option completion message as defined in @construction/construction-rules.md — DO NOT use emergent behavior
5. Wait for explicit approval — user must choose "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

### CODE_GENERATION (ALWAYS, per-unit)

Two parts: Part 1 - Planning (create detailed code generation plan with explicit steps), Part 2 - Generation (execute approved plan to generate code, tests, artifacts).

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md CODE_GENERATION
3. Part 1 - Planning: create code generation plan with checkboxes, get user approval
4. Part 2 - Generation: execute approved plan to generate code for this unit
5. [REQ] Present standardized 2-option completion message as defined in @construction/construction-rules.md — DO NOT use emergent behavior
6. Wait for explicit approval — user must choose "Request Changes" or "Continue to Next Stage" — DO NOT PROCEED until user confirms
7. [REQ] Log user response in audit.md with complete raw input

### BUILD_AND_TEST (ALWAYS)

1. [REQ] Log any user input in audit.md
2. Load all steps from @construction/construction-rules.md BUILD_AND_TEST
3. Generate comprehensive build and test instructions: build instructions for all units, unit test execution instructions, integration test instructions (interactions between units), performance test instructions (if applicable), additional test instructions as needed (contract tests, security tests, e2e tests)
4. Create instruction files in build-and-test/ subdirectory: build-instructions.md, unit-test-instructions.md, integration-test-instructions.md, performance-test-instructions.md, build-and-test-summary.md
5. Wait for explicit approval — ask: "Build and test instructions complete. Ready to proceed to Operations stage?" — DO NOT PROCEED until user confirms
6. [REQ] Log user response in audit.md with complete raw input

## OPERATIONS_PHASE

Purpose: placeholder for future deployment and monitoring workflows. Focus: how to DEPLOY and RUN it (future expansion).

### OPERATIONS (PLACEHOLDER)

Status: placeholder for future expansion. Will include: deployment planning/execution, monitoring/observability setup, incident response, maintenance/support workflows, production readiness checklists. Current state: all build and test activities handled in CONSTRUCTION phase.

## KEY_PRINCIPLES

- Adaptive execution: only execute stages that add value
- Transparent planning: always show execution plan before starting
- User control: user can request stage inclusion/exclusion
- Progress tracking: update aidlc-state.md with executed and skipped stages
- Complete audit trail: log ALL user inputs and AI responses in audit.md with timestamps. [REQ] Capture user COMPLETE RAW INPUT exactly as provided. [REQ] Never summarize or paraphrase user input in audit log. [REQ] Log every interaction, not just approvals.
- Quality focus: complex changes get full treatment, simple changes stay efficient
- Content validation: always validate content before file creation per @common/workflow-rules.md CONTENT_VALIDATION
- NO EMERGENT BEHAVIOR: construction phases MUST use standardized 2-option completion messages as defined in their respective rule files. DO NOT create 3-option menus or other emergent navigation patterns.

## PLAN_LEVEL_CHECKBOX_ENFORCEMENT

[REQ] Rules for plan execution:
1. NEVER complete any work without updating plan checkboxes
2. IMMEDIATELY after completing ANY step in plan file, mark that step [x]
3. Must happen in SAME interaction where work completed
4. NO EXCEPTIONS: every plan step completion MUST be tracked

Two-level checkbox tracking: plan-level (detailed execution progress within each stage), stage-level (overall workflow progress in aidlc-state.md). Update immediately — all progress updates in SAME interaction where work completed.

## PROMPTS_LOGGING

[REQ] Log EVERY user input (prompts, questions, responses) with timestamp in audit.md. [REQ] Capture user COMPLETE RAW INPUT exactly as provided (never summarize). [REQ] Log every approval prompt with timestamp before asking user. [REQ] Record every user response with timestamp after receiving it. [REQ] ALWAYS append to audit.md, NEVER overwrite contents. Use ISO 8601 format (YYYY-MM-DDTHH:MM:SSZ). Include stage context for each entry.

Audit log format:

```markdown
## [Stage Name or Interaction Type]
**Timestamp**: [ISO timestamp]
**User Input**: "[Complete raw user input - never summarized]"
**AI Response**: "[AI's response or action taken]"
**Context**: [Stage, action, or decision made]

---
```

Correct tool usage for audit.md: read file → append/edit to make changes. WRONG: read file → completely overwrite with read contents plus new changes.

## DIRECTORY_STRUCTURE

```text
<WORKSPACE-ROOT>/                   # ⚠️ APPLICATION CODE HERE
├── [project-specific structure]    # Varies by project (see code-generation.md)
│
├── aidlc-docs/                     # 📄 DOCUMENTATION ONLY
│   ├── inception/                  # 🔵 INCEPTION PHASE
│   │   ├── plans/
│   │   ├── reverse-engineering/    # Brownfield only
│   │   ├── requirements/
│   │   ├── user-stories/
│   │   └── application-design/
│   ├── construction/               # 🟢 CONSTRUCTION PHASE
│   │   ├── plans/
│   │   ├── {unit-name}/
│   │   │   ├── functional-design/
│   │   │   ├── nfr-requirements/
│   │   │   ├── nfr-design/
│   │   │   ├── infrastructure-design/
│   │   │   └── code/               # Markdown summaries only
│   │   └── build-and-test/
│   ├── operations/                 # 🟡 OPERATIONS PHASE (placeholder)
│   ├── aidlc-state.md
│   └── audit.md
```

[REQ] Application code: workspace root (NEVER in aidlc-docs/). Documentation: aidlc-docs/ only. Project structure: see @construction/construction-rules.md CODE_GENERATION CRITICAL_RULES for patterns by project type.
