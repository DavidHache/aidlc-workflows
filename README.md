# AI-DLC (AI-Driven Development Life Cycle)

AI-DLC is an intelligent software development workflow that adapts to your needs, maintains quality standards, and keeps you in control of the process. For learning more about AI-DLC Methodology, read this [blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) and the [Method Definition Paper](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) referred in it.

## Table of Contents

- [Quick Start](#quick-start)
- [Usage](#usage)
- [Three-Phase Adaptive Workflow](#three-phase-adaptive-workflow)
- [Key Features](#key-features)
- [Extensions](#extensions)
- [Tenets](#tenets)
- [Prerequisites](#prerequisites)
- [Verifying the Workflow Is Loaded](#verifying-the-workflow-is-loaded)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

---

## Quick Start

Setup takes two steps. See [SETUP.md](SETUP.md) for the full instructions.

**Step 1** — Add AI-DLC as a submodule in your project root:

```bash
git submodule add https://github.com/awslabs/aidlc-workflows.git .aidlc
```

**Step 2** — Paste the prompt from [SETUP.md](SETUP.md) into your AI agent. It will create the right config file for your IDE and gitignore the submodule automatically.

That's it!

---

## Usage

1. Start any software development project by stating your intent starting with the phrase **"Using AI-DLC, ..."** in the chat
2. AI-DLC workflow automatically activates and guides you from there
3. Answer structured questions that AI-DLC asks you
4. Carefully review every plan that AI generates. Provide your oversight and validation
5. Review the execution plan to see which stages will run
6. Carefully review the artifacts and approve each stage to maintain control
7. All the artifacts will be generated in the `aidlc-docs/` directory

---

## Three-Phase Adaptive Workflow

AI-DLC follows a structured three-phase approach that adapts to your project's complexity:

### 🔵 INCEPTION PHASE
Determines **WHAT** to build and **WHY**
- Requirements analysis and validation
- User story creation (when applicable)
- Application Design and creating units of work for parallel development
- Risk assessment and complexity evaluation

### 🟢 CONSTRUCTION PHASE
Determines **HOW** to build it
- Detailed component design
- Code generation and implementation
- Build configuration and testing strategies
- Quality assurance and validation

### 🟡 OPERATIONS PHASE
Deployment and monitoring (future)
- Deployment automation and infrastructure
- Monitoring and observability setup
- Production readiness validation

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Adaptive Intelligence** | Only executes stages that add value to your specific request |
| **Context-Aware** | Analyzes existing codebase and complexity requirements |
| **Risk-Based** | Complex changes get comprehensive treatment, simple changes stay efficient |
| **Question-Driven** | Structured multiple-choice questions in files, not chat |
| **Always in Control** | Review execution plans and approve each phase |
| **Extensible** | Layer custom rules e.g. security, compliance, and organization-specific rules on top of the core workflow |

---

## Extensions

AI-DLC supports an extension system that lets you layer additional rules on top of the core workflow. Extensions are markdown files organized under `aidlc-rules/aws-aidlc-rule-details/extensions/` and are automatically loaded and enforced when enabled during the Requirements Analysis phase.

### How Extensions Work

Extensions are grouped by category (e.g., `security/`, `scalability/`, `accessibility/`). Each category can contain its own rules and any number of subcategories you define.

Each extension should include an **Applicability Question** — a structured multiple-choice question that AI-DLC automatically presents during the Requirements Analysis phase. This lets the user decide whether to enable or skip that extension for the current project. For example, the built-in security extension includes:

```markdown
## Question: Security Extensions
Should security extension rules be enforced for this project?

A) Yes — enforce all SECURITY rules as blocking constraints
B) No — skip all SECURITY rules
X) Other (please describe)

[Answer]:
```

When you create your own extensions, include a similar applicability question so users can opt in or out per project.

Here's the general flow once an extension is enabled:

1. During the Inception phase, AI-DLC presents the extension's applicability question.
2. If enabled, the extension's rules are loaded as mandatory, blocking constraints that apply across all AI-DLC phases.
3. At each stage, the model verifies compliance with all loaded extension rules before allowing the stage to proceed.

### Extension Directory Structure

The workflow currently ships with a baseline security extension.

```
aidlc-rules/
└── aws-aidlc-rule-details/
    └── extensions/
        └── security/                      # Extension category
            └── baseline/
            │   └── security-baseline.md   # Baseline security rules
            ├── compliance/                # Proposed folder hierarchy
            │   ├── hipaa/                 # HIPAA compliance rules
            │   ├── pci-dss/               # PCI-DSS compliance rules
            │   └── soc2/                  # SOC 2 compliance rules
            └── internal-policies/         # Your organization's custom rules
```

### Adding Your Own Extensions

You can extend an existing category or create an entirely new one.

To add rules to an existing category (e.g., security):

1. Create a new directory under `extensions/security/` (e.g., `compliance/hipaa/`).
2. Add one or more markdown files with your rules. Follow the same structure as `security-baseline.md`:
   - Give each rule a unique ID.
   - Include an **Applicability Question** described above.
   - Include a **Rule** section describing the requirement.
   - Include a **Verification** section with concrete checks the model should evaluate.
3. Rules are blocking by default — if verification criteria are not met, the stage cannot proceed until the finding is resolved.

To create a new extension category, add a new directory under `extensions/` (e.g., `extensions/performance/`) and place your rule markdown files inside it following the same format.

---

## Tenets

These are our core principles to guide our decision making.

- **No duplication**. The source of truth lives in one place. If we add support for new tools or formats that require specific files, we generate them from the source rather than maintaining separate copies.

- **Methodology first**. AI-DLC is fundamentally a methodology, not a tool. Users shouldn't need to install anything to get started. That said, we're open to convenience tooling (scripts, CLIs) down the road if it helps users adopt or extend the methodology.

- **Reproducible**. Rules should be clear enough that different models produce similar outcomes. We know models behave differently, but the methodology should minimize variance through explicit guidance.

- **Agnostic**. The methodology works with any IDE, agent, or model. We don't tie ourselves to specific tools or vendors.

- **Human in the loop**. Critical decisions require explicit user confirmation. The agent proposes, the human approves.

---

## Prerequisites

Have one of our supported platforms/tools for Assisted AI Coding installed:

| Platform | Installation Link |
|----------|------------------|
| Kiro | [Install](https://kiro.dev/) |
| Kiro CLI | [Install](https://kiro.dev/cli/) |
| Amazon Q Developer IDE Plugin | [Install](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html) |
| Cursor IDE | [Install](https://cursor.com/) |
| Cline VS Code Extension | [Install](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev) |
| Claude Code CLI | [Install](https://github.com/anthropics/claude-code) |
| GitHub Copilot | [Install](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) + [Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) |

---

## Verifying the Workflow Is Loaded

After setup, confirm the `ai-dlc` steering/rules file is visible in your IDE:

| Platform | How to verify |
|----------|---------------|
| Kiro IDE | Open the Steering files panel and confirm `ai-dlc` appears under Workspace |
| Kiro CLI | Run `/context show` and confirm `.kiro/steering/ai-dlc.md` is listed |
| Amazon Q Developer | Click the **Rules** button in the Q Chat window and confirm `ai-dlc.md` is listed |
| Cursor | Check **Settings → Rules** and confirm `ai-dlc` is listed and enabled |
| Cline | Check the Rules popover under the chat input field |
| Claude Code | Run `/config` or ask "What instructions are currently active?" |
| GitHub Copilot | Select **Configure Chat → Chat Instructions** to verify |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Rules not loading | Verify the `ai-dlc` steering/rules file was created and points to `.aidlc/aidlc-rules/aws-aidlc-rules/core-workflow.md` |
| File encoding issues | Ensure files are UTF-8 encoded |
| Rules not applied in session | Start a new chat session after file changes |
| Rule details not loading | Verify `.aidlc/aidlc-rules/aws-aidlc-rule-details/` exists with subdirectories (`common/`, `inception/`, etc.) |
| Submodule directory empty | Run `git submodule update --init .aidlc` |

---

## Additional Resources

| Resource | Link |
|----------|------|
| AI-DLC Method Definition Paper | [Paper](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) |
| AI-DLC Methodology Blog | [AWS Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) |
| AI-DLC Open-source Launch Blog | [AWS Blog](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/) |
| AI-DLC Example Walkthrough Blog | [AWS Blog](https://aws.amazon.com/blogs/devops/building-with-ai-dlc-using-amazon-q-developer/) |
| Amazon Q Developer Documentation | [Docs](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/q-in-IDE.html) |
| Kiro CLI Documentation | [Docs](https://kiro.dev/docs/cli/steering/) |
| Cursor Rules Documentation | [Docs](https://cursor.com/docs/context/rules) |
| Claude Code Documentation | [GitHub](https://github.com/anthropics/claude-code) |
| GitHub Copilot Documentation | [Docs](https://docs.github.com/en/copilot) |
| Contributing Guidelines | [CONTRIBUTING.md](CONTRIBUTING.md) |
| Code of Conduct | [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) |

---

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
