# Units Generation - Detailed Steps

## Overview
This stage decomposes the system into manageable units of work through two integrated parts:
- **Part 1 - Planning**: Create decomposition plan with questions, collect answers, analyze for ambiguities, get approval
- **Part 2 - Generation**: Execute approved plan to generate unit artifacts

**DEFINITION**: A unit of work is a logical grouping of stories for development purposes. For microservices, each unit becomes an independently deployable service. For monoliths, the single unit represents the entire application with logical modules.

**Terminology**: Use "Service" for independently deployable components, "Module" for logical groupings within a service, "Unit of Work" for planning context.

## Prerequisites
- Context Assessment must be complete
- Requirements Assessment recommended (provides functional scope)
- Story Development recommended (stories map to units)
- Application Design phase REQUIRED (determines components, methods, and services)
- Execution plan must indicate Design phase should execute

---

# PART 1: PLANNING

## Step 1: Create Unit of Work Plan
- Generate plan with checkboxes [] for decomposing system into units of work
- Focus on breaking down the system into manageable development units
- Each step and sub-step should have a checkbox []

## Step 2: Include Mandatory Unit Artifacts in Plan
**ALWAYS** include these mandatory artifacts in the unit plan:
- [ ] Generate `aidlc-docs/inception/application-design/unit-of-work.md` with unit definitions and responsibilities
- [ ] Generate `aidlc-docs/inception/application-design/unit-of-work-dependency.md` with dependency matrix
- [ ] Generate `aidlc-docs/inception/application-design/unit-of-work-story-map.md` mapping stories to units
- [ ] Generate `aidlc-docs/inception/application-design/unit-of-work-parallel-execution.md` with parallel execution groups (see Parallel Execution Planning below)
- [ ] **Greenfield only**: Document code organization strategy in `unit-of-work.md` (see code-generation.md for structure patterns)
- [ ] Validate unit boundaries and dependencies
- [ ] Ensure all stories are assigned to units

## Step 3: Generate Context-Appropriate Questions
**DIRECTIVE**: Analyze the requirements, stories, and application design to generate ONLY questions relevant to THIS specific decomposition problem. Use the categories below as inspiration, NOT as a mandatory checklist. Skip entire categories if not applicable.

- EMBED questions using [Answer]: tag format
- Focus on ambiguities and missing information specific to this context
- Generate questions only where user input is needed for decision-making

**Example question categories** (adapt as needed):
- **Story Grouping** - Only if multiple stories exist and grouping strategy is unclear
- **Dependencies** - Only if multiple units likely and integration approach is ambiguous
- **Team Alignment** - Only if team structure or ownership is unclear
- **Technical Considerations** - Only if scalability/deployment requirements differ across units
- **Business Domain** - Only if domain boundaries or bounded contexts are unclear
- **Code Organization (Greenfield multi-unit only)** - Ask deployment model and directory structure preferences

## Step 4: Store UOW Plan
- Save as `aidlc-docs/inception/plans/unit-of-work-plan.md`
- Include all [Answer]: tags for user input
- Ensure plan covers all aspects of system decomposition

## Step 5: Request User Input
- Ask user to fill [Answer]: tags directly in the plan document
- Emphasize importance of decomposition decisions
- Provide clear instructions on completing the [Answer]: tags

## Step 6: Collect Answers
- Wait for user to provide answers to all questions using [Answer]: tags in the document
- Do not proceed until ALL [Answer]: tags are completed
- Review the document to ensure no [Answer]: tags are left blank

## Step 7: ANALYZE ANSWERS (MANDATORY)
Before proceeding, you MUST carefully review all user answers for:
- **Vague or ambiguous responses**: "mix of", "somewhere between", "not sure", "depends"
- **Undefined criteria or terms**: References to concepts without clear definitions
- **Contradictory answers**: Responses that conflict with each other
- **Missing generation details**: Answers that lack specific guidance
- **Answers that combine options**: Responses that merge different approaches without clear decision rules

