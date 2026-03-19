# COMMON WORKFLOW RULES

## PROCESS_OVERVIEW

Phases: INCEPTION (what+why) → CONSTRUCTION (how) → OPERATIONS (placeholder)
Flow: Workspace Detection → [COND] Reverse Engineering → Requirements Analysis → [COND] phases → Workflow Planning → per-unit Code Generation → Build and Test
Always: Workspace Detection, Requirements Analysis, Workflow Planning, Code Generation, Build and Test
Conditional: Reverse Engineering, User Stories, Application Design, Units Generation, Functional Design, NFR Requirements, NFR Design, Infrastructure Design
User role: answer [Answer]: tags (A-E, X=Other) in .md files; review+approve each phase

## TERMINOLOGY

Phase: INCEPTION | CONSTRUCTION | OPERATIONS. Stage: activity within phase.
Unit of Work: story grouping. Service: independently deployable. Module: logical grouping within service. Component: reusable building block.
Planning: plan with questions + checkboxes. Generation: execute plan → artifacts.
Depth: Minimal (simple) | Standard (typical) | Comprehensive (complex/high-risk)
Artifacts: plans (`aidlc-docs/plans/`), outputs (`aidlc-docs/` subdirs), state (`aidlc-state.md`, `audit.md`)

## DEPTH_LEVELS

Stage executes → ALL defined artifacts created. Depth = detail level within artifacts, adapts to complexity.
Stage selection: binary (EXECUTE | SKIP via Workflow Planning). Detail level: adaptive (model decides based on clarity, complexity, scope, risk, context, user preference).
Simple → concise artifacts, essential detail. Complex → comprehensive artifacts, extensive detail.
Principle: create exactly the detail needed, no more, no less.

## WELCOME_MESSAGE

Display ONCE at workflow start. Load from @common/welcome-message.md. Do NOT reload in subsequent interactions.

Output template (preserve verbatim):

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

[REQ] Never ask questions in chat. ALL questions in dedicated .md files.

File naming: `{phase-name}-questions.md`

