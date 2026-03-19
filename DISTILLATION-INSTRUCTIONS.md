# Distillation Instructions

## What This Is

`aidlc-rules-distilled/` contains AI-optimized compressed versions of the source rules in `aidlc-rules/`. The source has two subdirectories:

- `aidlc-rules/aws-aidlc-rules/` — the top-level orchestrator (`core-workflow.md`)
- `aidlc-rules/aws-aidlc-rule-details/` — detailed phase rules (common, inception, construction, extensions, operations)

The distilled output mirrors this structure. Source files are the human-readable truth. Distilled files exist purely for AI context efficiency: same rules, fewer tokens.

Relationship is one-way: source → distilled. Never modify source files when editing distilled files.

## Audience: AI Agents Only

No human will ever read distilled files. Optimize exclusively for minimal token count and parse efficiency. Apply maximum compression uniformly to ALL files — orchestrator files, detail files, common rules, extensions, operations. No file or section gets lighter treatment.

- Strip ALL emoji from rule prose and headers — zero exceptions
- No bold, italic, or decorative formatting — use formatting only when structurally meaningful (code blocks for templates, `backticks` for paths)
- No horizontal rules, emoji bullet markers, or visual hierarchy tricks
- EXCEPTION: emoji/formatting inside output templates the AI must reproduce verbatim are PRESERVED exactly as-is — the rule is: strip from INSTRUCTIONS, preserve in OUTPUT TEMPLATES

## Output Equivalence

Distillation compresses RULES, not the outputs those rules produce. A distilled ruleset is correct only if an AI following it produces identical output artifacts — same file names, same directory structure, same document sections, same detail level.

- Source says "create `inception/application-design/components.md`" → distilled MUST preserve that exact path
- Source defines a template with specific sections → distilled MUST preserve that template verbatim
- Source rules across multiple files specify different output artifacts → distilled MUST preserve ALL specs — merging rules ≠ merging outputs
- Output file names, directory structure, document templates, section headers → PRESERVE_VERBATIM, not compressible

### Output Contract Checklist

Before finalizing any distilled file:
1. Every output file path in source rules appears in distilled version
2. Every document template/structure preserved verbatim
3. Every required section header within output documents preserved
4. Distinct output artifact count equals source count
5. No two source output artifacts merged into one

### Common Mistakes