## Step 8: MANDATORY Follow-up Questions
If the analysis in step 7 reveals ANY ambiguous answers, you MUST:
- Add specific follow-up questions to the plan document using [Answer]: tags
- DO NOT proceed to approval until all ambiguities are resolved
- Examples of required follow-ups:
  - "You mentioned 'mix of A and B' - what specific criteria should determine when to use A vs B?"
  - "You said 'somewhere between A and B' - can you define the exact middle ground approach?"
  - "You indicated 'not sure' - what additional information would help you decide?"
  - "You mentioned 'depends on complexity' - how do you define complexity levels?"

## Step 9: Request Approval
- Ask: "**Unit of work plan complete. Review the plan in aidlc-docs/inception/plans/unit-of-work-plan.md. Ready to proceed to generation?**"
- DO NOT PROCEED until user confirms

## Step 10: Log Approval
- Log prompt and response in audit.md with timestamp
- Use ISO 8601 timestamp format
- Include complete approval prompt text

## Step 11: Update Progress
- Mark Units Planning complete in aidlc-state.md
- Update the "Current Status" section
- Prepare for transition to Units Generation

---

# PART 2: GENERATION

## Step 12: Load Unit of Work Plan
- [ ] Read the complete plan from `aidlc-docs/inception/plans/unit-of-work-plan.md`
- [ ] Identify the next uncompleted step (first [ ] checkbox)
- [ ] Load the context and requirements for that step

## Step 13: Execute Current Step
- [ ] Perform exactly what the current step describes
- [ ] Generate unit artifacts as specified in the plan
- [ ] Follow the approved decomposition approach from Planning
- [ ] Use the criteria and boundaries specified in the plan

## Step 14: Update Progress
- [ ] Mark the completed step as [x] in the unit of work plan
- [ ] Update `aidlc-docs/aidlc-state.md` current status
- [ ] Save all generated artifacts

## Step 15: Continue or Complete
- [ ] If more steps remain, return to Step 12
- [ ] If all steps complete, verify units are ready for design stages
- [ ] Mark Units Generation stage as complete

## Step 16: Present Completion Message

```markdown
# 🔧 Units Generation Complete

[AI-generated summary of units and decomposition created in bullet points]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the units generation artifacts at: `aidlc-docs/inception/application-design/`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the units generation if required
> ✅ **Approve & Continue** - Approve units and proceed to **CONSTRUCTION PHASE**
```

## Step 17: Wait for Explicit Approval
- Do not proceed until the user explicitly approves the units generation
- Approval must be clear and unambiguous
- If user requests changes, update the units and repeat the approval process

## Step 18: Record Approval Response
- Log the user's approval response with timestamp in `aidlc-docs/audit.md`
- Include the exact user response text
- Mark the approval status clearly

## Step 19: Update Progress
- Mark Units Generation stage complete in `aidlc-docs/aidlc-state.md`
- Update the "Current Status" section
- Prepare for transition to CONSTRUCTION PHASE

---

## Critical Rules

### Planning Phase Rules
- Generate ONLY context-relevant questions
- Use [Answer]: tag format for all questions
- Analyze all answers for ambiguities before proceeding
- Resolve ALL ambiguities with follow-up questions
- Get explicit user approval before generation

### Generation Phase Rules
- **NO HARDCODED LOGIC**: Only execute what's written in the unit of work plan
- **FOLLOW PLAN EXACTLY**: Do not deviate from the step sequence
- **UPDATE CHECKBOXES**: Mark [x] immediately after completing each step
- **USE APPROVED APPROACH**: Follow the decomposition methodology from Planning
- **VERIFY COMPLETION**: Ensure all unit artifacts are complete before proceeding

## Completion Criteria
- All planning questions answered and ambiguities resolved
- User approval obtained for the plan
- All steps in unit of work plan marked [x]
- All unit artifacts generated according to plan:
  - `unit-of-work.md` with unit definitions
  - `unit-of-work-dependency.md` with dependency matrix
  - `unit-of-work-story-map.md` with story mappings
  - `unit-of-work-parallel-execution.md` with parallel execution groups
- Units verified and ready for per-unit design stages
- Parallel execution groups validated (no circular dependencies within groups)

---

