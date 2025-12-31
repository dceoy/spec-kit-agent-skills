---
name: spec-kit-checklist
description: Generate custom checklists to validate requirements quality. Creates "unit tests for requirements" - validates completeness, clarity, consistency, and measurability of specifications (NOT implementation testing). Use when need quality gates, PR review checklists, or requirements validation.
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Spec-Kit Checklist

Generate requirement quality validation checklists. **"Unit Tests for English"** - validates requirements are complete, clear, and consistent.

## Core Concept: Unit Tests for Requirements

**CRITICAL:** Checklists test **REQUIREMENTS QUALITY**, not implementation.

**❌ NOT for verification/testing:**

- "Verify button clicks correctly"
- "Test error handling works"
- "Confirm API returns 200"
- Checking if code matches spec

**✅ FOR requirements quality:**

- "Are visual hierarchy requirements defined for all card types?" [Completeness]
- "Is 'prominent display' quantified with specific sizing?" [Clarity]
- "Are hover state requirements consistent across interactive elements?" [Consistency]
- "Are accessibility requirements defined for keyboard navigation?" [Coverage]
- "Does spec define fallback when logo image fails to load?" [Edge Cases]

**Metaphor:** If your spec is code written in English, the checklist is its unit test suite.

## When to Use

- After creating spec.md or plan.md
- Before implementation to validate requirements
- For PR review quality gates
- User asks for "quality checklist" or "requirements validation"
- Need to ensure requirements are implementation-ready

## What It Does

1. **Asks clarifying questions** (1-5) to understand focus and depth
2. **Loads feature context** from spec/plan/tasks
3. **Generates checklist** testing requirements across quality dimensions
4. **Creates unique file** in FEATURE_DIR/checklists/
5. **Reports summary** with focus areas and item count

## Prerequisites

- spec.md should exist (created by `spec-kit-specify`)
- Run from repository root
- Optional: plan.md, tasks.md for additional context

## Basic Usage

### Step 1: Initialize

Run prerequisites check:

```bash
.specify/scripts/bash/check-prerequisites.sh --json
```

Provides:

- FEATURE_DIR path
- AVAILABLE_DOCS list

### Step 2: Clarifying Questions (Dynamic)

Asks 1-5 questions based on user request and feature context:

**Sample Q1 - Scope:**

```
Should this checklist include integration touchpoints with external APIs
or stay limited to local module correctness?

| Option | Candidate | Why It Matters |
|--------|-----------|----------------|
| A | Local only | Faster review, focused scope |
| B | Include integrations | Comprehensive coverage |
| C | Integration edge cases only | Balance depth and speed |
```

**Sample Q2 - Risk:**

```
Which risk areas should receive mandatory gating checks?

**Suggested:** Security + Performance - Based on NFR emphasis in spec

Options: Security, Performance, Accessibility, Data integrity, Scalability
```

**Sample Q3 - Depth:**

```
Is this a lightweight pre-commit sanity list or a formal release gate?

| Option | Candidate | Why It Matters |
|--------|-----------|----------------|
| A | Lightweight (15-25 items) | Quick PR reviews |
| B | Standard (25-40 items) | Balanced coverage |
| C | Comprehensive (40+ items) | Release gates |
```

**Sample Q4 - Audience:**

```
Who will use this checklist?

**Suggested:** PR Reviewer - Most common use case

| Option | Candidate | Why It Matters |
|--------|-----------|----------------|
| A | Author (self-review) | Completeness focus |
| B | PR Reviewer | Clarity and testability focus |
| C | QA Team | Acceptance criteria focus |
| D | Release Manager | Readiness and documentation |
```

**Sample Q5 - Exclusions:**

```
Should we explicitly exclude performance tuning items this round?

Answer: Yes/No (short answer)
```

**Question Limits:**

- Maximum 5 questions
- Skip if already clear from user request
- Stop if user says "done" or "skip"

### Step 3: Generate Checklist

Creates checklist file in `FEATURE_DIR/checklists/`:

**Filename Format:**

- `[domain].md` - e.g., `ux.md`, `api.md`, `security.md`
- Short, descriptive names
- Each run creates NEW file (never overwrites)

**Checklist Structure:**

