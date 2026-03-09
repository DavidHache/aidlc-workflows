## FUNCTIONAL_DESIGN

Purpose: detailed business logic design per unit — business logic, algorithms, domain models, entities, relationships, business rules, validation logic, constraints. Technology-agnostic (no infrastructure concerns). Builds upon high-level component design from Application Design (INCEPTION).

Prerequisites: Units Generation complete, unit of work artifacts available, Application Design recommended, execution plan indicates Functional Design should execute.

### Steps

1. Analyze unit context: read unit definition from `aidlc-docs/inception/application-design/unit-of-work.md`, read assigned stories from `aidlc-docs/inception/application-design/unit-of-work-story-map.md`, understand unit responsibilities and boundaries
2. Create functional design plan with checkboxes [] focusing on business logic, domain models, business rules
3. Generate context-appropriate questions — thoroughly analyze unit definition and functional design artifacts to identify ALL areas where clarification improves design. Default to asking questions when ANY ambiguity or missing detail exists. EMBED questions using [Answer]: tag format. Question categories to consider (evaluate ALL): Business Logic Modeling (core entities, workflows, data transformations, business processes), Domain Model (domain concepts, entity relationships, data structures, business objects), Business Rules (decision rules, validation logic, constraints, business policies), Data Flow (data inputs, outputs, transformations, persistence requirements), Integration Points (external system interactions, APIs, data exchange), Error Handling (error scenarios, validation failures, exception handling), Business Scenarios (edge cases, alternative flows, complex business situations), Frontend Components (if applicable — UI component structure, user interactions, state management, form handling)
4. Store plan as `aidlc-docs/construction/plans/{unit-name}-functional-design-plan.md` with all [Answer]: tags
5. Collect and analyze answers — wait for user to complete all [Answer]: tags. [REQ] carefully review ALL responses for vague or ambiguous answers. [REQ] add follow-up questions for ANY unclear responses — do not proceed with ambiguity. Look for: "depends", "maybe", "not sure", "mix of", "somewhere between". Create clarification questions file if ANY ambiguities detected. Do not proceed until ALL ambiguities resolved.
6. Generate functional design artifacts:
   - `aidlc-docs/construction/{unit-name}/functional-design/business-logic-model.md`
   - `aidlc-docs/construction/{unit-name}/functional-design/business-rules.md`
   - `aidlc-docs/construction/{unit-name}/functional-design/domain-entities.md`
   - [COND] unit includes frontend/UI → `aidlc-docs/construction/{unit-name}/functional-design/frontend-components.md` (component hierarchy/structure, props/state definitions, user interaction flows, form validation rules, API integration points)
7. Present completion message:

```markdown
# 🔧 Functional Design Complete - [unit-name]
```

   AI summary (optional): structured bullet-point summary — "Functional design has created [description]:" — list key business logic models/entities, business rules/validation logic, domain model structure/relationships. No workflow instructions. Factual and content-focused.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the functional design artifacts at: `aidlc-docs/construction/[unit-name]/functional-design/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the functional design based on your review  
> ✅ **Continue to Next Stage** - Approve functional design and proceed to **[next-stage-name]**

---
```

8. Wait for explicit approval — do not proceed until user explicitly approves. [COND] user requests changes → update design, repeat approval.
9. Record approval and update progress — log approval in audit.md with timestamp, record user response, mark Functional Design stage complete in aidlc-state.md.

## NFR_REQUIREMENTS

Prerequisites: Functional Design complete for unit, unit functional design artifacts available, execution plan indicates NFR Requirements should execute.

Purpose: determine non-functional requirements for unit and make tech stack choices.

### Steps