## Parallel Execution Planning

### Purpose
When multiple units of work exist, analyze the dependency graph to determine which units can be developed in parallel by independent subagents without conflicts. This enables concurrent construction while preventing file collisions, contract violations, and integration failures.

### Grouping Scheme

Units are organized into lettered parallel execution groups. Within each group, units are numbered. Groups execute sequentially (Group A completes before Group B starts). Units within the same group execute in parallel.

**Format**: `Group {Letter}.{Number} - {unit-name}`

**Example**:
```markdown
## Parallel Execution Groups

### Group A (no dependencies — can start immediately)
- A.1 - user-auth-service
- A.2 - notification-service
- A.3 - static-content-module

### Group B (depends on Group A completion)
- B.1 - order-processing-service (depends on A.1)
- B.2 - payment-service (depends on A.1)

### Group C (depends on Group B completion)
- C.1 - reporting-service (depends on B.1, B.2)
```

### Dependency Analysis Rules

1. **Build the dependency graph** from `unit-of-work-dependency.md`
2. **Identify root units** — units with no dependencies on other units → these form Group A
3. **Topological sort** — assign subsequent groups based on dependency depth:
   - Group A: depth 0 (no unit dependencies)
   - Group B: depth 1 (depends only on Group A units)
   - Group C: depth 2 (depends on Group B or earlier units)
   - Continue lettering for deeper dependency chains
4. **Validate**: no circular dependencies exist; every unit is assigned exactly one group

### Inter-Unit Contract Definitions

Before parallel execution begins, contracts between units MUST be defined and locked. These contracts are the integration boundaries that subagents code against independently.

Generate `aidlc-docs/inception/application-design/unit-contracts.md` containing:

```markdown
## Inter-Unit Contracts

### Contract: {provider-unit} → {consumer-unit}
- **Contract ID**: C-{number}
- **Type**: API / Event / Shared Model / Database View
- **Provider**: {unit-name} (Group {Letter}.{Number})
- **Consumer(s)**: {unit-name(s)}
- **Interface Definition**:
  - Endpoint / Event name / Model schema
  - Input types
  - Output types
  - Error types
- **Stability**: LOCKED (do not modify without re-approval)
```

**Rules**:
- Contracts MUST be defined for every edge in the dependency graph
- Contracts are LOCKED after user approval — subagents code against them, not around them
- If a subagent discovers a contract needs to change, it MUST stop and escalate to the orchestrator for re-approval
- Provider units MUST implement the contract interface exactly as defined
- Consumer units MUST code against the contract interface, not the provider's internals

### File Ownership Matrix

Generate a file ownership section in `unit-of-work-parallel-execution.md` that maps directories and files to their owning unit:

```markdown
## File Ownership

| Path Pattern | Owner Unit | Access |
|---|---|---|
| `src/auth/**` | A.1 - user-auth-service | EXCLUSIVE |
| `src/notifications/**` | A.2 - notification-service | EXCLUSIVE |
| `src/shared/models/user.ts` | A.1 - user-auth-service | PROVIDER |
| `src/shared/models/user.ts` | B.1 - order-processing-service | READ-ONLY |
| `src/shared/contracts/**` | ORCHESTRATOR | READ-ONLY (all units) |
```

**Access levels**:
- **EXCLUSIVE**: Only the owning unit's subagent may create or modify files in this path
- **PROVIDER**: The owning unit creates and maintains the file; other units may read but not modify
- **READ-ONLY**: The unit may import/reference but never modify
- **ORCHESTRATOR**: Managed by the orchestrating agent, not by any subagent

### Shared Resource Rules

When units share resources (database, message bus, config files):
1. **Shared models**: Defined in contracts, owned by the provider unit, read-only for consumers
2. **Database migrations**: Each unit owns its own tables/schemas; shared tables require a designated owner unit
3. **Configuration files**: Root-level config (e.g., `package.json`, `docker-compose.yml`) is ORCHESTRATOR-owned — subagents propose changes, orchestrator applies them
4. **Build artifacts**: Each unit manages its own build; integration build is ORCHESTRATOR-owned
