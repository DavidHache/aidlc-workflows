# CONSTRUCTION RULES

PATTERN:COLLECT_ANSWERS = wait for all [Answer]: completed; [REQ] review ALL for vague/ambiguous; [REQ] follow up ANY unclear; watch: "depends", "maybe", "not sure", "mix of", "somewhere between"; block until resolved

PATTERN:APPROVAL_GATE = wait for explicit approval; changes requested → update, re-approve; log approval+response in audit.md with timestamp; mark stage complete in aidlc-state.md

---

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

```markdown
# 🔧 Functional Design Complete - [unit-name]
```

   AI summary (optional): bullet-point summary of business logic models, entities, rules, domain structure. No workflow instructions.

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

8. [PATTERN:APPROVAL_GATE]

---

## NFR_REQUIREMENTS

Purpose: NFR requirements + tech stack choices per unit
Prerequisites: Functional Design complete, artifacts available, execution plan indicates execute

1. Read `aidlc-docs/construction/{unit-name}/functional-design/`; understand business logic complexity
2. Generate plan with [] checkboxes; focus: scalability, performance, availability, security
3. [REQ] Analyze functional design for ALL ambiguities; [REQ] default to asking when ANY ambiguity; EMBED [Answer]: tags
   Categories (evaluate ALL): Scalability Requirements (load, growth, scaling triggers, capacity), Performance Requirements (response times, throughput, latency, benchmarks), Availability Requirements (uptime, DR, failover, continuity), Security Requirements (data protection, compliance, auth, threat models), Tech Stack Selection (preferences, constraints, existing systems, integration), Reliability Requirements (error handling, fault tolerance, monitoring, alerting), Maintainability Requirements (code quality, documentation, testing, ops), Usability Requirements (UX, accessibility, interface)
4. Save → `aidlc-docs/construction/plans/{unit-name}-nfr-requirements-plan.md` with [Answer]: tags
5. [PATTERN:COLLECT_ANSWERS]
6. Create in `aidlc-docs/construction/{unit-name}/nfr-requirements/`: nfr-requirements.md, tech-stack-decisions.md
7. Present completion message:

```markdown
# 📊 NFR Requirements Complete - [unit-name]
```

   AI summary (optional): bullet-point summary of scalability, performance, availability, security requirements + tech stack decisions. No workflow instructions.

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

8. [PATTERN:APPROVAL_GATE]

---

## NFR_DESIGN

Purpose: incorporate NFR requirements into unit design using patterns + logical components
Prerequisites: NFR Requirements complete, artifacts available, execution plan indicates execute

1. Read `aidlc-docs/construction/{unit-name}/nfr-requirements/`; understand scalability, performance, availability, security needs
2. Generate plan with [] checkboxes; focus: design patterns, logical components
3. [REQ] Analyze NFR requirements for ambiguities; EMBED [Answer]: tags; generate ONLY relevant questions
   Example categories (adapt, skip if N/A): Resilience Patterns, Scalability Patterns, Performance Patterns, Security Patterns, Logical Components (queues, caches, etc.)
4. Save → `aidlc-docs/construction/plans/{unit-name}-nfr-design-plan.md` with [Answer]: tags
5. [PATTERN:COLLECT_ANSWERS]
6. Create in `aidlc-docs/construction/{unit-name}/nfr-design/`: nfr-design-patterns.md, logical-components.md
7. Present completion message:

```markdown
# 🎨 NFR Design Complete - [unit-name]
```

   AI summary (optional): bullet-point summary of design patterns, logical components, resilience/scalability/performance patterns. No workflow instructions.

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

8. [PATTERN:APPROVAL_GATE]

---

## INFRASTRUCTURE_DESIGN

Purpose: map logical components to actual infrastructure for deployment
Prerequisites: Functional Design complete, NFR Design recommended, execution plan indicates execute

1. Load `aidlc-docs/construction/{unit-name}/functional-design/`, `nfr-design/` (if exists); identify components needing infrastructure
2. Generate plan with [] checkboxes; focus: mapping to actual services (AWS, Azure, GCP, on-premise)
3. [REQ] Analyze functional + NFR design for infrastructure ambiguities; EMBED [Answer]: tags; generate ONLY relevant questions
   Example categories (adapt, skip if N/A): Deployment Environment, Compute Infrastructure, Storage Infrastructure, Messaging Infrastructure, Networking Infrastructure, Monitoring Infrastructure, Shared Infrastructure
