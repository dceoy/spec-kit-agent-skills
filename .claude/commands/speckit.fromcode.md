---
description: Generate feature specifications by analyzing existing source code.
handoffs:
  - label: Clarify Generated Spec
    agent: speckit.clarify
    prompt: Clarify specification requirements
    send: true
  - label: Create Implementation Plan
    agent: speckit.plan
    prompt: Create a plan for the spec. I am building with...
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

The text the user typed after `/speckit.fromcode` in the triggering message specifies the code to analyze. This can be file paths, directory paths, or glob patterns. Assume you always have it available in this conversation even if `$ARGUMENTS` appears literally below.

Given that code target, do this:

1. **Parse the target input**:
   - Accept file paths (e.g., `src/auth/login.ts`)
   - Accept directory paths (e.g., `src/auth/`)
   - Accept glob patterns (e.g., `src/**/*.ts`)
   - If empty: ERROR "No code target provided - specify files, directories, or patterns"

2. **Discover and read source files**:

   a. Expand the target to a list of files:
   - For glob patterns: Use file search to find matching files
   - For directories: List all relevant source files recursively
   - For single files: Read directly

   b. Read and analyze file contents:
   - Identify the primary programming language(s)
   - Detect frameworks and libraries in use
   - Map dependencies and relationships between files

3. **Analyze code structure**:
   - **Entry points**: Main functions, route handlers, event listeners
   - **Public interfaces**: Exported functions, API endpoints, UI components
   - **Data models**: Classes, types, schemas, database models
   - **Business logic**: Core algorithms, validation rules, state management
   - **User interactions**: Form handlers, command processors, UI events
   - **Error handling**: Exception patterns, fallback behaviors, validation errors

4. **Generate a concise short name** (2-4 words) from the analyzed code:
   - Analyze the primary functionality and extract meaningful keywords
   - Use action-noun format (e.g., "user-auth", "payment-processing", "report-generator")
   - Preserve technical terms where they describe the feature well
   - Examples:
     - Authentication logic → "user-auth"
     - E-commerce cart code → "shopping-cart"
     - Dashboard components → "analytics-dashboard"
     - API client code → "api-integration"

5. **Check for existing branches before creating new one**:

   a. First, fetch all remote branches to ensure we have the latest information:

   ```bash
   git fetch --all --prune
   ```

   b. Find the highest feature number across all sources for the short-name:
   - Remote branches: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
   - Local branches: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
   - Specs directories: Check for directories matching `specs/[0-9]+-<short-name>`

   c. Determine the next available number:
   - Extract all numbers from all three sources
   - Find the highest number N
   - Use N+1 for the new branch number

   d. Run the script `.specify/scripts/bash/create-new-feature.sh --json` with the calculated number and short-name:
   - Pass `--number N+1` and `--short-name "your-short-name"` along with a description
   - Example: `.specify/scripts/bash/create-new-feature.sh --json --number 5 --short-name "user-auth" "User authentication (generated from code)"`

   **IMPORTANT**:
   - The JSON output will contain BRANCH_NAME and SPEC_FILE paths
   - You must only ever run this script once per feature

6. **Load spec template** from `.specify/templates/spec-template.md` to understand required sections.

7. **Generate specification content by analyzing code**:

   a. **User Scenarios & Testing**:
   - Infer user stories from user-facing code paths
   - Look for: route handlers, UI components, form submissions, API endpoints
   - Convert code flows to "As a [user], I can [action] so that [benefit]" format
   - Derive acceptance scenarios from:
     - Test files (unit tests, integration tests)
     - Validation logic and error messages
     - Edge case handling in the code

   b. **Functional Requirements**:
   - Extract from business logic, validation rules, and constraints
   - Convert conditionals to requirements: `if (x > 10)` → "Value MUST be greater than 10"
   - Identify mandatory vs optional behaviors from code paths
   - Look for: guards, assertions, input validation, permission checks

   c. **Key Entities**:
   - Identify from data models, classes, types, database schemas
   - Document relationships between entities
   - Extract key attributes and their purposes

   d. **Success Criteria**:
   - Infer from existing metrics, logging, or performance code
   - Look for: timing measurements, success/failure counts, user completion tracking
   - Convert to measurable, technology-agnostic outcomes

   e. **Assumptions**:
   - Document all inferences made during analysis
   - Note where code behavior was ambiguous
   - Flag areas that need domain expert review