1. Analyze functional design — read artifacts from `aidlc-docs/construction/{unit-name}/functional-design/`, understand business logic complexity and requirements
2. Create NFR requirements plan with checkboxes [] focusing on scalability, performance, availability, security
3. Generate context-appropriate questions — thoroughly analyze functional design to identify ALL areas where NFR clarification improves system quality and architecture decisions. Default to asking questions when ANY ambiguity or missing detail exists. EMBED questions using [Answer]: tag format. Question categories to evaluate (consider ALL): Scalability Requirements (expected load, growth patterns, scaling triggers, capacity planning), Performance Requirements (response times, throughput, latency, benchmarks), Availability Requirements (uptime expectations, disaster recovery, failover, business continuity), Security Requirements (data protection, compliance, authentication, authorization, threat models), Tech Stack Selection (technology preferences, constraints, existing systems, integration requirements), Reliability Requirements (error handling, fault tolerance, monitoring, alerting needs), Maintainability Requirements (code quality, documentation, testing, operational requirements), Usability Requirements (user experience, accessibility, interface requirements)
4. Store plan as `aidlc-docs/construction/plans/{unit-name}-nfr-requirements-plan.md` with all [Answer]: tags
5. Collect and analyze answers — wait for user to complete all [Answer]: tags. [REQ] carefully review ALL responses for vague or ambiguous answers. [REQ] add follow-up questions for ANY unclear responses. Look for: "depends", "maybe", "not sure", "mix of", "somewhere between", "standard", "typical". Create clarification questions file if ANY ambiguities detected. Do not proceed until ALL ambiguities resolved.
6. Generate NFR requirements artifacts:
   - `aidlc-docs/construction/{unit-name}/nfr-requirements/nfr-requirements.md`
   - `aidlc-docs/construction/{unit-name}/nfr-requirements/tech-stack-decisions.md`
7. Present completion message:

```markdown
# 📊 NFR Requirements Complete - [unit-name]
```

   AI summary (optional): structured bullet-point summary — "NFR requirements assessment has identified [description]:" — list key scalability, performance, availability requirements, security/compliance requirements, tech stack decisions/rationale. No workflow instructions. Factual and content-focused.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the NFR requirements at: `aidlc-docs/construction/[unit-name]/nfr-requirements/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the NFR requirements based on your review  
> ✅ **Continue to Next Stage** - Approve NFR requirements and proceed to **[next-stage-name]**

---
```

8. Wait for explicit approval — do not proceed until user explicitly approves. [COND] user requests changes → update requirements, repeat approval.
9. Record approval and update progress — log approval in audit.md with timestamp, record user response, mark NFR Requirements stage complete in aidlc-state.md.

## NFR_DESIGN

Prerequisites: NFR Requirements complete for unit, NFR requirements artifacts available, execution plan indicates NFR Design should execute.

Purpose: incorporate NFR requirements into unit design using patterns and logical components.

### Steps

1. Analyze NFR requirements — read from `aidlc-docs/construction/{unit-name}/nfr-requirements/`, understand scalability, performance, availability, security needs
2. Create NFR design plan with checkboxes [] focusing on design patterns and logical components
3. Generate context-appropriate questions — analyze NFR requirements to generate ONLY questions relevant to THIS unit's NFR design. Use categories as inspiration, NOT mandatory checklist. Skip entire categories if not applicable. EMBED questions using [Answer]: tag format. Example categories (adapt as needed): Resilience Patterns (only if fault tolerance approach needs clarification), Scalability Patterns (only if scaling mechanisms unclear), Performance Patterns (only if performance optimization strategy ambiguous), Security Patterns (only if security implementation approach needs input), Logical Components (only if infrastructure components like queues, caches need clarification)
4. Store plan as `aidlc-docs/construction/plans/{unit-name}-nfr-design-plan.md` with all [Answer]: tags
5. Collect and analyze answers — wait for user to complete all [Answer]: tags. Review for vague or ambiguous responses. Add follow-up questions if needed.
6. Generate NFR design artifacts:
   - `aidlc-docs/construction/{unit-name}/nfr-design/nfr-design-patterns.md`
   - `aidlc-docs/construction/{unit-name}/nfr-design/logical-components.md`
7. Present completion message:

```markdown
# 🎨 NFR Design Complete - [unit-name]
```

   AI summary (optional): structured bullet-point summary — "NFR design has incorporated [description]:" — list key design patterns, logical components/infrastructure elements, resilience/scalability/performance patterns applied. No workflow instructions. Factual and content-focused.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the NFR design at: `aidlc-docs/construction/[unit-name]/nfr-design/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the NFR design based on your review  
> ✅ **Continue to Next Stage** - Approve NFR design and proceed to **[next-stage-name]**

---
```

8. Wait for explicit approval — do not proceed until user explicitly approves. [COND] user requests changes → update design, repeat approval.
9. Record approval and update progress — log approval in audit.md with timestamp, record user response, mark NFR Design stage complete in aidlc-state.md.

## INFRASTRUCTURE_DESIGN

Prerequisites: Functional Design complete for unit, NFR Design recommended, execution plan indicates Infrastructure Design should execute.

Purpose: map logical software components to actual infrastructure choices for deployment environments.

### Steps