- Merging `components.md`, `services.md`, `component-methods.md`, `component-dependency.md` rules into one section → AI infers one file → WRONG. Must still produce four separate files
- Compressing output template section headers → WRONG. Required format, not prose
- Dropping explicit file path from "write this to X" → WRONG. Path is required output spec
- Joining separate plan steps with `+` or commas (e.g., "Business Logic Generation + Unit Testing + Summary") → AI interprets as ONE combined step → WRONG. Each source step MUST remain a separate numbered item in the distilled output
- Compressing enumerated sub-categories into a flat list (e.g., source lists "Functional Requirements: Core features, user interactions, system behaviors" → distilled drops the sub-items) → AI loses specificity cues that guide correct behavior → WRONG. Preserve enumerated sub-categories as inline lists
- Flattening sequential numbered steps (e.g., "Step 1... Step 2... Step 3..." each producing a distinct artifact) into a single bullet list → AI treats artifacts as optional/low-priority → WRONG. Preserve step numbering when each step produces a distinct output artifact
- Replacing a full markdown template code block with a summary of its section names (e.g., source has a complete ```markdown template for build-instructions.md → distilled replaces it with "template sections: Prerequisites, Build Steps, Troubleshooting") → AI generates shallow/incomplete artifacts because it doesn't know the expected structure → WRONG. Full markdown template code blocks that define the structure of output artifacts MUST be preserved verbatim in the distilled file

## Distilled File Organization

The distilled output mirrors the two-level source structure:

```
aidlc-rules-distilled/
├── aws-aidlc-rules/
│   └── core-workflow.md                    # from aidlc-rules/aws-aidlc-rules/core-workflow.md (distilled last)
└── aws-aidlc-rule-details/
    ├── common/workflow-rules.md            # all aws-aidlc-rule-details/common/ sources merged
    ├── inception/inception-rules.md        # all aws-aidlc-rule-details/inception/ sources merged
    ├── construction/construction-rules.md  # all aws-aidlc-rule-details/construction/ sources merged
    ├── extensions/...                      # each extension standalone
    └── operations/...                      # standalone
```

Source → distilled path mapping:
- `aidlc-rules/aws-aidlc-rules/core-workflow.md` → `aidlc-rules-distilled/aws-aidlc-rules/core-workflow.md`
- `aidlc-rules/aws-aidlc-rule-details/{phase}/` → `aidlc-rules-distilled/aws-aidlc-rule-details/{phase}/`

Merge strategy:
- Same subdirectory, same phase → merge into one distilled file
- Cross-cutting extensions → standalone
- Placeholder/minimal files → standalone

Merging RULES into one file ≠ merging OUTPUTS. Merged rule file must still instruct AI to produce every individual output artifact.

## Processing Order

Deepest/leaf files first → root. Prevents broken cross-references.

Order: `extensions/` → `operations/` → `construction/` → `inception/` → `common/` → `core-workflow.md`

Within each directory: runtime execution order.

---

## PIPELINE

follow_order: each step builds on previous

### 1_SCAN
- read ALL source files in both source directories:
  - `aidlc-rules/aws-aidlc-rules/` (core workflow orchestrator)
  - `aidlc-rules/aws-aidlc-rule-details/` (detailed phase rules)
- group `aws-aidlc-rule-details/` files by directory (common, inception, construction, extensions/*, operations)
- `aws-aidlc-rules/core-workflow.md` is always standalone (distilled last)
- merge candidates: files in same `aws-aidlc-rule-details/` subdir serving same phase → single distilled file
- standalone candidates: cross-cutting extensions, placeholders, isolated files
- extract output manifest: catalog every output file path, document template, required section header

### 2_MERGE
- combine merge candidates into single content block per distilled file
- `## UPPERCASE_SECTION_NAME` headers to separate former files
- preserve every output artifact specification from every merged file — never consolidate output specs

### 3_DEDUPLICATE
- rules stated >1 time across sections → keep in most relevant section only
- rule in summary + intro + body → appears exactly once
- NEVER deduplicate output artifact specifications — different output files both remain even if surrounding instructions are similar

### 4_REORDER
- sections in execution order (runtime sequence)
- earlier sections MUST NOT reference concepts defined later
- within phase: follow stage execution sequence from core-workflow.md

### 5_SEMANTIC_COMPRESSION
Rewrite in terse imperative language:
- drop articles, conjunctions, filler ("you should", "make sure to", "it is important that")
- prose paragraphs → `key: value` lines
- "if X then Y" → `condition→action`
- keep ONLY examples that define a required format AI must reproduce exactly
- drop all other examples, rationale, "why" explanations
- strip ALL emoji from rule/instruction prose (preserve inside verbatim output templates)
- strip decorative formatting: no bold/italic for emphasis, no horizontal rules, no emoji bullets
- NEVER compress output file paths, document templates, or output section headers

#### UNIVERSAL COMPRESSION MANDATE

[REQ] ALL distilled files — detail files, orchestrator files, common rules, every single file — receive IDENTICAL maximum compression. There is NO category of distilled file that gets lighter treatment. The rules below apply uniformly:

- Common/shared rule files (workflow-rules.md) compress EXACTLY as hard as construction-rules.md or inception-rules.md
- Orchestrator files (core-workflow.md) compress EXACTLY as hard as detail files
- Overview/terminology/process sections compress EXACTLY as hard as step-by-step sections
- Descriptive prose compresses to `key: value` or semicolon-separated lines regardless of which file it appears in

If a section in ANY distilled file reads like natural English sentences with articles, conjunctions, and filler — it is under-compressed. No exceptions. No "this file is more conceptual so it needs more prose." The audience is an LLM. Terse is always better.

#### Instructional Prose Compression Rules

These rules apply to ALL distilled files equally — detail files, orchestrator files, common rules, extensions, operations. No file or section gets lighter treatment. The distiller's primary failure mode is leaving instructional sub-bullets, step descriptions, overview prose, and repeated patterns at near-source verbosity. An advanced LLM does not need natural-language sentences to follow numbered steps, nor does it need the same information stated multiple times across sections.

##### Rule 0: Eliminate cross-section redundancy within a file

When the same information appears in multiple sections of the same distilled file (e.g., stage lists in PROCESS_OVERVIEW + TERMINOLOGY, phase descriptions in overview + welcome template), keep it in ONE place only — the most structurally relevant section. The LLM does not need the same list of stages repeated in prose, then in a terminology table, then in a template.

BEFORE (PROCESS_OVERVIEW repeats what TERMINOLOGY and WELCOME_MESSAGE already contain):
```
## PROCESS_OVERVIEW

Three-phase lifecycle:
- INCEPTION: planning + architecture (Workspace Detection + conditional phases + Workflow Planning)
- CONSTRUCTION: design, implementation, build+test (per-unit design + Code Planning/Generation + Build & Test)
- OPERATIONS: placeholder for future deployment/monitoring

Adaptive workflow: Workspace Detection (always) → Reverse Engineering (brownfield only) → Requirements Analysis (always, adaptive depth) → conditional phases (as needed) → Workflow Planning (always) → Code Generation (always, per-unit) → Build and Test (always)

Always-execute stages: Workspace Detection, Requirements Analysis, Workflow Planning, Code Generation (per-unit), Build and Test
Conditional stages: Reverse Engineering, User Stories, Application Design, Units Generation, per-unit design (Functional Design, NFR Requirements, NFR Design, Infrastructure Design)

Team role: answer questions in dedicated files using [Answer]: tags with letter choices (A, B, C, D, E); option E/X = "Other"; review + approve each phase; involve relevant stakeholders
```

AFTER (terse, no duplication with TERMINOLOGY or WELCOME_MESSAGE):
```
## PROCESS_OVERVIEW

Phases: INCEPTION (what+why) → CONSTRUCTION (how) → OPERATIONS (placeholder)
Flow: Workspace Detection → [COND] Reverse Engineering → Requirements Analysis → [COND] phases → Workflow Planning → per-unit Code Generation → Build and Test
Always: Workspace Detection, Requirements Analysis, Workflow Planning, Code Generation, Build and Test
Conditional: Reverse Engineering, User Stories, Application Design, Units Generation, per-unit design stages
User role: answer [Answer]: tags in .md files (A/B/C/D/E/X choices); review + approve each phase
```

The stage-by-stage breakdown with descriptions belongs in TERMINOLOGY (which defines each term). The visual diagram belongs in WELCOME_MESSAGE (output template). PROCESS_OVERVIEW should be a terse routing table only.

##### Rule 1: Collapse multi-bullet actions into semicolon-separated single lines

Each numbered step should be ONE line (or as few as possible). Sub-bullets that describe sequential or parallel actions within a step become semicolons on the same line.

BEFORE (current distilled — too verbose):
```
1. Analyze Unit Context
   - Read unit definition from `aidlc-docs/inception/application-design/unit-of-work.md`
   - Read assigned stories from `aidlc-docs/inception/application-design/unit-of-work-story-map.md`
   - Understand unit responsibilities and boundaries
```

AFTER (correct):
```
1. Read `aidlc-docs/inception/application-design/unit-of-work.md`, `unit-of-work-story-map.md`; understand unit boundaries
```

BEFORE (current distilled — too verbose):
```
5. Collect and Analyze Answers
   - Wait for user to complete all [Answer]: tags
   - [REQ] Review ALL responses for vague/ambiguous answers
   - [REQ] Add follow-up questions for ANY unclear responses
   - Look for: "depends", "maybe", "not sure", "mix of", "somewhere between"
   - Create clarification questions file if ANY ambiguities detected
   - Do not proceed until ALL ambiguities resolved
```

AFTER (correct):
```
5. Wait for all [Answer]: completed; [REQ] review ALL for vague/ambiguous; [REQ] follow up ANY unclear; watch: "depends", "maybe", "not sure", "mix of", "somewhere between"; block until resolved
```

##### Rule 2: Drop step name labels when the action is self-evident

Step names like "Analyze Unit Context", "Create Functional Design Plan", "Store Plan" are human-readable labels. The LLM does not need them — the action content is sufficient. Drop the label, keep the action.

BEFORE:
```
4. Store Plan
   - Save as `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md`
   - Include all [Answer]: tags
```

AFTER:
```
4. Save → `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md` with [Answer]: tags
```

BEFORE:
```
2. Create Functional Design Plan
   - Generate plan with checkboxes [] for functional design
   - Focus on business logic, domain models, business rules
```

AFTER:
```
2. Generate plan with [] checkboxes; focus: business logic, domain models, business rules
```

##### Rule 3: Compress category/sub-category lists into dense inline format

When a step lists categories with sub-items (e.g., question categories), use terse `Name (sub1, sub2, sub3)` inline format. One category per comma-separated entry, NOT one category per indented bullet.

BEFORE (current distilled — 8 indented bullets):
```
   - Question categories to consider (evaluate ALL):
     - Business Logic Modeling: core entities, workflows, data transformations, business processes
     - Domain Model: domain concepts, entity relationships, data structures, business objects
     - Business Rules: decision rules, validation logic, constraints, business policies
     - Data Flow: data inputs, outputs, transformations, persistence requirements
     - Integration Points: external system interactions, APIs, data exchange
     - Error Handling: error scenarios, validation failures, exception handling
     - Business Scenarios: edge cases, alternative flows, complex situations
     - Frontend Components (if applicable): UI component structure, user interactions, state management, form handling
```

AFTER (correct — single block, inline):
```
   Categories (evaluate ALL): Business Logic Modeling (entities, workflows, transformations, processes), Domain Model (concepts, relationships, structures, objects), Business Rules (decisions, validation, constraints, policies), Data Flow (inputs, outputs, transformations, persistence), Integration Points (external systems, APIs, exchange), Error Handling (error scenarios, validation failures, exceptions), Business Scenarios (edge cases, alternative flows), Frontend Components [COND frontend] (component structure, interactions, state, forms)
```

##### Rule 4: Extract repeated patterns into named references

When multiple sections repeat the same approval/logging/answer-collection pattern with near-identical wording, define the pattern ONCE at the top of the file and reference it by name.

BEFORE (repeated in FUNCTIONAL_DESIGN, NFR_REQUIREMENTS, NFR_DESIGN, INFRASTRUCTURE_DESIGN):
```
8. Wait for Explicit Approval
   - Do not proceed until explicit unambiguous approval
   - changes requested → update design, repeat approval

9. Record Approval and Update Progress
   - Log approval in audit.md with timestamp
   - Record user approval response with timestamp
   - Mark [Stage] complete in aidlc-state.md
```

AFTER (define once, reference everywhere):

At top of file:
```
PATTERN:APPROVAL_GATE = wait for explicit approval; changes requested → update, re-approve; log approval+response in audit.md with timestamp; mark stage complete in aidlc-state.md
```

In each section:
```
8. [PATTERN:APPROVAL_GATE]
```

Similarly for answer collection:
```
PATTERN:COLLECT_ANSWERS = wait for all [Answer]: completed; [REQ] review ALL for vague/ambiguous; [REQ] follow up ANY unclear; watch: "depends", "maybe", "not sure", "mix of", "somewhere between"; block until resolved
```

##### Rule 5: Compress conditional/prerequisite blocks into single key-value lines

BEFORE:
```
Prerequisites: Functional Design complete for unit, functional design artifacts available, execution plan indicates stage should execute.

Purpose: determine non-functional requirements for unit, make tech stack choices.
```

AFTER:
```
Prerequisites: Functional Design complete, artifacts available, execution plan indicates execute
Purpose: NFR requirements + tech stack choices per unit
```

##### Rule 6: Compress artifact creation lists

When a step creates multiple files in the same directory, use the common path prefix once.

BEFORE:
```
6. Generate Functional Design Artifacts
   - Create `aidlc-docs/construction/{unit-name}/functional-design/business-logic-model.md`
   - Create `aidlc-docs/construction/{unit-name}/functional-design/business-rules.md`
   - Create `aidlc-docs/construction/{unit-name}/functional-design/domain-entities.md`
   - [COND] unit includes frontend/UI → Create `aidlc-docs/construction/{unit-name}/functional-design/frontend-components.md` with: component hierarchy/structure, props/state definitions, user interaction flows, form validation rules, API integration points
```

AFTER:
```
6. Create in `aidlc-docs/construction/{unit-name}/functional-design/`: business-logic-model.md, business-rules.md, domain-entities.md; [COND] frontend/UI → frontend-components.md (hierarchy, props/state, interaction flows, form validation, API integration)
```

##### Rule 7: Drop verbs that add no information

"Read", "Analyze", "Understand", "Identify", "Determine" before a noun often add nothing. The step number and context imply the action.

BEFORE:
```
1. Analyze Design Artifacts
   - Read functional design from `aidlc-docs/construction/{unit-name}/functional-design/`
   - Read NFR design from `aidlc-docs/construction/{unit-name}/nfr-design/` (if exists)
   - Identify logical components needing infrastructure
```

AFTER:
```
1. Load `aidlc-docs/construction/{unit-name}/functional-design/`, `nfr-design/` (if exists); identify components needing infrastructure
```

##### Rule 8: Compress repeated per-stage orchestrator blocks

Orchestrator files (like core-workflow.md) often repeat the same execution pattern for every stage: log → load rules → execute → present completion → wait approval → log response. Define the common pattern ONCE, then each stage block becomes just its unique content (execute/skip conditions + stage-specific behavior).

BEFORE (repeated for EVERY construction stage in core-workflow.md):
```
#### Functional Design (COND per-unit)

Execute if: new data models/schemas, complex business logic, business rules need detailed design
Skip if: simple logic, no new business logic

1. [REQ] Log user input in audit.md
2. Load @construction/functional-design steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion (see functional-design); NO emergent 3-option behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

#### NFR Requirements (COND per-unit)

Execute if: performance requirements, security considerations, scalability concerns, tech stack selection needed
Skip if: no NFR requirements, tech stack determined

1. [REQ] Log user input in audit.md
2. Load @construction/nfr-requirements steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion; NO emergent behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md
```

AFTER (define pattern once, each stage is 2-3 lines):
```
PATTERN:STAGE_EXEC(rulefile) = [REQ] log user input in audit.md → load @{rulefile} steps → execute → [REQ] present standardized 2-option completion per rule file (NO emergent behavior) → wait for explicit approval (DO NOT proceed until confirmed) → [REQ] log response in audit.md

#### Functional Design (COND per-unit)
Execute if: new data models/schemas, complex business logic, detailed business rules needed
Skip if: simple logic, no new business logic
[PATTERN:STAGE_EXEC(construction/functional-design)]

#### NFR Requirements (COND per-unit)
Execute if: performance, security, scalability requirements, tech stack selection needed
Skip if: no NFR requirements, tech stack determined
[PATTERN:STAGE_EXEC(construction/nfr-requirements)]
```

This applies equally to INCEPTION stages that follow the same log → load → execute → approve → log pattern. The unique content per stage is ONLY the execute/skip conditions and any stage-specific deviations from the pattern.

##### Rule 9: Compress descriptive/overview sections the same as step sections

Overview, terminology, and process description sections are NOT exempt from compression. They contain instructional prose that the LLM parses identically to step instructions. Compress them with the same intensity.

BEFORE (current distilled PROCESS_OVERVIEW — still reads like documentation):
```
## PROCESS_OVERVIEW

Three-phase lifecycle:
- INCEPTION: planning + architecture (Workspace Detection + conditional phases + Workflow Planning)
- CONSTRUCTION: design, implementation, build+test (per-unit design + Code Planning/Generation + Build & Test)
- OPERATIONS: placeholder for future deployment/monitoring

Adaptive workflow: Workspace Detection (always) → Reverse Engineering (brownfield only) → Requirements Analysis (always, adaptive depth) → conditional phases (as needed) → Workflow Planning (always) → Code Generation (always, per-unit) → Build and Test (always)

Always-execute stages: Workspace Detection, Requirements Analysis, Workflow Planning, Code Generation (per-unit), Build and Test
Conditional stages: Reverse Engineering, User Stories, Application Design, Units Generation, per-unit design (Functional Design, NFR Requirements, NFR Design, Infrastructure Design)

Team role: answer questions in dedicated files using [Answer]: tags with letter choices (A, B, C, D, E); option E/X = "Other"; review + approve each phase; involve relevant stakeholders
```

AFTER (correctly compressed):
```
## PROCESS_OVERVIEW

Phases: INCEPTION (what+why) → CONSTRUCTION (how) → OPERATIONS (placeholder)
Flow: Workspace Detection → [COND] Reverse Engineering → Requirements Analysis → [COND] phases → Workflow Planning → Code Generation (per-unit) → Build and Test
Always: Workspace Detection, Requirements Analysis, Workflow Planning, Code Generation, Build and Test
Conditional: Reverse Engineering, User Stories, Application Design, Units Generation, Functional Design, NFR Requirements, NFR Design, Infrastructure Design
User role: answer [Answer]: tags (A-E, X=Other) in .md files; review+approve each phase
```

BEFORE (current distilled TERMINOLOGY — too many full sentences):
```
## TERMINOLOGY

Phase = high-level lifecycle (INCEPTION, CONSTRUCTION, OPERATIONS)
Stage = individual workflow activity within a phase (e.g., Requirements Analysis stage)

Unit of Work = logical grouping of stories for development (planning context)
Service = independently deployable component (microservices)
Module = logical grouping within a service/monolith (not independently deployable)
Component = reusable building block (classes, functions, packages)

Planning = creating plan with questions + checkboxes
Generation = executing plan to create artifacts

Depth levels: Minimal (simple), Standard (typical), Comprehensive (complex/high-risk)

Artifacts: plans in `aidlc-docs/plans/`, generated outputs in `aidlc-docs/` subdirs
State files: `aidlc-state.md` (workflow state), `audit.md` (audit trail)
```

AFTER (correctly compressed):
```
## TERMINOLOGY

Phase: INCEPTION | CONSTRUCTION | OPERATIONS. Stage: activity within phase.
Unit of Work: story grouping. Service: independently deployable. Module: logical grouping within service. Component: reusable building block.
Planning: plan with questions + checkboxes. Generation: execute plan → artifacts.
Depth: Minimal (simple) | Standard (typical) | Comprehensive (complex/high-risk)
Artifacts: plans (`aidlc-docs/plans/`), outputs (`aidlc-docs/` subdirs), state (`aidlc-state.md`, `audit.md`)
```

BEFORE (current distilled ERROR_HANDLING phase-specific — verbose bullets):
```
Phase-specific handling:
- Context: can't read workspace → ask user verify path/permissions; corrupted aidlc-state.md → ask fresh start or recovery
- Requirements: contradictory → follow-up questions, block until resolved; incomplete answers → highlight unanswered, block
- Stories: ambiguous answers → follow-up, block; uncompleted steps → resume from first uncompleted
```

AFTER (correctly compressed):
```
Phase-specific: Context (can't read → verify path/permissions; corrupted state → fresh start | recovery), Requirements (contradictions → follow-up, block; incomplete → highlight, block), Stories (ambiguous → follow-up, block; uncompleted → resume first uncompleted)
```

##### Compression Target

Instructional steps (excluding output templates) should achieve 40-60% token reduction vs source. If a distilled step is >80% of source token count, it is under-compressed. Re-apply rules 1-9.

#### Granularity Preservation Rules
- when source lists N separate sequential steps that each produce a distinct artifact or plan item, distilled MUST preserve N separate numbered items — NEVER join with `+`, `&`, or commas into fewer items
- when source enumerates sub-categories under a category (e.g., "Functional Requirements: Core features, user interactions, system behaviors"), distilled MUST preserve the sub-items as an inline list — dropping them removes specificity cues the AI needs to produce correct output
- when source uses numbered steps (Step 1, Step 2...) where each step has its own artifact, distilled MUST preserve the step-per-artifact structure — flattening into a single list degrades artifact quality
- when source lists explicit evaluation areas with sub-bullets (e.g., completeness analysis categories with their specific concerns), preserve the sub-bullets as terse inline items — these are behavioral cues, not prose

#### Template Preservation Rules
- when a source step contains a markdown code block template (``` ```markdown ... ``` ```) that defines the structure of an output artifact, the ENTIRE template code block MUST be preserved verbatim in the distilled file — it is an output specification, not compressible prose
- NEVER replace a full template with a summary of its section names (e.g., "template sections: X, Y, Z") — the AI needs the complete template to produce correct output
- this applies to ALL artifact templates: build-instructions.md, unit-test-instructions.md, integration-test-instructions.md, performance-test-instructions.md, build-and-test-summary.md, execution-plan.md, aidlc-state.md, and any other template that defines output structure
- the surrounding instructional prose (e.g., "Create this file with the following structure:") CAN be compressed — only the template code block itself is verbatim

### 6_SYMBOLIC_ENCODING
Convert to structured notation:
- sequences: `A → B → C`
- conditional: `[COND]` or `condition→action`
- required/mandatory: `[REQ]`
- file references: `@filename` (relative to distilled root, respecting `aws-aidlc-rules/` and `aws-aidlc-rule-details/` prefixes)
- OR: `A | B`, AND: `A & B`
- unordered lists: comma-separated inline
- ordered steps: `1. 2. 3.`
- section headers: `## UPPERCASE_NAME` (no emoji prefixes)
- no emoji in encoded output (exception: inside verbatim output template code blocks)
- output artifact paths and templates: preserve as-is, do not encode symbolically
- named patterns: `PATTERN:NAME = definition` at file top, `[PATTERN:NAME]` to invoke (see Rule 4 in Instructional Prose Compression)

---

## PRESERVE_VERBATIM
- file names and paths (rule file references AND output artifact paths)
- tag names (e.g. `[Answer]:`)
- allowed/forbidden character sets
- format patterns AI must reproduce exactly (question file structure, completion message templates)
- markdown code blocks defining required output formats
- output document templates including all section headers
- output directory structure specifications
- handoff message formats referencing specific file paths
- enumerated sub-categories under evaluation/analysis areas (these are behavioral cues that guide output specificity, not compressible prose)
- per-step artifact granularity in plan/generation sequences (source has N steps → distilled has N steps, never fewer)
- FULL markdown template code blocks that define the structure of output artifacts (e.g., build-instructions.md template, unit-test-instructions.md template, build-and-test-summary.md template) — these are output specifications, NEVER replace with section name summaries

## DROP
- worked examples unless example IS the required format
- ALL emoji from rule/instruction prose (exception: emoji inside output template code blocks preserved)
- decorative formatting: bold single-line rules, horizontal rules, italic emphasis, emoji bullet markers
- summaries restating rules already in body
- rationale and "why" context
- section headers adding no information beyond content below them
- step name labels when the action content is self-evident (e.g., "Store Plan" before "Save as `path`")
- redundant verbs before nouns when context implies the action (e.g., "Read X" → just `X` when inside a numbered step)
- multi-line sub-bullet structures when a single semicolon-separated line conveys the same instructions
- repeated pattern instances beyond the first (extract to PATTERN:NAME, reference thereafter)
- repeated per-stage execution blocks in orchestrator files (extract to parameterized PATTERN:STAGE_EXEC, reference per stage)
- cross-section redundancy within a file (same stage lists, phase descriptions, or role descriptions appearing in multiple sections — keep in one place only)

---

## UPDATE_EXISTING

source_file_changed:
1. read modified source file in full
2. find corresponding section in distilled file (by section header)
3. identify delta: new | removed | modified | reworded rule
4. delta affects output artifacts → update output manifest
5. apply delta using 5_SEMANTIC_COMPRESSION + 6_SYMBOLIC_ENCODING
6. run QUALITY_CHECK

new_source_file_added:
1. read new source file
2. belongs in existing distilled file (new section) | needs own distilled file
3. existing → full pipeline on new content, append as `## SECTION_NAME`
4. new file → create `aidlc-rules-distilled/{aws-aidlc-rules|aws-aidlc-rule-details}/{folder}/{name}.md`, full pipeline
5. extract new output artifact specs → add to output manifest
6. run QUALITY_CHECK

source_file_removed:
1. find corresponding section in distilled file
2. remove section
3. distilled file now empty → delete it
4. run QUALITY_CHECK

## FULL_REBUILD

when: major restructuring, or distilled files suspected out of sync

1. delete all files in `aidlc-rules-distilled/`
2. scan `aidlc-rules/` recursively → group by merge strategy
3. extract complete output manifest from all source rules
4. process backwards: extensions → operations → construction → inception → common → core-workflow
5. each group: full pipeline (steps 1–6)
6. QUALITY_CHECK on every distilled file
7. OUTPUT_EQUIVALENCE_CHECK

---

## QUALITY_CHECK

after any update:
- every behavior-changing rule present in distilled version
- no distilled rule contradicts source
- no rule appears >1 time across sections in a distilled file
- distilled file shorter than combined source(s) — if not, re-compress
- no examples unless they define a required format
- sections in execution order
- cross-references use correct `@path` and `## SECTION` targets
- all `[REQ]` markers preserved
- no emoji in rule/instruction prose (only inside verbatim output template code blocks)

### Step Granularity Check
- count numbered plan/generation steps in source → count in distilled → must be equal
- no `+` or `&` joining what were separate steps in source
- each source step that produces a distinct artifact → separate numbered item in distilled

### Sub-Category Preservation Check
- for every source section that lists evaluation areas with sub-items (e.g., completeness analysis, question categories), verify distilled preserves the sub-items as inline lists
- missing sub-items = FAIL (they are behavioral cues, not droppable prose)

### Template Code Block Check
- for every source step that contains a markdown code block template defining an output artifact's structure, verify the distilled version preserves the COMPLETE template code block verbatim
- template replaced with section name summary = FAIL (AI cannot produce correct output without the full template)
- count template code blocks in source → count in distilled → must be equal

### Instructional Prose Compression Audit

For every numbered step in the distilled file that is NOT an output template:
0. Check for cross-section redundancy. Same information (stage lists, phase descriptions, role descriptions) appears in >1 section of the same file = FAIL → apply Rule 0 (keep in one place only)
1. Count sub-bullets under the step. >2 sub-bullets for a non-artifact step = FAIL → apply Rule 1 (collapse to semicolons)
2. Check for step name labels. Label present + action content is self-evident = FAIL → apply Rule 2 (drop label)
3. Check category lists. Categories as indented bullets = FAIL → apply Rule 3 (inline dense format)
4. Scan for repeated patterns (approval gates, answer collection, logging). Same pattern appears >2 times = FAIL → apply Rule 4 (extract PATTERN:NAME)
5. Check prerequisite/purpose blocks. >1 line for simple prerequisites = FAIL → apply Rule 5 (single key-value line)
6. Check artifact creation lists. Multiple `Create` lines with shared path prefix = FAIL → apply Rule 6 (common prefix once)
7. Check for redundant verbs. "Read X" / "Analyze X" / "Understand X" where context implies action = FAIL → apply Rule 7 (drop verb)
8. Check for repeated per-stage blocks. Same numbered step sequence (e.g., log → load → execute → present → wait → log) appears in >2 stage definitions = FAIL → apply Rule 8 (extract parameterized PATTERN:STAGE_EXEC)
9. Check descriptive/overview sections. Section reads like natural English with articles, conjunctions, multi-word phrases where `key: value` would suffice = FAIL → apply Rule 9 (compress identically to step sections)

Any FAIL → re-compress the step, then re-run this audit on the fixed version.

## OUTPUT_EQUIVALENCE_CHECK

after any update:
- every output file path in source rules → confirmed in distilled rules
- every document template in source → preserved verbatim in distilled
- every required section header in output templates → preserved
- distinct output artifact count: source == distilled
- any missing or merged → FAIL, fix before proceeding
- output manifest from 1_SCAN vs distilled content → full coverage required

---

## FULL_SECTION_EXAMPLE

This shows the complete transformation of a real section (FUNCTIONAL_DESIGN) from current under-compressed distilled output to correctly compressed output. Use this as the reference standard.

### BEFORE (under-compressed — current distilled output):

```
## FUNCTIONAL_DESIGN

Purpose: detailed business logic design per unit (technology-agnostic, no infrastructure concerns). Builds on Application Design from INCEPTION.

Prerequisites: Units Generation complete, unit-of-work artifacts available, Application Design recommended, execution plan indicates stage should execute.

### Steps

1. Analyze Unit Context
   - Read unit definition from `aidlc-docs/inception/application-design/unit-of-work.md`
   - Read assigned stories from `aidlc-docs/inception/application-design/unit-of-work-story-map.md`
   - Understand unit responsibilities and boundaries

2. Create Functional Design Plan
   - Generate plan with checkboxes [] for functional design
   - Focus on business logic, domain models, business rules

3. Generate Context-Appropriate Questions
   - [REQ] Thoroughly analyze unit definition and functional design artifacts to identify ALL areas needing clarification
   - [REQ] Default to asking questions when ANY ambiguity exists
   - EMBED questions using [Answer]: tag format
   - Question categories to consider (evaluate ALL):
     - Business Logic Modeling: core entities, workflows, data transformations, business processes
     - Domain Model: domain concepts, entity relationships, data structures, business objects
     - Business Rules: decision rules, validation logic, constraints, business policies
     - Data Flow: data inputs, outputs, transformations, persistence requirements
     - Integration Points: external system interactions, APIs, data exchange
     - Error Handling: error scenarios, validation failures, exception handling
     - Business Scenarios: edge cases, alternative flows, complex situations
     - Frontend Components (if applicable): UI component structure, user interactions, state management, form handling

4. Store Plan
   - Save as `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md`
   - Include all [Answer]: tags

5. Collect and Analyze Answers
   - Wait for user to complete all [Answer]: tags
   - [REQ] Review ALL responses for vague/ambiguous answers
   - [REQ] Add follow-up questions for ANY unclear responses
   - Look for: "depends", "maybe", "not sure", "mix of", "somewhere between"
   - Create clarification questions file if ANY ambiguities detected
   - Do not proceed until ALL ambiguities resolved

6. Generate Functional Design Artifacts
   - Create `aidlc-docs/construction/{unit-name}/functional-design/business-logic-model.md`
   - Create `aidlc-docs/construction/{unit-name}/functional-design/business-rules.md`
   - Create `aidlc-docs/construction/{unit-name}/functional-design/domain-entities.md`
   - [COND] unit includes frontend/UI → Create `aidlc-docs/construction/{unit-name}/functional-design/frontend-components.md` with: component hierarchy/structure, props/state definitions, user interaction flows, form validation rules, API integration points

7. Present Completion Message
   [completion template...]

8. Wait for Explicit Approval
   - Do not proceed until explicit unambiguous approval
   - changes requested → update design, repeat approval

9. Record Approval and Update Progress
   - Log approval in audit.md with timestamp
   - Record user approval response with timestamp
   - Mark Functional Design complete in aidlc-state.md
```

### AFTER (correctly compressed):

```
## FUNCTIONAL_DESIGN

Purpose: detailed business logic design per unit, technology-agnostic. Builds on Application Design.
Prerequisites: Units Generation complete, unit-of-work artifacts available, execution plan indicates execute

1. Read `aidlc-docs/inception/application-design/unit-of-work.md`, `unit-of-work-story-map.md`; understand unit boundaries
2. Generate plan with [] checkboxes; focus: business logic, domain models, business rules
3. [REQ] Analyze unit definition for ALL ambiguities; [REQ] default to asking when ANY ambiguity; EMBED [Answer]: tags
   Categories (evaluate ALL): Business Logic Modeling (entities, workflows, transformations, processes), Domain Model (concepts, relationships, structures, objects), Business Rules (decisions, validation, constraints, policies), Data Flow (inputs, outputs, transformations, persistence), Integration Points (external systems, APIs, exchange), Error Handling (error scenarios, validation failures, exceptions), Business Scenarios (edge cases, alternative flows), Frontend Components [COND frontend] (component structure, interactions, state, forms)
4. Save → `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md` with [Answer]: tags
5. [PATTERN:COLLECT_ANSWERS]
6. Create in `aidlc-docs/construction/{unit-name}/functional-design/`: business-logic-model.md, business-rules.md, domain-entities.md; [COND] frontend/UI → frontend-components.md (hierarchy, props/state, interaction flows, form validation, API integration)
7. Present completion message:
   [completion template preserved verbatim...]
8. [PATTERN:APPROVAL_GATE]
```

Key differences:
- 9 steps → 8 steps (approval+logging merged into pattern reference)
- Sub-bullets eliminated (semicolons instead)
- Step labels dropped
- Categories compressed to inline
- Artifact paths use common prefix
- Repeated patterns referenced, not repeated
- ~55% fewer tokens in instructional prose
- ALL file paths, [REQ] markers, sub-category items, and output templates preserved

---

## ORCHESTRATOR_FILE_EXAMPLE

This shows how to compress core-workflow.md (or similar orchestrator files) where every stage repeats the same execution pattern.

### BEFORE (under-compressed — repeated 6-step block per stage):

```
### Per-Unit Loop

#### Functional Design (COND per-unit)

Execute if: new data models/schemas, complex business logic, business rules need detailed design
Skip if: simple logic, no new business logic

1. [REQ] Log user input in audit.md
2. Load @construction/functional-design steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion (see functional-design); NO emergent 3-option behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

#### NFR Requirements (COND per-unit)

Execute if: performance requirements, security considerations, scalability concerns, tech stack selection needed
Skip if: no NFR requirements, tech stack determined

1. [REQ] Log user input in audit.md
2. Load @construction/nfr-requirements steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion; NO emergent behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

#### NFR Design (COND per-unit)

Execute if: NFR Requirements executed, NFR patterns need incorporation
Skip if: no NFR requirements, NFR Requirements skipped

1. [REQ] Log user input in audit.md
2. Load @construction/nfr-design steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion; NO emergent behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md

#### Infrastructure Design (COND per-unit)

Execute if: infrastructure services need mapping, deployment architecture required, cloud resources need spec
Skip if: no infrastructure changes, infrastructure defined

1. [REQ] Log user input in audit.md
2. Load @construction/infrastructure-design steps
3. Execute for this unit
4. [REQ] Present standardized 2-option completion; NO emergent behavior
5. Wait for explicit approval; DO NOT proceed until confirmed
6. [REQ] Log response in audit.md
```

### AFTER (correctly compressed — pattern defined once, stages are 3 lines each):

```
PATTERN:STAGE_EXEC(rulefile) = [REQ] log user input in audit.md → load @{rulefile} steps → execute → [REQ] present standardized 2-option completion per rule file (NO emergent behavior) → wait for explicit approval (DO NOT proceed until confirmed) → [REQ] log response in audit.md

### Per-Unit Loop

#### Functional Design (COND per-unit)
Execute if: new data models/schemas, complex business logic, detailed business rules
Skip if: simple logic, no new business logic
[PATTERN:STAGE_EXEC(construction/functional-design)]

#### NFR Requirements (COND per-unit)
Execute if: performance, security, scalability requirements, tech stack selection needed
Skip if: no NFR requirements, tech stack determined
[PATTERN:STAGE_EXEC(construction/nfr-requirements)]

#### NFR Design (COND per-unit)
Execute if: NFR Requirements executed, NFR patterns need incorporation
Skip if: no NFR requirements, NFR Requirements skipped
[PATTERN:STAGE_EXEC(construction/nfr-design)]

#### Infrastructure Design (COND per-unit)
Execute if: infrastructure mapping needed, deployment architecture required, cloud resources need spec
Skip if: no infrastructure changes, infrastructure defined
[PATTERN:STAGE_EXEC(construction/infrastructure-design)]
```

Key differences:
- 4 stages × 6 steps = 24 lines of repeated steps → 4 stages × 1 pattern reference = 4 lines + 1 pattern definition
- Execute/skip conditions compressed to single lines
- ~75% fewer tokens for the per-unit loop section
- ALL behavioral rules preserved in the pattern definition ([REQ] markers, NO emergent behavior, DO NOT proceed)