4. Save → `aidlc-docs/construction/plans/{unit-name}-infrastructure-design-plan.md` with [Answer]: tags
5. [PATTERN:COLLECT_ANSWERS]
6. Create `aidlc-docs/construction/{unit-name}/infrastructure-design/`: infrastructure-design.md, deployment-architecture.md; [COND] shared infra → `aidlc-docs/construction/shared-infrastructure.md`
7. Present completion message:

```markdown
# 🏢 Infrastructure Design Complete - [unit-name]
```

   AI summary (optional): bullet-point summary of infrastructure services, deployment architecture, cloud provider choices. No workflow instructions.

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

8. [PATTERN:APPROVAL_GATE]

---

## CODE_GENERATION

Two parts: Part 1 Planning, Part 2 Generation.
Brownfield: "generate" means modify existing files when appropriate, not create duplicates.
Prerequisites: unit design complete, NFR (if executed) complete, all design artifacts available

### PART 1: PLANNING

1. Read unit design artifacts, unit story map; identify dependencies + interfaces; validate readiness
2. Read workspace root + project type from `aidlc-docs/aidlc-state.md`; determine code location (see Critical Rules); [COND brownfield] review code-structure.md for existing files; document exact paths (never aidlc-docs/)
3. Create explicit numbered steps with [] checkboxes for:
   - Project Structure Setup (greenfield only)
   - Business Logic Generation
   - Business Logic Unit Testing
   - Business Logic Summary
   - API Layer Generation
   - API Layer Unit Testing
   - API Layer Summary
   - Repository Layer Generation
   - Repository Layer Unit Testing
   - Repository Layer Summary
   - Frontend Components Generation (if applicable)
   - Frontend Components Unit Testing (if applicable)
   - Frontend Components Summary (if applicable)
   - Database Migration Scripts (if data models exist)
   - Documentation Generation (API docs, README updates)
   - Deployment Artifacts Generation
4. Include unit context: stories, dependencies, interfaces, DB entities, service boundaries
5. Save → `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md` with step numbering, unit context, story traceability; emphasize plan = single source of truth
6. Summarize plan to user: generation approach, step sequence, story coverage, total steps, estimated scope
7. Log approval prompt in `aidlc-docs/audit.md` with timestamp + plan reference
8. Wait for explicit approval of entire plan; changes requested → update + re-approve
9. Log approval response in `aidlc-docs/audit.md` with exact user response + timestamp
10. Mark Code Planning complete in `aidlc-state.md`

### PART 2: GENERATION

11. Read plan from `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`; identify next uncompleted step (first [])
12. Execute current step: verify target dir from plan (never aidlc-docs/); [COND brownfield] check if file exists → modify in-place (never create `ClassName_modified.java` etc.) | not exists → create new; write to correct locations (app code → workspace root, docs → `aidlc-docs/construction/{unit-name}/code/`, build/config → workspace root)
13. Mark completed step [x] in plan; mark associated stories [x] when done; update `aidlc-state.md`; [COND brownfield] verify no duplicate files
14. More steps → return to step 11 | all complete → present completion
15. Present completion message:

```markdown
# 💻 Code Generation Complete - [unit-name]
```

   AI summary (optional): [COND brownfield] distinguish modified vs created files; [COND greenfield] list created files with paths; list tests, docs, deployment artifacts. No workflow instructions.

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

16. [PATTERN:APPROVAL_GATE]

### Critical Rules

Code location: app code → workspace root (NEVER aidlc-docs/); docs → aidlc-docs/ only; read workspace root from aidlc-state.md.

Structure patterns: brownfield → use existing structure; greenfield single unit → `src/`, `tests/`, `config/`; greenfield multi-unit microservices → `{unit-name}/src/`, `{unit-name}/tests/`; greenfield multi-unit monolith → `src/{unit-name}/`, `tests/{unit-name}/`.

Brownfield rules: check file exists before generating; exists → modify in-place; not exists → create new; verify no duplicates after generation.

