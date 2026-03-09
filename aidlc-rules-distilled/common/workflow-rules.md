## PROCESS_OVERVIEW

Three-phase lifecycle:
- INCEPTION: planning and architecture (Workspace Detection + conditional phases + Workflow Planning)
- CONSTRUCTION: design, implementation, build and test (per-unit design + Code Planning/Generation + Build & Test)
- OPERATIONS: placeholder for future deployment and monitoring workflows

Adaptive workflow: Workspace Detection (always) → Reverse Engineering (brownfield only) → Requirements Analysis (always, adaptive depth) → Conditional Phases (as needed) → Workflow Planning (always) → Code Generation (always, per-unit) → Build and Test (always)

Always-execute stages: Workspace Detection, Requirements Analysis (adaptive depth), Workflow Planning, Code Generation (per-unit), Build and Test.

Conditional stages: Reverse Engineering, User Stories, Application Design, Units Generation, per-unit design stages (Functional Design, NFR Requirements, NFR Design, Infrastructure Design).

No fixed sequences — stages execute in order that makes sense for specific task.

User role: answer questions in dedicated question files using [Answer]: tags with letter choices (A, B, C, D, E). Option E available for "Other" custom response. Work as team to review and approve each phase. Involve relevant stakeholders for each phase.

INCEPTION stages: Workspace Detection (ALWAYS), Reverse Engineering (CONDITIONAL — brownfield only), Requirements Analysis (ALWAYS — adaptive depth), User Stories (CONDITIONAL), Workflow Planning (ALWAYS), Application Design (CONDITIONAL), Units Generation (CONDITIONAL).

CONSTRUCTION stages: Functional Design (CONDITIONAL, per-unit), NFR Requirements (CONDITIONAL, per-unit), NFR Design (CONDITIONAL, per-unit), Infrastructure Design (CONDITIONAL, per-unit), Code Generation (ALWAYS, per-unit), Build and Test (ALWAYS).

OPERATIONS stages: Operations (PLACEHOLDER).

Key principles: phases execute only when they add value, each phase independently evaluated, INCEPTION focuses on "what" and "why", CONSTRUCTION focuses on "how" plus "build and test", OPERATIONS placeholder for future expansion.

## WELCOME_MESSAGE

[REQ] Display ONCE at start of any AI-DLC workflow. Load from this section. Do NOT load in subsequent interactions.

```markdown
# 👋 Welcome to AI-DLC (AI-Driven Development Life Cycle)! 👋

I'll guide you through an adaptive software development workflow that intelligently tailors itself to your specific needs.

## What is AI-DLC?

AI-DLC is a structured yet flexible software development process that adapts to your project's needs. Think of it as having an experienced software architect who:

- **Analyzes your requirements** and asks clarifying questions when needed
- **Plans the optimal approach** based on complexity and risk
- **Skips unnecessary steps** for simple changes while providing comprehensive coverage for complex projects
- **Documents everything** so you have a complete record of decisions and rationale
- **Guides you through each phase** with clear checkpoints and approval gates

## The Three-Phase Lifecycle

```
                         User Request
                              |
                              v
        ╔═══════════════════════════════════════╗
        ║     INCEPTION PHASE                   ║
        ║     Planning & Application Design     ║
        ╠═══════════════════════════════════════╣
        ║ • Workspace Detection (ALWAYS)        ║
        ║ • Reverse Engineering (COND)          ║
        ║ • Requirements Analysis (ALWAYS)      ║
        ║ • User Stories (CONDITIONAL)          ║
        ║ • Workflow Planning (ALWAYS)          ║
        ║ • Application Design (CONDITIONAL)    ║
        ║ • Units Generation (CONDITIONAL)      ║
        ╚═══════════════════════════════════════╝
                              |
                              v
        ╔═══════════════════════════════════════╗
        ║     CONSTRUCTION PHASE                ║
        ║     Design, Implementation & Test     ║
        ╠═══════════════════════════════════════╣
        ║ • Per-Unit Loop (for each unit):      ║
        ║   - Functional Design (COND)          ║
        ║   - NFR Requirements Assess (COND)    ║
        ║   - NFR Design (COND)                 ║
        ║   - Infrastructure Design (COND)      ║
        ║   - Code Generation (ALWAYS)          ║
        ║ • Build and Test (ALWAYS)             ║
        ╚═══════════════════════════════════════╝
                              |
                              v
        ╔═══════════════════════════════════════╗
        ║     OPERATIONS PHASE                  ║
        ║     Placeholder for Future            ║
        ╠═══════════════════════════════════════╣
        ║ • Operations (PLACEHOLDER)            ║
        ╚═══════════════════════════════════════╝
                              |
                              v
                          Complete