1. Analyze design artifacts — read functional design from `aidlc-docs/construction/{unit-name}/functional-design/`, read NFR design from `aidlc-docs/construction/{unit-name}/nfr-design/` (if exists), identify logical components needing infrastructure
2. Create infrastructure design plan with checkboxes [] focusing on mapping to actual services (AWS, Azure, GCP, on-premise)
3. Generate context-appropriate questions — analyze functional and NFR design to generate ONLY questions relevant to THIS unit's infrastructure needs. Use categories as inspiration, NOT mandatory checklist. Skip entire categories if not applicable. EMBED questions using [Answer]: tag format. Example categories (adapt as needed): Deployment Environment (only if cloud provider or environment setup unclear), Compute Infrastructure (only if compute service choice needs clarification), Storage Infrastructure (only if database or storage selection ambiguous), Messaging Infrastructure (only if messaging/queuing services need specification), Networking Infrastructure (only if load balancing or API gateway approach unclear), Monitoring Infrastructure (only if observability tooling needs clarification), Shared Infrastructure (only if infrastructure sharing strategy ambiguous)
4. Store plan as `aidlc-docs/construction/plans/{unit-name}-infrastructure-design-plan.md` with all [Answer]: tags
5. Collect and analyze answers — wait for user to complete all [Answer]: tags. Review for vague or ambiguous responses. Add follow-up questions if needed.
6. Generate infrastructure design artifacts:
   - `aidlc-docs/construction/{unit-name}/infrastructure-design/infrastructure-design.md`
   - `aidlc-docs/construction/{unit-name}/infrastructure-design/deployment-architecture.md`
   - [COND] shared infrastructure → `aidlc-docs/construction/shared-infrastructure.md`
7. Present completion message:

```markdown
# 🏢 Infrastructure Design Complete - [unit-name]
```

   AI summary (optional): structured bullet-point summary — "Infrastructure design has mapped [description]:" — list key infrastructure services/components, deployment architecture decisions/rationale, cloud provider choices/service mappings. No workflow instructions. Factual and content-focused.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the infrastructure design at: `aidlc-docs/construction/[unit-name]/infrastructure-design/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the infrastructure design based on your review  
> ✅ **Continue to Next Stage** - Approve infrastructure design and proceed to **Code Generation**

---
```

8. Wait for explicit approval — do not proceed until user explicitly approves. [COND] user requests changes → update design, repeat approval.
9. Record approval and update progress — log approval in audit.md with timestamp, record user response, mark Infrastructure Design stage complete in aidlc-state.md.

## CODE_GENERATION

Two integrated parts: Part 1 - Planning (create detailed code generation plan with explicit steps), Part 2 - Generation (execute approved plan to generate code, tests, artifacts).

For brownfield projects: "generate" means modify existing files when appropriate, not create duplicates.

Prerequisites: Unit Design Generation complete for unit, NFR Implementation (if executed) complete for unit, all unit design artifacts available, unit ready for code generation.

### PART_1_PLANNING

1. Analyze unit context — read unit design artifacts from Unit Design Generation, read unit story map for assigned stories, identify unit dependencies and interfaces, validate unit ready for code generation
2. Create detailed unit code generation plan — read workspace root and project type from `aidlc-docs/aidlc-state.md`, determine code location (see Critical Rules for structure patterns), [COND] brownfield → review reverse engineering code-structure.md for existing files to modify, document exact paths (never aidlc-docs/). Create explicit steps for unit generation:
   1. Project Structure Setup (greenfield only)
   2. Business Logic Generation
   3. Business Logic Unit Testing
   4. Business Logic Summary
   5. API Layer Generation
   6. API Layer Unit Testing
   7. API Layer Summary
   8. Repository Layer Generation
   9. Repository Layer Unit Testing
   10. Repository Layer Summary
   11. Frontend Components Generation (if applicable)
   12. Frontend Components Unit Testing (if applicable)
   13. Frontend Components Summary (if applicable)
   14. Database Migration Scripts (if data models exist)
   15. Documentation Generation (API docs, README updates)
   16. Deployment Artifacts Generation
   Number each step sequentially. Include story mapping references. Add checkboxes [] for each step.
