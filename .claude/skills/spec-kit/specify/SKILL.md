---
name: spec-kit-specify
description: Create or update feature specifications from natural language descriptions. Use when starting a new feature, user describes what they want to build, or needs to document requirements. Generates structured spec.md with user stories, acceptance criteria, and requirements.
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Spec-Kit Specify

Create structured feature specifications from natural language descriptions. This is the **first step** in the spec-kit workflow.

## When to Use

- User describes a new feature they want to build
- Starting a new project or feature
- Need to document requirements
- User says "I want to add..." or "Let's build..."
- Converting ideas into structured specifications

## What It Does

1. Generates concise branch name from feature description
2. Creates/updates spec.md with:
   - Feature overview
   - User stories with priorities
   - Acceptance criteria
   - Requirements (functional, non-functional, constraints)
3. Sets up feature directory structure
4. Creates git branch for the feature

## Prerequisites

Spec-kit should be initialized in the repository:

```bash
ls .specify/  # Check if spec-kit is set up
```

## Basic Usage

### Step 1: Get Feature Description

The feature description is what the user wants to build. Examples:

- "Add user authentication with email/password"
- "Create a dashboard with analytics"
- "Implement file upload with drag and drop"

### Step 2: Generate Specification

Run the specify script:

```bash
.specify/scripts/bash/setup-spec.sh --json
```

This creates:

- Feature branch
- spec.md with structured requirements
- Feature directory in .specify/features/

### Step 3: Fill spec.md Template

Generate comprehensive spec.md with:

**Feature Overview:**

- What: Brief description
- Why: Business value
- Who: Target users

**User Stories:**

```markdown
## User Stories

### P1 (Must Have)

**US1: [Story Title]**

- **As a** [user type]
- **I want** [goal/desire]
- **So that** [benefit/value]

**Acceptance Criteria:**

- [ ] Criterion 1
- [ ] Criterion 2
```

**Requirements:**

- Functional requirements
- Non-functional requirements (performance, security, UX)
- Technical constraints

## Output Format

The spec.md follows this structure:

```markdown
# Feature: [Name]

## Overview

[Brief description of the feature]

## User Stories

### Priority 1 (Must Have)

**US1: User Login**

- **As a** registered user
- **I want** to log in with email/password
- **So that** I can access my account

**Acceptance Criteria:**

- [ ] Email validation works
- [ ] Password is hashed
- [ ] Session management implemented

### Priority 2 (Should Have)

[Additional stories...]

## Requirements

### Functional

- FR1: System must validate email format
- FR2: Password must be 8+ characters

### Non-Functional

- NFR1: Login response time < 2 seconds
- NFR2: Passwords stored with bcrypt

### Constraints

- Must use existing authentication library
- Must comply with GDPR
```

## Best Practices

✅ **DO:**

- Write user stories from user perspective
- Include clear acceptance criteria
- Prioritize stories (P1, P2, P3)
- Define non-functional requirements
- Consider edge cases

❌ **DON'T:**

- Skip acceptance criteria
- Make stories too large
- Mix technical implementation with requirements
- Forget non-functional requirements

## Example Workflow

```
User: "I want to add user authentication with email/password and social login"

1. Generate branch name: "add-user-authentication"
2. Create spec.md with:
   - User Story 1: Email/Password Login (P1)
   - User Story 2: Social Login (P2)
   - User Story 3: Password Reset (P2)
   - Acceptance criteria for each
   - Security requirements
   - Performance requirements

3. Save to .specify/features/add-user-authentication/spec.md
```

## Next Steps

After creating spec.md:

- **Clarify** ambiguities with `spec-kit-clarify`
- **Plan** implementation with `spec-kit-plan`
- **Generate tasks** with `spec-kit-tasks`

## Related Skills

- **spec-kit-clarify** - Identify and resolve ambiguities
- **spec-kit-plan** - Create technical implementation plan
- **spec-kit-tasks** - Generate actionable task list

## Common Issues

**Feature description too vague:**

- Ask clarifying questions
- Identify key user goals
- Break into smaller features

**Too many features at once:**

- Focus on one feature per specification
- Create separate specs for distinct features

## Tips for Good Specifications

1. **User-Centric**: Write from user perspective
2. **Testable**: Acceptance criteria should be verifiable
3. **Prioritized**: Use P1, P2, P3 to indicate priority
4. **Complete**: Include functional, non-functional, and constraints
5. **Concise**: Each story should be independent and valuable

---

**Remember**: This is the first step. After specify, use `spec-kit-plan` to create the technical implementation plan.