```

### Phase Breakdown:

**INCEPTION PHASE** - *Planning & Application Design*
- **Purpose**: Determines WHAT to build and WHY
- **Activities**: Understanding requirements, analyzing existing code (if any), planning the approach
- **Output**: Clear requirements, execution plan, decisions on the number of units of work for parallel development
- **Your Role**: Answer questions, review plans, approve direction

**CONSTRUCTION PHASE** - *Detailed Design, Implementation & Test*
- **Purpose**: Determines HOW to build it
- **Activities**: Detailed design (when needed), code generation, comprehensive testing
- **Output**: Working code, tests, build instructions
- **Your Role**: Review designs, approve implementation plans, validate results

**OPERATIONS PHASE** - *Deployment & Monitoring (Future)*
- **Purpose**: How to DEPLOY and RUN it
- **Status**: Placeholder for future deployment and monitoring workflows
- **Current State**: Build and test activities handled in CONSTRUCTION phase

## Key Principles:

- ⚡ **Fully Adaptive**: Each stage independently evaluated based on your needs
- 🎯 **Efficient**: Simple changes execute only essential stages
- 📋 **Comprehensive**: Complex changes get full treatment with all safeguards
- 🔍 **Transparent**: You see and approve the execution plan before work begins
- 📝 **Documented**: Complete audit trail of all decisions and changes
- 🎛️ **User Control**: You can request stages be included or excluded

## What Happens Next:

1. **I'll analyze your workspace** to understand if this is a new or existing project
2. **I'll gather requirements** and ask clarifying questions if needed
3. **I'll create an execution plan** showing which stages I propose to run and why
4. **You'll review and approve** the plan (or request changes)
5. **We'll execute the plan** with checkpoints at each major stage
6. **You'll get working code** with complete documentation and tests

The AI-DLC process adapts to:
- 📋 Your intent clarity and complexity
- 🔍 Existing codebase state
- 🎯 Scope and impact of changes
- ⚡ Risk and quality requirements

Let's begin!
```

## QUESTION_FORMAT

[REQ] All questions MUST use this format. NEVER ask questions directly in chat — ALL questions in dedicated question files.

File naming: `{phase-name}-questions.md` (e.g., `classification-questions.md`, `requirements-questions.md`, `story-planning-questions.md`, `design-questions.md`)

Question structure — every question must include meaningful options plus "Other" as last option:

```markdown
## Question [Number]
[Clear, specific question text]

A) [First meaningful option]
B) [Second meaningful option]
[...additional options as needed...]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

[REQ] "Other" is MANDATORY as LAST option for every question. Only include meaningful options — don't make up options to fill slots. Use as many or as few options as make sense (minimum 2 + Other).

Option count: minimum 2 meaningful + "Other", typical 3-4 meaningful + "Other", maximum 5 meaningful + "Other". Options must be mutually exclusive, cover common scenarios, be specific and clear.

User response format: letter choice after [Answer]: tag. Reading responses: read question file, extract answers after [Answer]: tags, validate all answered, proceed with analysis.

Error handling — missing answers: ask user to provide answer. Invalid answers: ask for valid letter choice. Ambiguous answers: ask for letter choice or "Other" with description.

### CONTRADICTION_AND_AMBIGUITY_DETECTION

[REQ] After reading user responses, MUST check for contradictions and ambiguities.

Detecting contradictions — logically inconsistent answers: scope mismatch ("Bug fix" but "Entire codebase affected"), risk mismatch ("Low risk" but "Breaking changes"), timeline mismatch ("Quick fix" but "Multiple subsystems"), impact mismatch ("Single component" but "Significant architecture changes").

Detecting ambiguities — unclear or borderline responses, answers fitting multiple classifications, responses lacking specificity, conflicting indicators across questions.

[COND] contradictions or ambiguities detected → create `{phase-name}-clarification-questions.md`:

```markdown
# [Phase Name] Clarification Questions

I detected contradictions in your responses that need clarification:

## Contradiction 1: [Brief Description]
You indicated "[Answer A]" (Q[X]:[Letter]) but also "[Answer B]" (Q[Y]:[Letter]).
These responses are contradictory because [explanation].

### Clarification Question 1
[Specific question to resolve contradiction]

A) [Option that resolves toward first answer]
B) [Option that resolves toward second answer]
C) [Option that provides middle ground]
D) [Option that reframes the question]

[Answer]: 

## Ambiguity 1: [Brief Description]
Your response to Q[X] ("[Answer]") is ambiguous because [explanation].