3. Include unit generation context — stories implemented by this unit, dependencies on other units/services, expected interfaces and contracts, database entities owned by this unit, service boundaries and responsibilities
4. Create unit plan document — save as `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md` with step numbering, unit context/dependencies, story traceability. Ensure plan executable step-by-step. Emphasize this plan is single source of truth for Code Generation.
5. Summarize unit plan — provide summary to user, highlight generation approach, explain step sequence and story coverage, note total steps and estimated scope
6. Log approval prompt — before asking for approval, log prompt with timestamp in `aidlc-docs/audit.md`, include reference to complete unit code generation plan, use ISO 8601 timestamp
7. Wait for explicit approval — do not proceed until user explicitly approves. Approval must cover entire plan and generation sequence. [COND] user requests changes → update plan, repeat approval.
8. Record approval response — log user approval response with timestamp in `aidlc-docs/audit.md`, include exact user response text, mark approval status
9. Update progress — mark Code Planning complete in `aidlc-state.md`, update "Current Status" section

### PART_2_GENERATION

10. Load unit code generation plan — read complete plan from `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`, identify next uncompleted step (first [] checkbox), load context for that step
11. Execute current step — verify target directory from plan (never aidlc-docs/), [COND] brownfield → check if target file exists. Generate exactly what current step describes: [COND] file exists → modify in-place (never create `ClassName_modified.java`, `ClassName_new.java`, etc.), [COND] file doesn't exist → create new file. Write to correct locations: application code → workspace root per project structure, documentation → `aidlc-docs/construction/{unit-name}/code/` (markdown only), build/config files → workspace root. Follow unit story requirements. Respect dependencies and interfaces.
12. Update progress — mark completed step as [x] in unit code generation plan, mark associated unit stories as [x] when generation finished, update `aidlc-docs/aidlc-state.md` current status, [COND] brownfield → verify no duplicate files created, save all generated artifacts
13. Continue or complete — [COND] more steps remain → return to step 10, [COND] all steps complete → proceed to completion message
14. Present completion message:

```markdown
# 💻 Code Generation Complete - [unit-name]
```

   AI summary (optional): structured bullet-point summary. [COND] brownfield → distinguish modified vs created files (e.g., "Modified: `src/services/user-service.ts`", "Created: `src/services/auth-service.ts`"). [COND] greenfield → list created files with paths. List tests, documentation, deployment artifacts with paths. Factual, no workflow instructions.

```markdown
> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the generated code at:
> - **Application Code**: `[actual-workspace-path]`
> - **Documentation**: `aidlc-docs/construction/[unit-name]/code/`



> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the generated code based on your review  
> ✅ **Continue to Next Stage** - Approve code generation and proceed to **[next-unit/Build & Test]**

---
```

15. Wait for explicit approval — do not proceed until user explicitly approves. [COND] user requests changes → update code, repeat approval.
16. Record approval and update progress — log approval in audit.md with timestamp, record user response, mark Code Generation stage complete for this unit in aidlc-state.md.

### CRITICAL_RULES

Code location rules:
- Application code: workspace root only (NEVER aidlc-docs/)
- Documentation: aidlc-docs/ only (markdown summaries)
- Read workspace root from aidlc-state.md before generating code

Structure patterns by project type:
- Brownfield: use existing structure (e.g., `src/main/java/`, `lib/`, `pkg/`)
- Greenfield single unit: `src/`, `tests/`, `config/` in workspace root
- Greenfield multi-unit (microservices): `{unit-name}/src/`, `{unit-name}/tests/`
- Greenfield multi-unit (monolith): `src/{unit-name}/`, `tests/{unit-name}/`

Brownfield file modification rules:
- Check if file exists before generating
- [COND] exists → modify in-place (never create copies like `ClassName_modified.java`)
- [COND] doesn't exist → create new file
- Verify no duplicate files after generation

Planning phase rules: create explicit numbered steps, include story traceability, document unit context/dependencies, get explicit user approval before generation.

Generation phase rules:
- NO HARDCODED LOGIC: only execute what is written in unit plan
- FOLLOW PLAN EXACTLY: do not deviate from step sequence
- UPDATE CHECKBOXES: mark [x] immediately after completing each step
- STORY TRACEABILITY: mark unit stories [x] when functionality implemented
- RESPECT DEPENDENCIES: only implement when unit dependencies satisfied

Automation friendly code rules (UI code — web, mobile, desktop):
- Add `data-testid` attributes to interactive elements (buttons, inputs, links, forms)
- Consistent naming: `{component}-{element-role}` (e.g., `login-form-submit-button`, `user-list-search-input`)
- Avoid dynamic or auto-generated IDs that change between renders
- Keep `data-testid` values stable across code changes (only change when element purpose changes)

