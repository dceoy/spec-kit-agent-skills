---
name: spec-kit-analyze
description: Perform non-destructive cross-artifact consistency and quality analysis across spec.md, plan.md, and tasks.md after task generation. Identifies duplications, ambiguities, coverage gaps, and constitution violations. Use after creating tasks.md to validate before implementation.
allowed-tools: Bash, Read, Grep, Glob
---

# Spec-Kit Analyze

Cross-artifact consistency and quality analysis for feature specifications. This is a **validation step** after creating tasks.md.

## When to Use

- After creating tasks.md with `spec-kit-tasks`
- Before starting implementation with `spec-kit-implement`
- User asks to "check consistency" or "validate artifacts"
- Need to identify gaps or conflicts before coding

## What It Does

**STRICTLY READ-ONLY** - Does not modify any files.

1. **Loads artifacts**: spec.md, plan.md, tasks.md, constitution.md
2. **Detects issues**: Duplications, ambiguities, gaps, inconsistencies
3. **Validates coverage**: Maps requirements to tasks
4. **Checks constitution**: Ensures alignment with project principles
5. **Produces report**: Structured analysis with severity levels

## Prerequisites

- tasks.md must exist (created by `spec-kit-tasks`)
- spec.md should exist (created by `spec-kit-specify`)
- plan.md should exist (created by `spec-kit-plan`)
- Run from repository root

## Basic Usage

### Step 1: Initialize Analysis

Run prerequisites check:

```bash
.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks
```

Provides:

- FEATURE_DIR path
- AVAILABLE_DOCS list
- Tasks content

Derives paths:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md
- CONSTITUTION = .specify/memory/constitution.md

### Step 2: Load Artifacts (Progressive)

Loads only necessary context:

**From spec.md:**

- Overview/Context
- Functional Requirements
- Non-Functional Requirements
- User Stories
- Edge Cases

**From plan.md:**

- Architecture/stack choices
- Data Model references
- Phases
- Technical constraints

**From tasks.md:**

- Task IDs
- Descriptions
- Phase grouping
- Parallel markers [P]
- File paths

**From constitution.md:**

- Project principles
- MUST/SHOULD requirements

### Step 3: Build Semantic Models

Creates internal representations (not shown in output):

- **Requirements inventory**: Each requirement with stable key
- **User story/action inventory**: Discrete actions with acceptance criteria
- **Task coverage mapping**: Maps tasks to requirements
- **Constitution rule set**: Principle names and normative statements

### Step 4: Detection Passes

**A. Duplication Detection**

- Near-duplicate requirements
- Lower-quality phrasing

**B. Ambiguity Detection**

- Vague adjectives: "fast", "scalable", "secure", "intuitive", "robust"
- Unresolved placeholders: TODO, TKTK, ???, `<placeholder>`

**C. Underspecification**

- Requirements missing object or measurable outcome
- User stories missing acceptance criteria
- Tasks referencing undefined files/components

**D. Constitution Alignment**

- Conflicts with MUST principles (always CRITICAL)
- Missing mandated sections/gates

**E. Coverage Gaps**

- Requirements with zero tasks
- Tasks with no mapped requirement
- Non-functional requirements not reflected in tasks

**F. Inconsistency**

- Terminology drift (same concept, different names)
- Data entities in plan but not spec (or vice versa)
- Task ordering contradictions
- Conflicting requirements

**Limit:** Maximum 50 findings (aggregate overflow in summary)

### Step 5: Severity Assignment

**CRITICAL:**

- Violates constitution MUST
- Missing core spec artifact
- Requirement with zero coverage blocking baseline functionality

**HIGH:**

- Duplicate or conflicting requirement
- Ambiguous security/performance attribute
- Untestable acceptance criterion

**MEDIUM:**

- Terminology drift
- Missing non-functional task coverage
- Underspecified edge case

**LOW:**

- Style/wording improvements
- Minor redundancy not affecting execution

### Step 6: Analysis Report

Outputs Markdown report (no file writes):

```markdown
## Specification Analysis Report

| ID  | Category      | Severity | Location(s)      | Summary                                      | Recommendation                    |
| --- | ------------- | -------- | ---------------- | -------------------------------------------- | --------------------------------- |
| A1  | Duplication   | HIGH     | spec.md:L120-134 | Two similar requirements for auth            | Merge; keep clearer version       |
| U2  | Underspec     | MEDIUM   | plan.md:L45      | User model references undefined "role" field | Add role definition to data model |
| C3  | Coverage      | HIGH     | spec.md:FR-5     | Performance requirement has no tasks         | Add performance testing tasks     |
| I4  | Inconsistency | MEDIUM   | Multiple         | "User" vs "Account" terminology              | Normalize to single term          |
| B5  | Ambiguity     | HIGH     | spec.md:NFR-2    | "Fast response" not quantified               | Define specific latency threshold |

**Coverage Summary:**

| Requirement Key     | Has Task? | Task IDs  | Notes            |
| ------------------- | --------- | --------- | ---------------- |
| user-can-register   | Yes       | T007-T014 | Complete         |
| user-can-login      | Yes       | T015-T022 | Complete         |
| performance-metrics | No        | -         | Missing tasks    |
| security-audit-log  | Yes       | T030      | Partial coverage |

**Constitution Alignment Issues:**

| Principle                      | Violation                     | Severity | Location     |
| ------------------------------ | ----------------------------- | -------- | ------------ |
| "All APIs must use versioning" | No version in endpoint design | CRITICAL | plan.md:L120 |

**Unmapped Tasks:**

| Task ID | Description                | Issue                |
| ------- | -------------------------- | -------------------- |
| T025    | Create analytics dashboard | No requirement found |

**Metrics:**

- Total Requirements: 15
- Total Tasks: 30
- Coverage %: 87% (13/15 requirements have tasks)
- Ambiguity Count: 3
- Duplication Count: 2
- Critical Issues: 1
- High Issues: 4
- Medium Issues: 6
- Low Issues: 2
```