Question structure:
```markdown
## Question [Number]
[Clear, specific question text]

A) [First meaningful option]
B) [Second meaningful option]
[...additional options as needed...]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

Rules: "Other" MANDATORY as LAST option every question; only meaningful options (min 2 + Other, max 5 + Other); mutually exclusive; don't make up options to fill slots.

User responds with letter after [Answer]: tag. Read file → extract answers → validate all answered → proceed.

Error handling: missing answer → ask user to complete; invalid letter → ask for valid choice; explanation instead of letter → ask for letter choice or "Other".

[REQ] After reading responses, check for contradictions + ambiguities: scope mismatch, risk mismatch, timeline mismatch, impact mismatch, borderline/unclear answers. Contradictions/ambiguities detected → create `{phase-name}-clarification-questions.md`:

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

Flow: detect → create clarification file → inform user → wait → re-validate → proceed only when all resolved.

## OVERCONFIDENCE_PREVENTION

Default to asking when ANY ambiguity exists. Evaluate ALL relevant categories, don't skip. Thoroughly analyze ALL responses. [REQ] Follow up ANY unclear response. [REQ] No proceeding with ambiguity.

Red flags: stages completing without questions on complex projects; proceeding with vague responses; skipping categories without justification; assumptions instead of clarification.

## CONTENT_VALIDATION

[REQ] Before creating ANY file, validate content:

ASCII diagrams: load @common/workflow-rules.md ASCII_DIAGRAM_STANDARDS; validate each diagram (same char width per line, only `+` `-` `|` `^` `v` `<` `>` + spaces, no Unicode box-drawing, spaces not tabs, corners use `+`).

Mermaid: validate syntax before file creation; alphanumeric + underscore node IDs; escape special chars; provide text alternative fallback.

General: validate embedded code blocks; check special char escaping; verify markdown syntax; include fallback for complex elements.

Validation failure → log error, use fallback, continue workflow, inform user.

## ASCII_DIAGRAM_STANDARDS

[REQ] Basic ASCII only: `+` `-` `|` `^` `v` `<` `>` + alphanumeric text
FORBIDDEN: Unicode box-drawing (`┌` `─` `│` `└` `┐` `┘` etc.)
[REQ] Every line in a box MUST have EXACTLY same character count (including spaces)
Corners: `+`. Spaces only (no tabs).
For complex diagrams: use Mermaid (see CONTENT_VALIDATION).

## SESSION_CONTINUITY

When existing project detected, present:

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

Instructions:
1. Always read aidlc-state.md first
2. Parse current status
3. [REQ] Load previous stage artifacts before resuming: Reverse Engineering (architecture.md, code-structure.md, api-documentation.md), Requirements (requirements.md, questions.md), Stories (stories.md, personas.md, plan.md), Application Design (components.md, component-methods.md, services.md), Units (unit-of-work.md, dependency.md, story-map.md), Per-Unit Design (functional-design.md, nfr-*.md, infrastructure-design.md), Code (all code + plans + ALL previous artifacts)
4. Smart context loading: early stages → workspace analysis; requirements/stories → RE + requirements; design → requirements + stories + architecture + design; code → ALL artifacts + code
5. Adapt options to architectural choice + current phase
6. Show specific next steps
7. Log continuity prompt in audit.md
8. Provide brief context summary after loading
9. [REQ] Questions always in .md files, never inline in chat

Missing/corrupted artifacts during resumption → see ERROR_HANDLING.

## ERROR_HANDLING

Severity: Critical (workflow blocked) | High (phase can't complete) | Medium (workaround possible) | Low (minor, non-blocking)

General: identify error → assess impact → communicate → offer solutions → log in audit.md.

Phase-specific: Context (can't read → verify path/permissions; corrupted state → fresh start | recovery), Requirements (contradictions → follow-up, block; incomplete → highlight, block), Stories (ambiguous → follow-up, block; uncompleted → resume first uncompleted), Design (circular deps → identify + suggest refactoring; missing steps → regenerate), NFR (incompatible tech → highlight + ask user; human tasks → mark HUMAN TASK + wait), Code (can't generate → skip + document; syntax errors → fix + regenerate), Operations (unclear build tool → ask user; unclear deployment → ask user)

Recovery procedures:
- Partial completion: load plan → find last [x] → resume next uncompleted
- Corrupted state: backup → ask user current phase → regenerate state from artifacts
- Missing artifacts: identify → regenerate if possible | ask user | document gap
- User wants restart: confirm → archive as .backup → reset state + checkboxes → re-execute
- User wants skip: confirm + warn impact → mark SKIPPED → adjust later phases → log

Session resumption errors:
- Missing artifacts: identify stage → check state → regenerate | resume from that stage
- Empty/corrupted artifact: backup → regenerate | ask user
- State shows complete but artifacts missing: mark incomplete → re-execute
- Artifacts exist but state shows incomplete: verify artifacts → update state → proceed
- Multiple "current" stages: review artifacts → ask user → correct state
- Can't load context: list needed artifacts → check missing → regenerate | complete prerequisites
- Contradictory artifacts: identify → present to user → reconcile before proceeding

Best practices: validate state matches artifacts; load incrementally; fail fast on critical missing; communicate clearly; offer options (regenerate | provide manually | fresh start); log recovery in audit.md.

## WORKFLOW_CHANGES

### Adding skipped phase
Confirm → check dependencies → update execution-plan.md + aidlc-state.md → execute → log in audit.md

### Skipping planned phase
Confirm → warn impact → explicit confirmation → mark SKIPPED → adjust later phases → log

### Restarting current stage
Understand concern → offer modify vs restart → if restart: archive as .backup.{timestamp}, reset checkboxes, mark IN PROGRESS, re-execute → log

### Restarting previous stage
Assess impact on all dependent stages → warn user of full cascade → explicit confirmation → archive all affected → reset all affected → return to stage → log

### Changing depth
Confirm → update execution-plan.md → adjust approach → update timeline → log

### Pausing workflow
Complete current step if possible → update checkboxes → update state → log pause → provide resume instructions

### Changing architecture (monolith ↔ microservices)
Assess progress → explain impact (early = minimal, late = significant) → recommend approach → get confirmation → execute

### Adding/removing units
Assess impact on completed design/code → explain consequences → update unit artifacts (unit-of-work.md, dependency.md, story-map.md) → reset affected units → execute changes

General: always confirm before destructive changes; archive before modifying; update all tracking files; validate consistency after; log thoroughly.