8. **Abstract implementation details**:

   Transform technical patterns to user-focused language:

   | Code Pattern | Spec Requirement |
   |--------------|------------------|
   | `if (user.role === 'admin')` | System MUST restrict action to administrator users |
   | `password.length >= 8` | Passwords MUST be at least 8 characters |
   | `setTimeout(fn, 5000)` | System MUST process within reasonable time |
   | `try { } catch { notify() }` | System MUST notify users when errors occur |
   | `localStorage.setItem()` | System MUST persist user preferences |
   | `fetch('/api/users')` | System MUST retrieve user information |

   **Guidelines**:
   - Remove framework-specific terminology (React, Express, Django, etc.)
   - Focus on WHAT the code does, not HOW it does it
   - Express behaviors in technology-agnostic terms
   - Preserve business rules and domain concepts

9. **Handle unclear code sections**:
   - Mark with `[NEEDS CLARIFICATION: specific question]` if:
     - Code behavior is ambiguous or confusing
     - Multiple interpretations are possible
     - Business logic seems incomplete or inconsistent
   - **LIMIT: Maximum 3 [NEEDS CLARIFICATION] markers total**
   - Prioritize: scope-impacting > security > UX > technical details

10. **Write the specification** to SPEC_FILE using the template structure.

11. **Create spec quality checklist** at `FEATURE_DIR/checklists/requirements.md`:

    ```markdown
    # Specification Quality Checklist: [FEATURE NAME]

    **Purpose**: Validate generated specification accuracy and completeness
    **Created**: [DATE]
    **Feature**: [Link to spec.md]
    **Source**: Generated from code analysis

    ## Abstraction Quality

    - [ ] No implementation details (languages, frameworks, APIs)
    - [ ] Technical patterns converted to user-focused requirements
    - [ ] Framework-specific terms removed
    - [ ] All mandatory sections completed

    ## Accuracy Review

    - [ ] User stories accurately reflect code functionality
    - [ ] Requirements match actual code behavior
    - [ ] Edge cases from code are captured
    - [ ] Entities match data models in code

    ## Completeness Check

    - [ ] All major code paths represented
    - [ ] Error handling behaviors documented
    - [ ] Success criteria are measurable
    - [ ] Assumptions clearly documented

    ## Domain Expert Review Needed

    - [ ] Business rules need validation
    - [ ] User stories need stakeholder review
    - [ ] Success criteria need business input

    ## Notes

    - This spec was auto-generated from code analysis
    - Items marked incomplete require expert review before `/speckit.plan`
    ```

12. **Report completion** with:
    - Branch name and spec file path
    - Summary of files analyzed (count and types)
    - Key features/functionality discovered
    - Number of user stories generated
    - Areas flagged for clarification or review
    - Recommendation for next steps (clarify vs plan)

## Guidelines for Code Analysis

### What to Look For

**User-facing functionality**:
- Route handlers and API endpoints
- Form submissions and input processing
- UI event handlers and interactions
- Command-line argument processing

**Business logic**:
- Validation rules and constraints
- Permission and access control checks
- State transitions and workflows
- Calculations and transformations

**Data operations**:
- Database queries and mutations
- Data model definitions
- Relationship mappings
- Migration files

**Error handling**:
- Try-catch blocks
- Error messages and codes
- Fallback behaviors
- Validation error responses

### What to Ignore (Implementation Details)

- Specific framework APIs and patterns
- Database query syntax
- Caching mechanisms
- Logging implementations
- Build and bundling configuration
- Development tooling

### Language-Specific Hints

**JavaScript/TypeScript**:
- Look for `export` statements for public interfaces
- Check `package.json` for feature hints
- Review test files (`*.test.ts`, `*.spec.ts`)

**Python**:
- Look for decorated functions (`@app.route`, `@api_view`)
- Check `requirements.txt` for framework hints
- Review test files (`test_*.py`)

**Go**:
- Look for exported functions (capitalized names)
- Check handler functions for HTTP endpoints
- Review `*_test.go` files

**Java/Kotlin**:
- Look for `@Controller`, `@Service`, `@Entity` annotations
- Check public methods on service classes
- Review test directories

## Example Output Summary

```
## Specification Generated

**Branch**: `3-user-auth`
**Spec File**: `specs/3-user-auth/spec.md`

### Files Analyzed
- 12 TypeScript files in `src/auth/`
- 4 test files in `tests/auth/`
- 2 migration files

### Features Discovered
1. User registration with email verification
2. Password-based authentication with rate limiting
3. OAuth2 integration (Google, GitHub)
4. Session management with refresh tokens
5. Password reset flow

### User Stories Generated: 5 (P1: 2, P2: 2, P3: 1)

### Areas for Review
- [NEEDS CLARIFICATION]: Session timeout duration (found 2 different values)
- Assumption: Email verification is mandatory (based on code flow)

### Next Steps
Recommend `/speckit.clarify` to validate generated requirements with domain expert.
```