Planning rules: explicit numbered steps, story traceability, document context + dependencies, explicit user approval before generation.

Generation rules: [REQ] NO HARDCODED LOGIC (only execute plan); [REQ] FOLLOW PLAN EXACTLY; [REQ] UPDATE CHECKBOXES immediately; [REQ] STORY TRACEABILITY (mark stories [x]); [REQ] RESPECT DEPENDENCIES.

Automation-friendly UI code: add `data-testid` to interactive elements; naming: `{component}-{element-role}`; avoid dynamic/auto-generated IDs; keep `data-testid` stable across changes.

Completion criteria: plan created + approved, all steps [x], all stories implemented, all code + tests generated, deployment artifacts generated, unit ready for build.

---

## BUILD_AND_TEST

Purpose: build all units + execute comprehensive testing
Prerequisites: Code Generation complete for all units, all code artifacts generated

1. Analyze testing requirements: unit tests (already generated per unit), integration tests, performance tests, end-to-end tests, contract tests, security tests

2. Create `aidlc-docs/construction/build-and-test/build-instructions.md`:

```markdown
# Build Instructions

## Prerequisites
- **Build Tool**: [Tool name and version]
- **Dependencies**: [List all required dependencies]
- **Environment Variables**: [List required env vars]
- **System Requirements**: [OS, memory, disk space]

## Build Steps

### 1. Install Dependencies
```bash
[Command to install dependencies]
# Example: npm install, mvn dependency:resolve, pip install -r requirements.txt
```

### 2. Configure Environment
```bash
[Commands to set up environment]
# Example: export variables, configure credentials
```

### 3. Build All Units
```bash
[Command to build all units]
# Example: mvn clean install, npm run build, brazil-build
```

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

3. Create `aidlc-docs/construction/build-and-test/unit-test-instructions.md`:

```markdown
# Unit Test Execution

## Run Unit Tests

### 1. Execute All Unit Tests
```bash
[Command to run all unit tests]
# Example: mvn test, npm test, pytest tests/unit
```

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

4. Create `aidlc-docs/construction/build-and-test/integration-test-instructions.md`:

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
```bash
[Commands to start services]
# Example: docker-compose up, start test database
```

### 2. Configure Service Endpoints
```bash
[Commands to configure endpoints]
# Example: export API_URL=http://localhost:8080
```

## Run Integration Tests

### 1. Execute Integration Test Suite
```bash
[Command to run integration tests]
# Example: mvn integration-test, npm run test:integration
```

### 2. Verify Service Interactions
- **Test Scenarios**: [List key integration test scenarios]
- **Expected Results**: [Describe expected outcomes]
- **Logs Location**: [Where to check logs]

### 3. Cleanup
```bash
[Commands to clean up test environment]
# Example: docker-compose down, stop test services
```
```

5. [COND] Create `aidlc-docs/construction/build-and-test/performance-test-instructions.md`:

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
```bash
[Commands to set up performance testing]
# Example: scale services, configure load balancers
```

### 2. Configure Test Parameters
- **Test Duration**: [X] minutes
- **Ramp-up Time**: [X] seconds
- **Virtual Users**: [X] users

## Run Performance Tests

### 1. Execute Load Tests
```bash
[Command to run load tests]
# Example: jmeter -n -t test.jmx, k6 run script.js
```

### 2. Execute Stress Tests
```bash
[Command to run stress tests]
# Example: gradually increase load until failure
```

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

6. [COND] Additional test instructions as needed:
   - Contract tests (microservices) → `aidlc-docs/construction/build-and-test/contract-test-instructions.md`: API contract validation, consumer-driven contracts, schema validation
   - Security tests → `aidlc-docs/construction/build-and-test/security-test-instructions.md`: vulnerability scanning, dependency security, auth testing, input validation
   - E2E tests → `aidlc-docs/construction/build-and-test/e2e-test-instructions.md`: complete user workflows, cross-service scenarios, UI testing

7. Create `aidlc-docs/construction/build-and-test/build-and-test-summary.md`:

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

8. Update `aidlc-docs/aidlc-state.md`: mark Build and Test complete

9. Present results:

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

10. [REQ] Log phase completion in `aidlc-docs/audit.md`:

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