Completion criteria: complete unit code generation plan created and approved, all steps marked [x], all unit stories implemented, all code and tests generated, deployment artifacts generated, complete unit ready for build and verification.

## BUILD_AND_TEST

Purpose: build all units and execute comprehensive testing strategy.

Prerequisites: Code Generation complete for all units, all code artifacts generated, project ready for build and testing.

### Steps

1. Analyze testing requirements — unit tests (already generated per unit during code generation), integration tests (interactions between units/services), performance tests (load, stress, scalability), end-to-end tests (complete user workflows), contract tests (API contract validation between services), security tests (vulnerability scanning, penetration testing)

2. Generate build instructions — create `aidlc-docs/construction/build-and-test/build-instructions.md`:

```markdown
# Build Instructions

## Prerequisites
- **Build Tool**: [Tool name and version]
- **Dependencies**: [List all required dependencies]
- **Environment Variables**: [List required env vars]
- **System Requirements**: [OS, memory, disk space]

## Build Steps

### 1. Install Dependencies
\`\`\`bash
[Command to install dependencies]
# Example: npm install, mvn dependency:resolve, pip install -r requirements.txt
\`\`\`

### 2. Configure Environment
\`\`\`bash
[Commands to set up environment]
# Example: export variables, configure credentials
\`\`\`

### 3. Build All Units
\`\`\`bash
[Command to build all units]
# Example: mvn clean install, npm run build, brazil-build
\`\`\`

### 4. Verify Build Success
- **Expected Output**: [Describe successful build output]
- **Build Artifacts**: [List generated artifacts and locations]
- **Common Warnings**: [Note any acceptable warnings]

## Troubleshooting

### Build Fails with Dependency Errors
- **Cause**: [Common causes]
- **Solution**: [Step-by-step fix]

### Build Fails with Compilation Errors
- **Cause**: [Common causes]
- **Solution**: [Step-by-step fix]
```

3. Generate unit test execution instructions — create `aidlc-docs/construction/build-and-test/unit-test-instructions.md`:

```markdown
# Unit Test Execution

## Run Unit Tests

### 1. Execute All Unit Tests
\`\`\`bash
[Command to run all unit tests]
# Example: mvn test, npm test, pytest tests/unit
\`\`\`

### 2. Review Test Results
- **Expected**: [X] tests pass, 0 failures
- **Test Coverage**: [Expected coverage percentage]
- **Test Report Location**: [Path to test reports]

### 3. Fix Failing Tests
If tests fail:
1. Review test output in [location]
2. Identify failing test cases
3. Fix code issues
4. Rerun tests until all pass
```

4. Generate integration test instructions — create `aidlc-docs/construction/build-and-test/integration-test-instructions.md`:

```markdown
# Integration Test Instructions

## Purpose
Test interactions between units/services to ensure they work together correctly.

## Test Scenarios

### Scenario 1: [Unit A] → [Unit B] Integration
- **Description**: [What is being tested]
- **Setup**: [Required test environment setup]
- **Test Steps**: [Step-by-step test execution]
- **Expected Results**: [What should happen]
- **Cleanup**: [How to clean up after test]

### Scenario 2: [Unit B] → [Unit C] Integration
[Similar structure]

## Setup Integration Test Environment

### 1. Start Required Services
\`\`\`bash
[Commands to start services]
# Example: docker-compose up, start test database
\`\`\`

### 2. Configure Service Endpoints
\`\`\`bash
[Commands to configure endpoints]
# Example: export API_URL=http://localhost:8080
\`\`\`

## Run Integration Tests

### 1. Execute Integration Test Suite
\`\`\`bash
[Command to run integration tests]
# Example: mvn integration-test, npm run test:integration
\`\`\`

### 2. Verify Service Interactions
- **Test Scenarios**: [List key integration test scenarios]
- **Expected Results**: [Describe expected outcomes]
- **Logs Location**: [Where to check logs]

### 3. Cleanup
\`\`\`bash
[Commands to clean up test environment]
# Example: docker-compose down, stop test services
\`\`\`
```

5. Generate performance test instructions (if applicable) — create `aidlc-docs/construction/build-and-test/performance-test-instructions.md`:

```markdown
# Performance Test Instructions

## Purpose
Validate system performance under load to ensure it meets requirements.

## Performance Requirements
- **Response Time**: < [X]ms for [Y]% of requests
- **Throughput**: [X] requests/second
- **Concurrent Users**: Support [X] concurrent users
- **Error Rate**: < [X]%

## Setup Performance Test Environment

### 1. Prepare Test Environment
\`\`\`bash
[Commands to set up performance testing]
# Example: scale services, configure load balancers
\`\`\`

### 2. Configure Test Parameters
- **Test Duration**: [X] minutes
- **Ramp-up Time**: [X] seconds
- **Virtual Users**: [X] users

## Run Performance Tests

### 1. Execute Load Tests
\`\`\`bash
[Command to run load tests]
# Example: jmeter -n -t test.jmx, k6 run script.js
\`\`\`

### 2. Execute Stress Tests
\`\`\`bash
[Command to run stress tests]
# Example: gradually increase load until failure
\`\`\`

### 3. Analyze Performance Results
- **Response Time**: [Actual vs Expected]
- **Throughput**: [Actual vs Expected]
- **Error Rate**: [Actual vs Expected]
- **Bottlenecks**: [Identified bottlenecks]
- **Results Location**: [Path to performance reports]

## Performance Optimization

If performance doesn't meet requirements:
1. Identify bottlenecks from test results
2. Optimize code/queries/configurations
3. Rerun tests to validate improvements
```

6. Generate additional test instructions (as needed) based on project requirements:
   - Contract tests (microservices) → `aidlc-docs/construction/build-and-test/contract-test-instructions.md`: API contract validation, consumer-driven contract testing, schema validation
   - Security tests → `aidlc-docs/construction/build-and-test/security-test-instructions.md`: vulnerability scanning, dependency security checks, authentication/authorization testing, input validation testing
   - End-to-end tests → `aidlc-docs/construction/build-and-test/e2e-test-instructions.md`: complete user workflow testing, cross-service scenarios, UI testing (if applicable)

7. Generate test summary — create `aidlc-docs/construction/build-and-test/build-and-test-summary.md`:

```markdown
# Build and Test Summary

## Build Status
- **Build Tool**: [Tool name]
- **Build Status**: [Success/Failed]
- **Build Artifacts**: [List artifacts]
- **Build Time**: [Duration]

## Test Execution Summary

### Unit Tests
- **Total Tests**: [X]
- **Passed**: [X]
- **Failed**: [X]
- **Coverage**: [X]%
- **Status**: [Pass/Fail]

### Integration Tests
- **Test Scenarios**: [X]
- **Passed**: [X]
- **Failed**: [X]
- **Status**: [Pass/Fail]

### Performance Tests
- **Response Time**: [Actual] (Target: [Expected])
- **Throughput**: [Actual] (Target: [Expected])
- **Error Rate**: [Actual] (Target: [Expected])
- **Status**: [Pass/Fail]

### Additional Tests
- **Contract Tests**: [Pass/Fail/N/A]
- **Security Tests**: [Pass/Fail/N/A]
- **E2E Tests**: [Pass/Fail/N/A]

## Overall Status
- **Build**: [Success/Failed]
- **All Tests**: [Pass/Fail]
- **Ready for Operations**: [Yes/No]

## Next Steps
[If all pass]: Ready to proceed to Operations phase for deployment planning
[If failures]: Address failing tests and rebuild
```

8. Update state tracking — update `aidlc-docs/aidlc-state.md`: mark Build and Test stage complete, update current status

9. Present results to user:

```
"🔨 Build and Test Complete!

**Build Status**: [Success/Failed]

**Test Results**:
✅ Unit Tests: [X] passed
✅ Integration Tests: [X] scenarios passed
✅ Performance Tests: [Status]
✅ Additional Tests: [Status]

**Generated Files**:
1. ✅ build-instructions.md
2. ✅ unit-test-instructions.md
3. ✅ integration-test-instructions.md
4. ✅ performance-test-instructions.md (if applicable)
5. ✅ [additional test files as needed]
6. ✅ build-and-test-summary.md

Review the summary in aidlc-docs/construction/build-and-test/build-and-test-summary.md

**Ready to proceed to Operations stage for deployment planning?""
```

10. Log interaction — [REQ] log phase completion in `aidlc-docs/audit.md`:

```markdown
## Build and Test Stage
**Timestamp**: [ISO timestamp]
**Build Status**: [Success/Failed]
**Test Status**: [Pass/Fail]
**Files Generated**:
- build-instructions.md
- unit-test-instructions.md
- integration-test-instructions.md
- performance-test-instructions.md
- build-and-test-summary.md

---
```