### Clarification Question 2
[Specific question to clarify ambiguity]

A) [Clear option 1]
B) [Clear option 2]
C) [Clear option 3]
D) [Clear option 4]

[Answer]: 
```

Workflow: detect → create → inform user → wait → re-validate → proceed only when all contradictions resolved.

## SESSION_CONTINUITY

Welcome back prompt template — when user returns to existing AI-DLC project:

```markdown
**Welcome back! I can see you have an existing AI-DLC project in progress.**

Based on your aidlc-state.md, here's your current status:
- **Project**: [project-name]
- **Current Phase**: [INCEPTION/CONSTRUCTION/OPERATIONS]
- **Current Stage**: [Stage Name]
- **Last Completed**: [Last completed step]
- **Next Step**: [Next step to work on]

**What would you like to work on today?**

A) Continue where you left off ([Next step description])
B) Review a previous stage ([Show available stages])

[Answer]: 
```

[REQ] Session continuity instructions:
1. Always read aidlc-state.md first when detecting existing project
2. Parse current status from workflow file to populate prompt
3. [REQ] Load previous stage artifacts before resuming any stage: Reverse Engineering (architecture.md, code-structure.md, api-documentation.md), Requirements Analysis (requirements.md, requirement-verification-questions.md), User Stories (stories.md, personas.md, story-generation-plan.md), Application Design (components.md, component-methods.md, services.md), Design/Units (unit-of-work.md, unit-of-work-dependency.md, unit-of-work-story-map.md), Per-Unit Design (functional-design.md, nfr-requirements.md, nfr-design.md, infrastructure-design.md), Code Stages (all code files, plans, AND all previous artifacts)
4. Smart context loading by stage: early stages → workspace analysis, requirements/stories → reverse engineering + requirements, design stages → requirements + stories + architecture + design, code stages → ALL artifacts + existing code
5. Adapt options based on architectural choice and current phase
6. Show specific next steps
7. Log continuity prompt in audit.md with timestamp
8. Context summary: after loading artifacts, provide brief summary of what was loaded
9. Asking questions: ALWAYS place in .md files, DO NOT place multiple-choice questions inline in chat

[COND] artifacts missing or corrupted during session resumption → see ERROR_HANDLING for recovery procedures.

## TERMINOLOGY

Phase: one of three high-level lifecycle phases (INCEPTION, CONSTRUCTION, OPERATIONS). Stage: individual workflow activity within a phase. Usage: "The CONSTRUCTION phase contains 7 stages", "The Code Planning stage is always executed".

INCEPTION PHASE: planning and architectural decisions, determine WHAT and WHY, location `inception/`. Stages: Workspace Detection (ALWAYS), Reverse Engineering (CONDITIONAL), Requirements Analysis (ALWAYS), User Stories (CONDITIONAL), Workflow Planning (ALWAYS), Application Design (CONDITIONAL), Units Planning/Generation (CONDITIONAL). Outputs: requirements, user stories, architectural decisions, unit definitions.

CONSTRUCTION PHASE: detailed design and implementation, determine HOW, location `construction/`. Stages: Functional Design (CONDITIONAL, per-unit), NFR Requirements (CONDITIONAL, per-unit), NFR Design (CONDITIONAL, per-unit), Infrastructure Design (CONDITIONAL, per-unit), Code Planning (ALWAYS), Code Generation (ALWAYS), Build and Test (ALWAYS). Outputs: design artifacts, NFR implementations, code, tests.

OPERATIONS PHASE: deployment and operational readiness, location `operations/`. Stages: Operations (PLACEHOLDER). Outputs: build instructions, deployment guides, monitoring setup.

Always-execute stages: Workspace Detection, Requirements Analysis, Workflow Planning, Code Planning, Code Generation, Build and Test.

Conditional stages: Reverse Engineering (brownfield only), User Stories, Application Design, Design (Units Planning/Generation), Functional Design, NFR Requirements, NFR Design, Infrastructure Design.

Architecture terms: Unit of Work (logical grouping of stories for development — planning context), Service (independently deployable component — microservices), Module (logical grouping within service/monolith — not independently deployable), Component (reusable building block — classes, functions, packages).

Stage terminology: Planning vs Generation (planning creates plan with questions/checkboxes, generation executes plan to create artifacts). Depth levels: Minimal, Standard, Comprehensive.

Artifact types: Plans (documents with checkboxes/questions in `aidlc-docs/plans/`), Artifacts (generated outputs in `aidlc-docs/` subdirectories), State files (`aidlc-state.md`, `audit.md`).

Abbreviations: AI-DLC (AI-Driven Development Life Cycle), NFR (Non-Functional Requirements), UOW (Unit of Work), API (Application Programming Interface), CDK (Cloud Development Kit).

## DEPTH_LEVELS

Core principle: when a stage executes, ALL its defined artifacts are created. "Depth" refers to level of detail and rigor within those artifacts, adapting to problem complexity.

Stage selection (binary): Workflow Planning decides EXECUTE or SKIP. [COND] EXECUTE → stage runs, creates ALL defined artifacts. [COND] SKIP → stage doesn't run.

Detail level (adaptive): simple problems → concise artifacts with essential detail, complex problems → comprehensive artifacts with extensive detail. Model decides based on problem characteristics.

Factors influencing detail: request clarity, problem complexity, scope (single file → system-wide), risk level, available context (greenfield vs brownfield), user preferences.

Guiding principle: "Create exactly the detail needed for the problem at hand — no more, no less." Don't inflate simple problems. Don't shortchange complex problems. Let problem characteristics drive detail naturally. All required artifacts always created when stage executes.

## ASCII_DIAGRAM_STANDARDS

[REQ] Use basic ASCII only for diagrams (maximum compatibility).

Allowed: `+` `-` `|` `^` `v` `<` `>` and alphanumeric text.

Forbidden: Unicode box-drawing characters (`┌` `─` `│` `└` `┐` `┘` `├` `┤` `┬` `┴` `┼` `▼` `▲` `►` `◄`).

[REQ] Character width rule: every line in a box MUST have EXACTLY same character count (including spaces).

Validation before creating diagrams: basic ASCII only, no Unicode box-drawing, spaces (not tabs) for alignment, corners use `+`, all box lines same character width, verify corners align vertically in monospace font.

For complex diagrams: use Mermaid (see CONTENT_VALIDATION).

## CONTENT_VALIDATION

[REQ] All generated content MUST be validated before writing to files.

ASCII diagrams: load ASCII_DIAGRAM_STANDARDS, validate each diagram (count characters per line — all lines MUST be same width, use ONLY allowed characters, no Unicode box-drawing, spaces only — no tabs), test alignment.

Mermaid diagram validation:
1. Syntax check: validate Mermaid syntax before file creation
2. Character escaping: ensure special characters properly escaped (use alphanumeric + underscore only for node IDs, escape `"` and `'` in labels)
3. Fallback content: provide text alternative if Mermaid fails validation

