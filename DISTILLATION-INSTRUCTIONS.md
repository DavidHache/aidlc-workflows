# Distillation Instructions

## What This Is

`aidlc-rules-distilled/` contains AI-optimized compressed versions of the source rules in `aidlc-rules/`. The source has two subdirectories:

- `aidlc-rules/aws-aidlc-rules/` — the top-level orchestrator (`core-workflow.md`)
- `aidlc-rules/aws-aidlc-rule-details/` — detailed phase rules (common, inception, construction, extensions, operations)

The distilled output mirrors this structure. Source files are the human-readable truth. Distilled files exist purely for AI context efficiency: same rules, fewer tokens.

Relationship is one-way: source → distilled. Never modify source files when editing distilled files.

## Audience: AI Agents Only

No human will ever read distilled files. Optimize exclusively for minimal token count and parse efficiency:

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

## OUTPUT_EQUIVALENCE_CHECK

after any update:
- every output file path in source rules → confirmed in distilled rules
- every document template in source → preserved verbatim in distilled
- every required section header in output templates → preserved
- distinct output artifact count: source == distilled
- any missing or merged → FAIL, fix before proceeding
- output manifest from 1_SCAN vs distilled content → full coverage required
