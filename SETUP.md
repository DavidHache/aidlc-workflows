# AI-DLC Setup

## Step 1 — Add the AI-DLC rules to your project

Run this once in your project root:

```bash
git submodule add https://github.com/aws-samples/aidlc-workflows .aidlc
```

## Step 2 — Paste this into your AI agent and let it do the rest

```
Set up AI-DLC in this project by doing the following:

1. Create the appropriate rules/steering file for your IDE using the options below.
   Pick the one that matches the agent you are running in:

   - Kiro IDE or Kiro CLI     → create `.kiro/steering/ai-dlc.md`
   - Amazon Q Developer       → create `.amazonq/rules/ai-dlc.md`
   - Cursor                   → create `.cursor/rules/ai-dlc.mdc` with frontmatter:
                                  ---
                                  description: "AI-DLC workflow"
                                  alwaysApply: true
                                  ---
   - Cline                    → create `.clinerules/ai-dlc.md`
   - Claude Code              → create `CLAUDE.md`
   - GitHub Copilot           → create `.github/copilot-instructions.md`
   - Any other agent          → create `AGENTS.md`

2. The file content should be:
   When the user invokes AI-DLC, read and follow `.aidlc/aidlc-rules-distilled/core-workflow.md`
   to start the workflow. All rule detail files referenced by the workflow are
   located under `.aidlc/aidlc-rules-distilled/`.

3. Add `.aidlc` to `.gitignore` unless I explicitly ask you not to.

4. Confirm what file you created and that `.aidlc` is gitignored.
```

## Updating AI-DLC

To pull the latest rules:

```bash
git submodule update --remote .aidlc
```
