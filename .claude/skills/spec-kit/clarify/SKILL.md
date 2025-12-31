---
name: spec-kit-clarify
description: Identify underspecified areas in the current feature spec by asking up to 5 highly targeted clarification questions and encoding answers back into the spec. Use when spec has ambiguities, vague requirements, or missing decisions. Best used after spec-kit-specify and before spec-kit-plan.
allowed-tools: Bash, Read, Write, Edit, Grep, Glob
---

# Spec-Kit Clarify

Detect and reduce ambiguity in feature specifications through targeted questioning. This is an **optional step** between spec-kit-specify and spec-kit-plan.

## When to Use

- After creating spec.md with `spec-kit-specify`
- Spec contains vague terms ("fast", "intuitive", "robust")
- Missing decision points or placeholders (TODO, ???)
- User asks to "clarify requirements" or "resolve ambiguities"
- Before planning when requirements seem incomplete

## What It Does

1. **Scans spec.md** for ambiguities across 11 categories
2. **Asks up to 5 targeted questions** one at a time
3. **Provides recommendations** for each question based on best practices
4. **Integrates answers** into spec incrementally after each response
5. **Creates clarifications log** in spec.md with session timestamp

## Prerequisites

- spec.md must exist (created by `spec-kit-specify`)
- Run from repository root
- Spec-kit initialized (.specify/ directory exists)

## Basic Usage

### Step 1: Load Specification

Run prerequisites check:

```bash
.specify/scripts/bash/check-prerequisites.sh --json --paths-only
```

Provides:

- FEATURE_DIR path
- FEATURE_SPEC path

### Step 2: Ambiguity Scan

Scans spec across these categories:

**Functional Scope & Behavior:**

- Core user goals & success criteria
- Explicit out-of-scope declarations
- User roles / personas

**Domain & Data Model:**

- Entities, attributes, relationships
- Data volume / scale assumptions
- Lifecycle states

**Interaction & UX:**

- Critical user journeys
- Error/empty/loading states
- Accessibility requirements

**Non-Functional:**

- Performance (latency, throughput)
- Scalability limits
- Security & privacy
- Observability signals

**Integration:**

- External services/APIs
- Failure modes
- Data formats

**Edge Cases:**

- Negative scenarios
- Rate limiting
- Conflict resolution

**Constraints:**

- Technical constraints
- Tradeoffs / rejected alternatives

**Terminology:**

- Canonical terms
- Avoided synonyms

**Completion Signals:**

- Testable acceptance criteria
- Definition of Done

**Placeholders:**

- TODO markers
- Vague adjectives

### Step 3: Targeted Questioning

**Question Format:**

Multiple-choice questions:

```
**Recommended:** Option B - JWT tokens for stateless auth

| Option | Description |
|--------|-------------|
| A | Cookie-based sessions (simpler, more secure) |
| B | JWT tokens (stateless, scalable) |
| C | OAuth only (delegate to provider) |

You can reply with the option letter (e.g., "B"), accept the recommendation
by saying "yes" or "recommended", or provide your own short answer.
```

Short-answer questions:

```
**Suggested:** < 2 seconds - Industry standard for interactive responses

Format: Short answer (<=5 words). You can accept the suggestion by saying
"yes" or "suggested", or provide your own answer.
```

**Question Rules:**

- **Maximum 5 questions** per session
- One question at a time (never batch)
- Only high-impact questions (affects architecture, data model, testing, UX, security)
- Skip if already answered in spec
- User can say "yes"/"recommended"/"suggested" to accept recommendation

### Step 4: Incremental Integration

After EACH accepted answer:

1. **Create/update Clarifications section** in spec.md:

   ```markdown
   ## Clarifications

   ### Session 2025-01-15

   - Q: What authentication method? → A: JWT tokens
   ```

2. **Apply to appropriate section:**
   - Functional ambiguity → Functional Requirements
   - User interaction → User Stories or Actors
   - Data shape → Data Model
   - Non-functional → Quality Attributes
   - Edge case → Error Handling
   - Terminology → Normalize across spec

3. **Save immediately** after each integration (atomic updates)

### Step 5: Coverage Report

After questioning completes:

**Completion Summary:**