[COND] Mermaid validation fails → use text-based workflow representation.

General content validation pre-creation checklist: validate embedded code blocks (Mermaid, JSON, YAML), check special character escaping, verify markdown syntax correctness, test content parsing compatibility, include fallback content for complex elements.

Error prevention: always validate before writing files, escape special characters (particularly in diagrams/code blocks), provide text alternatives for visual content, validate complex content structures.

[COND] validation fails → log error, use fallback content, continue workflow (don't block), inform user simplified content used.

## OVERCONFIDENCE_PREVENTION

Guiding principles:
1. Default to asking: any ambiguity → ask clarifying questions
2. Comprehensive coverage: evaluate ALL relevant categories, don't skip areas
3. Thorough analysis: carefully analyze ALL user responses for ambiguities
4. Mandatory follow-up: create follow-up questions for ANY unclear responses
5. No proceeding with ambiguity: don't move forward until ALL ambiguities resolved

For question generation: evaluate ALL question categories, ask wherever clarification improves quality, default to inclusion rather than exclusion.

For answer analysis: look for vague responses ("depends", "maybe", "not sure", "mix of", "somewhere between"), detect undefined terms, identify contradictory/incomplete answers, create follow-up questions for ANY ambiguities.

Red flags: stages completing without asking questions on complex projects, proceeding with vague responses, skipping entire question categories without justification, making assumptions instead of asking.

Key takeaway: better to ask too many questions than make incorrect assumptions.

## ERROR_HANDLING

Error severity levels: Critical (workflow cannot continue — missing required files, invalid input, system errors), High (phase cannot complete — incomplete answers, contradictory responses, missing dependencies), Medium (phase can continue with workarounds — optional artifacts missing, non-critical validation failures), Low (minor issues — formatting inconsistencies, optional info missing).

General error handling: identify error → assess impact → communicate to user → offer solutions → document in `audit.md`.

Phase-specific error handling:
- Context assessment: cannot read workspace → ask user to verify path/permissions. Corrupted aidlc-state.md → ask user: start fresh or attempt recovery.
- Requirements: contradictory requirements → create follow-up questions, do not proceed. Incomplete answers → highlight unanswered, do not proceed.
- Stories: ambiguous planning answers → add follow-up questions, do not proceed. Uncompleted plan steps → resume from first uncompleted step.
- Application design: unclear architectural decision → add follow-up questions, do not proceed.
- Design: circular unit dependencies → identify, suggest refactoring. Missing plan steps → regenerate plan.
- NFR: incompatible tech stack choices → highlight, ask user to choose, do not proceed.
- Code generation: cannot generate for step → skip, document as incomplete, continue. Syntax errors → fix, regenerate if needed.
- Build and test: unclear build tool → ask user to specify.

Recovery procedures:
- Partial phase completion: load plan file → identify last [x] checkbox → resume from next uncompleted step → verify prior steps complete
- Corrupted state file: backup → ask user current phase → regenerate state file → mark completed phases based on existing artifacts
- Missing artifacts: identify missing → determine if regenerable → [COND] yes → return to that phase, [COND] no → ask user for info manually → document gap in audit.md
- User wants restart: confirm (data will be lost) → archive existing as `{artifact}.backup` → reset phase status → clear checkboxes → re-execute
- User wants skip: confirm user understands implications → document skip reason in audit.md → mark "SKIPPED" in aidlc-state.md → proceed

Session resumption errors:
- Missing artifacts: identify which stage created them → check if stage marked complete → [COND] marked complete but missing → regenerate stage, [COND] not marked complete → resume from that stage
- Inconsistent state: aidlc-state.md shows complete but artifacts don't exist → mark incomplete, re-execute. Artifacts exist but state shows incomplete → verify artifacts, update state. Multiple stages marked current → review artifacts, ask user, correct state.
- Context loading errors: list needed artifacts → check missing/corrupted → regenerate or ask user. Contradictory artifacts → identify contradictions, present to user, reconcile before proceeding.

Resumption best practices: always validate state matches actual artifacts, load incrementally (stage-by-stage), fail fast if critical artifacts missing, communicate clearly what's missing, offer options (regenerate, provide manually, start fresh), document recovery in audit.md.

Error logging format:

```markdown
## Error - [Phase Name]
**Timestamp**: [ISO timestamp]
**Error Type**: [Critical/High/Medium/Low]
**Description**: [What went wrong]
**Cause**: [Why it happened]
**Resolution**: [How it was resolved]
**Impact**: [Effect on workflow]

---
```

Recovery logging format:

```markdown
## Recovery - [Phase Name]
**Timestamp**: [ISO timestamp]
**Issue**: [What needed recovery]
**Recovery Steps**: [What was done]
**Outcome**: [Result of recovery]
**Artifacts Affected**: [List of files]

---
```

Prevention: validate early (check inputs/dependencies before starting), checkpoint often (update checkboxes immediately), communicate clearly, ask questions (don't assume), document everything in audit.md.

## WORKFLOW_CHANGES

Types of mid-workflow changes:

1. Adding skipped phase: confirm request → check dependencies → update execution-plan.md → update aidlc-state.md (PENDING) → execute phase → log in audit.md
2. Skipping planned phase: confirm request → warn about impact → get explicit confirmation → update plan (SKIPPED) → update state → adjust later phases → log
3. Restarting current stage: understand concern → offer options (modify existing | complete restart) → [COND] restart → archive as `{artifact}.backup.{timestamp}`, reset checkboxes, re-execute → log
4. Restarting previous stage: assess impact on dependent stages → warn user of full cascade → get explicit confirmation → archive all affected → reset all affected stages → return to restart point → log
5. Changing stage depth: confirm request → update execution plan → adjust approach → update estimates → log
6. Pausing workflow: complete current step if possible → update checkboxes → update aidlc-state.md → log pause in audit.md → provide resume instructions. On resume: detect existing project → load context → show status → offer options → log resume.
7. Changing architectural decision: assess current progress → explain impact (early = minimal, late = significant rework) → recommend approach → get confirmation → execute change
8. Adding/removing units: assess impact → explain consequences → update unit artifacts (unit-of-work.md, unit-of-work-dependency.md, unit-of-work-story-map.md) → reset affected units → execute changes

General guidelines: always confirm before destructive changes, explain impact, offer alternatives, archive first, update all tracking files, validate after changes.

Change request log format:

```markdown
## Change Request - [Phase Name]
**Timestamp**: [ISO timestamp]
**Request**: [What user wants to change]
**Current State**: [Where we are in workflow]
**Impact Assessment**: [What will be affected]
**User Confirmation**: [User's explicit confirmation]
**Action Taken**: [What was done]
**Artifacts Affected**: [List of files changed/reset]

---
```
