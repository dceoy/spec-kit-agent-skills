---
name: spec-kit-plan
description: Create technical implementation plan from feature specification. Use after creating spec.md to generate detailed design artifacts including architecture, data models, API contracts, and technical decisions. Second step in spec-kit workflow.
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Spec-Kit Plan

Generate comprehensive technical implementation plan from feature specification. This is the **second step** in spec-kit workflow.

## When to Use

- After creating spec.md with `spec-kit-specify`
- Need technical architecture and design
- Ready to plan implementation details
- User asks "how should we implement this?"

## What It Does

Creates plan.md with:

1. **Technical Context**: Tech stack, dependencies, integrations
2. **Architecture**: Design patterns, component structure
3. **Data Model**: Entities, relationships, validation
4. **API Contracts**: Endpoints, request/response formats
5. **Technical Decisions**: Technology choices with rationale

## Prerequisites

- spec.md must exist (created by `spec-kit-specify`)
- Run from repository root
- Spec-kit initialized (.specify/ directory exists)

## Basic Usage

### Step 1: Load Specification

Read the existing spec.md:

```bash
.specify/scripts/bash/setup-plan.sh --json
```

This provides:

- FEATURE_SPEC path
- IMPL_PLAN path
- SPECS_DIR location

### Step 2: Execute Planning Workflow

Follow structured phases:

**Phase 0: Research & Outline**

- Extract unknowns from requirements
- Research best practices
- Document technical decisions

**Phase 1: Design & Contracts**

- Define data model
- Design API contracts
- Create quickstart guide

## Output Format

### plan.md Structure

```markdown
# Implementation Plan: [Feature Name]

## Technical Context

### Tech Stack

- Frontend: React 18 + TypeScript + Vite
- Backend: Node.js + Express
- Database: PostgreSQL
- Authentication: JWT

### Dependencies

- New: bcrypt, jsonwebtoken
- Existing: express, pg

### Integrations

- Email service (SendGrid)
- OAuth providers (Google, GitHub)

## Architecture

### Design Patterns

- Repository pattern for data access
- Middleware for authentication
- Service layer for business logic

### Component Structure
```

src/
├── components/
│ ├── auth/
│ │ ├── LoginForm.tsx
│ │ └── RegisterForm.tsx
├── services/
│ ├── authService.ts
│ └── userService.ts
└── middleware/
└── auth.ts

````

## Data Model

### User Entity
```typescript
interface User {
  id: string
  email: string
  passwordHash: string
  createdAt: Date
  lastLogin: Date?
}
````

### Relationships

- User 1:N Session
- User 1:N RefreshToken

## API Contracts

### POST /api/auth/register

**Request:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Response (201):**

```json
{
  "user": { "id": "123", "email": "user@example.com" },
  "token": "jwt-token"
}
```

## Technical Decisions

### Decision: JWT vs Session

**Chosen:** JWT
**Rationale:** Stateless, scalable, works with mobile apps
**Alternatives:** Cookie sessions (simpler, more secure)

```

## Phase Workflow

### Phase 0: Research

1. **Identify unknowns** from spec.md
2. **Research each unknown**:
   - Best practices
   - Libraries/frameworks
   - Security considerations
3. **Document in research.md**

### Phase 1: Design

1. **Data Model** (data-model.md):
   - Extract entities from user stories
   - Define fields and types
   - Specify relationships
   - Add validation rules

2. **API Contracts** (contracts/):
   - Map user actions to endpoints
   - Define request/response schemas
   - Document error responses

3. **Quickstart** (quickstart.md):
   - Test scenarios
   - Setup instructions
   - Example API calls

## Best Practices

✅ **DO:**
- Research unknowns thoroughly
- Document decision rationale
- Consider alternatives
- Design for testability
- Follow project conventions

❌ **DON'T:**
- Skip research phase
- Make undocumented decisions
- Over-engineer solutions
- Ignore security implications
- Forget error handling

## Example Workflow

```

Input: spec.md with user authentication stories

Phase 0 - Research:
→ Research JWT libraries (jsonwebtoken)
→ Research password hashing (bcrypt)
→ Document in research.md

Phase 1 - Design:
→ Create data-model.md:

- User entity
- Session entity
- RefreshToken entity

→ Create contracts/:

- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/refresh
- POST /api/auth/logout

→ Create quickstart.md:

- Registration test
- Login test
- Protected route test

Output: Complete plan.md with all design artifacts

```

## Constitution Check

The plan includes a constitution check:
- Read .specify/memory/constitution.md
- Verify plan aligns with project principles
- Flag violations with justification

## Next Steps

After creating plan.md:
- **Generate tasks** with `spec-kit-tasks`
- **Analyze** for consistency with `spec-kit-analyze`
- Start implementation with `spec-kit-implement`

## Related Skills

- **spec-kit-specify** - Creates the spec.md (prerequisite)
- **spec-kit-tasks** - Generates tasks from plan (next step)
- **spec-kit-clarify** - Resolves ambiguities
- **spec-kit-analyze** - Checks consistency

## Common Issues

**Missing spec.md:**
```

Run spec-kit-specify first to create the feature specification

```

**Unclear requirements:**
```

Use spec-kit-clarify to resolve ambiguities before planning

```

**Too broad:**
```

Break into smaller features and plan separately

```

## Tips for Good Plans

1. **Complete**: Cover all aspects (data, API, UI, tests)
2. **Justified**: Explain technology choices
3. **Testable**: Include test scenarios
4. **Realistic**: Consider existing codebase
5. **Secure**: Think about security from the start

---

**Remember**: Planning before coding saves time. After plan, use `spec-kit-tasks` to create actionable task list.
```