```
Clarification Summary:
- Questions asked: 3
- Sections updated: Functional Requirements, Data Model, NFRs
- Updated spec: .specify/features/user-auth/spec.md

Coverage Table:
| Category | Status |
|----------|--------|
| Functional Scope | Resolved |
| Data Model | Resolved |
| Non-Functional | Resolved |
| Edge Cases | Deferred (low impact) |
| Integration | Clear (already specified) |

Next: Run spec-kit-plan to create technical implementation plan
```

## Question Prioritization

Questions selected by **(Impact × Uncertainty)** heuristic:

**High Impact:**

- Security/auth approach
- Data model changes
- API contracts
- Performance requirements
- Acceptance criteria

**High Uncertainty:**

- Vague adjectives ("fast", "secure")
- Multiple valid approaches
- Missing constraints
- Undefined terms

**Low Priority (skipped):**

- Stylistic preferences
- Implementation details
- Already answered

## Early Termination

User can stop at any time:

- "stop" - End questioning, save progress
- "done" - End questioning, proceed
- "proceed" - Skip remaining questions

## Best Practices

✅ **DO:**

- Ask one question at a time
- Provide recommendations based on best practices
- Accept "yes" to use recommendation
- Integrate answers immediately
- Focus on high-impact ambiguities

❌ **DON'T:**

- Ask more than 5 questions
- Batch multiple questions
- Ask about implementation details
- Skip integration after answers
- Make assumptions without asking

## Example Workflow

```
User: "Run spec-kit-clarify on my auth feature"

1. Load spec.md from .specify/features/add-auth/

2. Scan finds ambiguities:
   - "Secure password storage" (vague)
   - Missing session duration
   - Undefined "fast login"
   - No recovery flow

3. Ask Q1 (highest impact):
   **Recommended:** bcrypt with cost factor 12

   Q: What password hashing algorithm?

   | Option | Description |
   |--------|-------------|
   | A | bcrypt (industry standard) |
   | B | argon2 (modern, more secure) |
   | C | PBKDF2 (NIST approved) |

   User: "yes"

   → Integrate: Add "bcrypt with cost 12" to Security Requirements
   → Save spec.md

4. Ask Q2:
   **Suggested:** 7 days - Balance security and UX

   Q: Session duration?
   Format: Short answer (<=5 words)

   User: "30 days"

   → Integrate: Add session duration to Functional Requirements
   → Save spec.md

5. Ask Q3:
   Q: Define "fast login" with measurable criteria?

   User: "< 2 seconds"

   → Integrate: Update NFRs with "Login < 2s p95"
   → Save spec.md

6. Report completion:
   - 3 questions asked
   - spec.md updated with clarifications
   - Recommend proceeding to spec-kit-plan
```

## Integration Details

**Clarifications Section Format:**

```markdown
## Clarifications

### Session 2025-01-15

- Q: Password hashing algorithm? → A: bcrypt with cost 12
- Q: Session duration? → A: 30 days
- Q: "Fast login" threshold? → A: < 2 seconds p95
```

**Section Updates:**

- Replaces vague terms with specific values
- Adds missing constraints
- Normalizes terminology
- Removes contradictions
- Preserves existing structure

## Validation

After each write:

- One bullet per answer in Clarifications
- No duplicate entries
- No contradictory statements
- Updated sections clear and specific
- Valid markdown structure

## Next Steps

After clarification:

- **Plan** with `spec-kit-plan` - Create technical design
- **Re-clarify** if new ambiguities emerge during planning
- **Skip to tasks** if urgent (increases rework risk)

## Related Skills

- **spec-kit-specify** - Creates spec.md (prerequisite)
- **spec-kit-plan** - Technical planning (next step)
- **spec-kit-analyze** - Validates consistency

## Common Issues

**No ambiguities found:**

```
"No critical ambiguities detected. Proceed to spec-kit-plan."
```

**Missing spec.md:**

```
Run spec-kit-specify first to create feature specification
```

**Too many questions:**

```
Limited to 5 questions maximum per session
Run clarify multiple times if needed
```

## Tips for Effective Clarification

1. **Trust recommendations**: Based on industry best practices
2. **Say "yes" to save time**: Recommendations are usually optimal
3. **Be specific**: Provide measurable answers (numbers, thresholds)
4. **Early termination OK**: Can stop and resume later
5. **Run multiple times**: Each session can address different categories

---

**Remember**: Clarification before planning prevents costly rework. Five targeted questions can save hours of implementation time.