```markdown
# [Type] Requirements Quality Checklist

**Purpose:** [Brief description of what this checklist validates]
**Created:** YYYY-MM-DD
**Focus:** [Derived from clarifying questions]

## Requirement Completeness

- [ ] CHK001 - Are error handling requirements defined for all API failure modes? [Gap]
- [ ] CHK002 - Are accessibility requirements specified for all interactive elements? [Completeness]
- [ ] CHK003 - Are mobile breakpoint requirements defined for responsive layouts? [Gap]

## Requirement Clarity

- [ ] CHK004 - Is "fast loading" quantified with specific timing thresholds? [Clarity, Spec §NFR-2]
- [ ] CHK005 - Are "related episodes" selection criteria explicitly defined? [Clarity, Spec §FR-5]
- [ ] CHK006 - Is "prominent" defined with measurable visual properties? [Ambiguity, Spec §FR-4]

## Requirement Consistency

- [ ] CHK007 - Do navigation requirements align across all pages? [Consistency, Spec §FR-10]
- [ ] CHK008 - Are card component requirements consistent between landing and detail pages? [Consistency]

## Acceptance Criteria Quality

- [ ] CHK009 - Are visual hierarchy requirements measurable/testable? [Acceptance Criteria, Spec §FR-1]
- [ ] CHK010 - Can "balanced visual weight" be objectively verified? [Measurability, Spec §FR-2]

## Scenario Coverage

- [ ] CHK011 - Are requirements defined for zero-state scenarios (no episodes)? [Coverage, Edge Case]
- [ ] CHK012 - Are concurrent user interaction scenarios addressed? [Coverage, Gap]
- [ ] CHK013 - Are requirements specified for partial data loading failures? [Coverage, Exception Flow]

## Edge Case Coverage

- [ ] CHK014 - Is fallback behavior specified when logo image fails to load? [Edge Case, Gap]
- [ ] CHK015 - Are loading states defined for asynchronous episode data? [Completeness]

## Non-Functional Requirements

- [ ] CHK016 - Are performance requirements quantified with specific metrics? [Clarity, Spec §NFR-1]
- [ ] CHK017 - Are performance targets defined for all critical user journeys? [Coverage]
- [ ] CHK018 - Can performance requirements be objectively measured? [Measurability]

## Dependencies & Assumptions

- [ ] CHK019 - Are external podcast API requirements documented? [Dependency, Gap]
- [ ] CHK020 - Is the assumption of "always available API" validated? [Assumption]

## Ambiguities & Conflicts

- [ ] CHK021 - Is the term "fast" quantified with specific metrics? [Ambiguity, Spec §NFR-1]
- [ ] CHK022 - Do navigation requirements conflict between §FR-10 and §FR-10a? [Conflict]
```

### Quality Dimensions

Every checklist item validates requirements across:

1. **Completeness**: Are all necessary requirements present?
2. **Clarity**: Are requirements unambiguous and specific?
3. **Consistency**: Do requirements align without conflicts?
4. **Measurability**: Can requirements be objectively verified?
5. **Coverage**: Are all scenarios/edge cases addressed?

### Writing Checklist Items

**Required Pattern:**

```
- [ ] CHK### - [Question about requirement quality]? [Dimension, Reference]
```

**Components:**

- Question format
- Focus on what's WRITTEN (or missing) in spec
- Quality dimension marker: [Completeness/Clarity/Consistency/Coverage/...]
- Traceability: [Spec §X.Y] or [Gap] or [Ambiguity] or [Conflict]

**✅ CORRECT Examples:**

```markdown
- [ ] CHK001 - Are the number and layout of featured episodes explicitly specified? [Completeness, Spec §FR-001]
- [ ] CHK002 - Are hover state requirements consistently defined for all interactive elements? [Consistency, Spec §FR-003]
- [ ] CHK003 - Are navigation requirements clear for all clickable brand elements? [Clarity, Spec §FR-010]
- [ ] CHK004 - Is the selection criteria for related episodes documented? [Gap, Spec §FR-005]
- [ ] CHK005 - Are loading state requirements defined for asynchronous episode data? [Gap]
- [ ] CHK006 - Can "visual hierarchy" requirements be objectively measured? [Measurability, Spec §FR-001]
```

**❌ WRONG Examples (these test implementation, not requirements):**

```markdown
- [ ] Verify landing page displays 3 episode cards
- [ ] Test hover states work correctly on desktop
- [ ] Confirm logo click navigates to home page
- [ ] Check related episodes section shows 3-5 items
```

### Scenario Classification

Check requirements exist for:

- **Primary scenarios**: Happy path requirements
- **Alternate scenarios**: Valid alternative flows
- **Exception scenarios**: Error handling requirements
- **Recovery scenarios**: Rollback/retry requirements
- **Non-functional scenarios**: Performance, security, accessibility requirements

For each:

```
- [ ] CHK### - Are [scenario type] requirements complete, clear, and consistent? [Coverage]
```

If missing:

```
- [ ] CHK### - Are [scenario type] requirements intentionally excluded or missing? [Gap]
```

### Traceability Requirements

**MINIMUM:** ≥80% of items MUST include traceability reference:

- `[Spec §X.Y]` - References spec section
- `[Plan §X.Y]` - References plan section
- `[Gap]` - Identifies missing requirement
- `[Ambiguity]` - Flags vague language
- `[Conflict]` - Notes contradiction
- `[Assumption]` - Unvalidated assumption

### Content Consolidation

**Soft cap:** 40 items

**If > 40 items:**

- Prioritize by risk/impact
- Merge near-duplicates
- Consolidate low-impact edge cases:
  ```
  - [ ] CHK### - Are edge cases X, Y, Z addressed in requirements? [Coverage]
  ```

## Best Practices

✅ **DO:**

- Test requirement quality, not implementation
- Use question format
- Include traceability (≥80%)
- Focus on completeness, clarity, consistency, measurability, coverage
- Reference spec sections
- Mark gaps and ambiguities

❌ **DON'T:**

- Write implementation verification steps
- Use "Verify", "Test", "Confirm" + behavior
- Check code execution
- Create test cases or QA procedures
- Reference implementation details

## Example Workflows

### UX Requirements Quality Checklist

```
User: "Create a UX requirements quality checklist"

Q1: Lightweight (15-25) or comprehensive (40+)?
User: "Standard"

Q2: Focus areas?
User: "Visual hierarchy, interactions, accessibility"

Generate ux.md:
- 30 items across 6 categories
- Focus on visual spec completeness
- Accessibility coverage validation
- Interaction state consistency
- Edge case requirements (loading, errors, empty states)

Output: .specify/features/podcast-landing/checklists/ux.md
```

### API Requirements Quality Checklist

```
User: "Create API requirements checklist for release gate"

Q1: Include integrations?
User: "Yes"

Q2: Risk areas?
User: "Security, performance"

Q3: Depth?
User: "Comprehensive"

Generate api.md:
- 45 items (comprehensive)
- Security requirements validation
- Performance criteria clarity
- Error response completeness
- Rate limiting specification
- Versioning strategy documentation

Output: .specify/features/podcast-api/checklists/api.md
```

## Common Checklist Types

**ux.md** - Visual/interaction requirements quality:

- Visual hierarchy requirements defined?
- Interaction states consistently specified?
- Accessibility requirements complete?
- Loading/error states documented?
- Responsive breakpoints clear?

**api.md** - API requirements quality:

- Error response formats specified?
- Rate limiting quantified?
- Authentication consistent?
- Versioning strategy documented?
- Timeout/retry requirements defined?

**security.md** - Security requirements quality:

- Authentication requirements specified?
- Data protection requirements defined?
- Threat model documented?
- Compliance requirements clear?
- Security failure responses defined?

**performance.md** - Performance requirements quality:

- Performance metrics quantified?
- Targets for all critical journeys?
- Load conditions specified?
- Requirements measurable?
- Degradation scenarios defined?

**test.md** - Test requirements quality:

- Acceptance criteria testable?
- Test scenarios for all flows?
- Edge case testing requirements?
- Non-functional testing defined?
- Success criteria measurable?

## Validation Metrics

**Good checklist:**

- ≥80% items with traceability
- All items question requirement quality
- No implementation verification steps
- Balanced across quality dimensions
- 15-40 items (focused scope)

**Poor checklist:**

- < 50% items with traceability
- Implementation testing mixed in
- "Verify X works" language
- Single dimension focus
- Too broad (50+ items) or narrow (< 10 items)

## Next Steps

After creating checklist:

- **Use for PR reviews**: Validate spec/plan before approval
- **Self-review**: Author validates own requirements
- **QA validation**: Ensure testable acceptance criteria
- **Implementation**: Use `spec-kit-implement` after validation

## Related Skills

- **spec-kit-specify** - Creates spec.md to validate
- **spec-kit-plan** - Creates plan.md to validate
- **spec-kit-analyze** - Automated consistency analysis
- **spec-kit-implement** - Implementation after validation

## Common Issues

**Checklist tests implementation:**

```
WRONG: "Verify login button works"
RIGHT: "Are button interaction requirements defined for all states?" [Completeness]
```

**Missing traceability:**

```
Add [Spec §X.Y] or [Gap] markers to 80%+ of items
```

**Too broad:**

```
Focus on 2-3 quality dimensions, not all 9
Use 15-40 items per checklist
```

## Tips for Quality Checklists

1. **Think "unit test"**: Test the requirements, not the product
2. **Question format**: "Are requirements X...?" not "Does system X...?"
3. **Add traceability**: Reference spec sections or mark gaps
4. **Focus scope**: One domain per checklist (UX, API, Security, etc.)
5. **Balance depth**: 15-40 items for focused, actionable validation

---

**Remember**: Checklists are unit tests for English. They validate requirement quality - completeness, clarity, consistency, measurability, and coverage - NOT whether code works.