### Step 7: Next Actions

**If CRITICAL issues:**

```
⚠️ CRITICAL ISSUES FOUND

Must resolve before implementation:
1. Add API versioning to plan.md (Constitution violation)

Recommended actions:
- Run spec-kit-plan to adjust architecture
- Update tasks.md manually to add missing coverage
```

**If only LOW/MEDIUM:**

```
✅ READY TO IMPLEMENT (with improvements recommended)

Optional improvements:
- Clarify "fast response" threshold in spec.md
- Normalize terminology (User vs Account)

You may proceed with spec-kit-implement
```

### Step 8: Remediation Offer

After report:

```
Would you like me to suggest concrete remediation edits for the top 5 issues?
(I will NOT apply them automatically - you must approve first)
```

## Best Practices

✅ **DO:**

- Run after tasks.md creation
- Review all CRITICAL issues before implementation
- Use for pre-implementation validation
- Check constitution alignment

❌ **DON'T:**

- Modify files (this is read-only)
- Ignore CRITICAL issues
- Skip if constitution exists
- Run before tasks.md created

## Example Workflow

```
User: "Analyze my feature artifacts for consistency"

1. Check prerequisites
   ✓ spec.md exists
   ✓ plan.md exists
   ✓ tasks.md exists
   ✓ constitution.md exists

2. Load artifacts (progressive)
   - 5 functional requirements
   - 3 user stories
   - 25 tasks across 4 phases
   - 3 constitution principles

3. Build semantic models
   - Map 25 tasks to requirements
   - Extract entities from data model
   - Identify terminology

4. Detection passes
   Found:
   - 2 duplicate requirements
   - 3 ambiguous terms
   - 1 coverage gap
   - 1 constitution violation
   - 2 terminology drifts

5. Severity assignment
   - 1 CRITICAL (constitution)
   - 2 HIGH (duplication, coverage)
   - 4 MEDIUM (terminology, ambiguity)

6. Generate report
   [Full markdown table as shown above]

7. Next actions
   - CRITICAL: Add API versioning
   - Recommend: Merge duplicate requirements
   - Optional: Clarify ambiguous terms

8. Offer remediation
   "Would you like concrete edit suggestions?"
```

## Report Interpretation

**Coverage % Guidelines:**

- **100%**: Perfect coverage (rare)
- **80-99%**: Excellent (some optional requirements unmapped)
- **60-79%**: Good (review gaps)
- **< 60%**: Poor (significant gaps, review tasks.md)

**Issue Count Guidelines:**

- **0 CRITICAL**: Safe to proceed
- **1+ CRITICAL**: Must fix before implementation
- **< 5 HIGH**: Normal for first pass
- **5+ HIGH**: Consider re-planning
- **MEDIUM/LOW**: Informational, fix opportunistically

## Constitution Authority

**CRITICAL PRINCIPLE:**

Constitution violations are **ALWAYS CRITICAL** and **NON-NEGOTIABLE**.

✅ **Correct Response:**

```
CRITICAL: Constitution violation detected
Principle: "All data must be encrypted at rest"
Violation: plan.md specifies plaintext storage
Action: Update plan.md to add encryption
```

❌ **WRONG Response:**

```
Maybe we can relax the encryption principle for this feature?
```

**To change constitution**: Run separate constitution update (not part of analyze)

## Token Efficiency

**Progressive Disclosure:**

- Load artifacts incrementally
- Summarize long sections
- Focus on actionable findings
- Limit to 50 findings max

**Minimal Output:**

- Tables over prose
- Stable IDs for findings
- Aggregate overflow
- Concise recommendations

## Next Steps

After analysis:

- **Fix CRITICAL**: Update spec/plan/tasks
- **Start implementation**: Use `spec-kit-implement`
- **Re-analyze**: After fixes to verify

## Related Skills

- **spec-kit-tasks** - Creates tasks.md (prerequisite)
- **spec-kit-implement** - Executes implementation (next step)
- **spec-kit-plan** - Updates technical plan if needed

## Common Issues

**Missing tasks.md:**

```
Run spec-kit-tasks first to generate task list
```

**No issues found:**

```
✅ All artifacts consistent
- 100% coverage
- No ambiguities
- Constitution aligned
- Ready to implement
```

**Too many findings:**

```
50+ issues detected, showing top 50 by severity
Consider running spec-kit-clarify to reduce ambiguities
```

## Tips for Clean Analysis

1. **Run after each workflow step**: Catch issues early
2. **Fix CRITICAL first**: Constitution violations block progress
3. **Review coverage %**: Aim for 80%+ before implementation
4. **Check terminology**: Consistency prevents confusion
5. **Trust the report**: Deterministic results, re-run to verify

---

**Remember**: Analysis is read-only validation. Fix issues in source artifacts (spec/plan/tasks), then re-analyze to confirm.
