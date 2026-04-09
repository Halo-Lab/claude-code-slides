# /plan - Create an Implementation Plan

Turn a technical specification (or idea) into a phased implementation plan.

This command has two modes:
1. **With spec**: Turn existing spec into implementation phases
2. **Without spec**: Discovery & scoping first, then create plan

**Note**: For quick ideation without creating a plan, use `/brainstorm` instead.

---

## Part 1: Discovery & Brainstorming

**If no spec exists, or requirements are unclear, start here.**

### Step 1: Understand the Request

Before asking questions, research first:
- Read existing code, docs, recent commits
- Understand what exists and what patterns are used
- Identify specific gaps you can't close yourself

Then share what you found: "Based on exploring X and Y, here's what I understand..."

### Step 2: Clarify Through Questions

Ask questions to fill gaps. Use appropriate format:

**Multiple choice** (when you have 2-3 real options):
> "For authentication, I see three approaches:
> A) JWT tokens (simpler, stateless)
> B) Session-based (more control, server state)
> C) OAuth only (delegate to provider)
> I recommend A because [reason]. Preference?"

**Open-ended** (when validating understanding):
> "Let me confirm the core requirement: [your understanding]. Correct?"

**Stop asking when you can articulate:**
- The user's goal
- The core technical challenge
- What success looks like
- What existing code/patterns to build on

### Step 3: Explore Alternatives

Before settling on an approach, propose 2-3 alternatives:

```markdown
## Approach Options

### Option A: [Name]
**Architecture**: [Brief description]
**Trade-offs**:
- ✅ [Benefit 1]
- ✅ [Benefit 2]
- ❌ [Drawback 1]
**Complexity**: Low/Medium/High
**Risk**: Low/Medium/High

### Option B: [Name]
**Architecture**: [Brief description]
**Trade-offs**:
- ✅ [Benefit 1]
- ❌ [Drawback 1]
- ❌ [Drawback 2]
**Complexity**: Low/Medium/High
**Risk**: Low/Medium/High

### Recommendation
Option [X] because [reasoning based on project context].
```

### Step 4: Scope Definition

Once approach is agreed, define scope explicitly:

```markdown
## Scope

### In Scope (Must Have)
- Feature 1: [description]
- Feature 2: [description]

### Out of Scope (Parking Lot)
- Future idea 1: [why not now]
- Future idea 2: [why not now]

### Non-Goals
- [What this explicitly will NOT do]
- [What this explicitly will NOT do]

### Assumptions
| Assumption | Rationale | Impact if Wrong |
|------------|-----------|-----------------|
| [Assumption] | [Why assumed] | [What breaks] |
```

---

## Part 2: Plan Structure

### Core Principles

- **Stoppable phases**: Can pause after any phase with working software
- **Close feedback loops fast**: Something working you can see/test early
- **Modular and minimal**: No extra complexity beyond requirements
- **Exact paths**: Specify which files to create/modify
- **Observable success**: Each phase has clear "done" criteria

### Phase Template

```markdown
## Phase N: [Name]

### Goal
What we're building and why it matters.

### Files
- Create: `path/to/new/file.ts`
- Modify: `path/to/existing/file.ts`

### Dependencies
- **Depends on**: Phase X (what from that phase)
- **Blocks**: Phase Y (what this enables)
- **Can parallel with**: Phase Z (if independent)

### Risk Assessment
**Risk Level**: 🟢 Low | 🟡 Medium | 🔴 High
**Risk Factors**:
- [Potential issue 1]
- [Potential issue 2]
**Mitigation**:
- [How to reduce risk]

### Done Means
- [ ] [Specific, observable criterion 1]
- [ ] [Specific, observable criterion 2]
- [ ] Tests pass: `[test command]`

### Test It
1. [Exact step to test]
2. [Expected result]
3. [How to verify]
```

### Standard Phase Structure

```markdown
## Phase 0: Prerequisites & Verification
Verify environment, install dependencies, confirm existing code works.
**Always start here** - never assume the foundation is solid.

## Phase 1-N: Implementation Phases
Each phase builds something working and testable.
Order by: dependencies first, then highest value, then lowest risk.

## Final Phase: Polish & Documentation
Only if explicitly requested. Usually folded into each phase.
```

---

## Part 3: Dependency Mapping

### Visualize Dependencies

```markdown
## Dependency Graph

Phase 0 (Setup)
    ↓
Phase 1 (Database) ←──────┐
    ↓                     │
Phase 2 (API) ───────→ Phase 3 (Auth)
    ↓                     │
Phase 4 (UI) ←────────────┘
    ↓
Phase 5 (Integration)
```

### Dependency Table

| Phase | Depends On | Blocks | Can Parallel With |
|-------|------------|--------|-------------------|
| 0 | - | 1,2,3,4,5 | - |
| 1 | 0 | 2,3 | - |
| 2 | 1 | 4,5 | 3 |
| 3 | 1 | 4,5 | 2 |
| 4 | 2,3 | 5 | - |
| 5 | 4 | - | - |

This enables:
- **Safe skipping**: Know what breaks if you skip a phase
- **Parallel work**: Multiple agents/sessions can work simultaneously
- **Recovery**: Know where to restart if interrupted

---

## Part 4: Risk Assessment

### Per-Phase Risk Factors

| Risk Factor | Indicators | Mitigation |
|-------------|------------|------------|
| External dependencies | APIs, services | Mock first, integrate later |
| New technology | Unfamiliar stack | Spike/prototype first |
| Complex logic | Many edge cases | TDD, extensive tests |
| Data migration | Existing data | Backup, rollback plan |
| Security sensitive | Auth, payments | Review, audit, test modes |

### Overall Plan Risk

```markdown
## Risk Summary

**Overall Risk**: 🟡 Medium

### High-Risk Phases
- Phase 3 (Auth): External OAuth dependency
- Phase 5 (Payment): PCI compliance requirements

### Mitigation Strategy
1. Spike OAuth integration before Phase 3
2. Use Stripe test mode throughout development
3. Security review checkpoint after Phase 3
```

---

## Part 5: Checkpoint Criteria

### Between Every Phase

```markdown
## Phase Checkpoint (Required)

Before proceeding to next phase:

### 1. Verification (show output)
- [ ] Tests pass: `[command]` → "X/X pass"
- [ ] Linter clean: `[command]` → "0 errors"
- [ ] Build works: `[command]` → "exit 0"

### 2. "Done Means" Criteria
- [ ] All criteria from phase marked complete
- [ ] Evidence shown (not just claimed)

### 3. Documentation
- [ ] README status updated
- [ ] TESTING.md has new test steps

### 4. User Confirmation
- [ ] User tested manually
- [ ] User confirmed "proceed to next phase"

### 5. Commit
- [ ] Clean commit with phase summary
- [ ] No debug code or TODOs left
```

**STOP if any checkpoint fails** - fix before proceeding.

---

## Part 6: Task Granularity

Break phases into bite-sized tasks (2-5 minutes each):

```markdown
### Phase 2 Tasks

- [ ] 2.1 Write failing test for user creation
- [ ] 2.2 Run test, verify it fails
- [ ] 2.3 Implement User model
- [ ] 2.4 Run test, verify it passes
- [ ] 2.5 Write failing test for user validation
- [ ] 2.6 Implement validation logic
- [ ] 2.7 Run all tests, verify pass
- [ ] 2.8 Update README with user model docs
- [ ] 2.9 Commit: "feat: add User model with validation"
```

Each task should have:
- **Exact file paths**: `src/models/user.ts` not "the model file"
- **Exact commands**: `npm test src/user.test.ts`
- **Expected output**: "PASS with 'user created'"

---

## Part 7: Writing for AI Agents

### Assumptions About Executor

- Skilled developer with zero context on your codebase
- Needs: which files, what to build, how to test, what success looks like
- Doesn't need: micro-management of how to code

### What to Include

- **What** to accomplish (specific requirements)
- **Where** to make changes (exact file paths)
- **How to verify** (test commands, expected output)
- **What success looks like** (observable criteria)

### What NOT to Include

- Code snippets (agent will write code)
- Bash commands for implementation (agent decides)
- Detailed "how to code it" instructions

---

## Execution Flow

When `/plan` is invoked:

### If Requirements Are Clear (Spec Exists)
1. Read the spec
2. Skip to Part 2: Create phased plan
3. Include dependency mapping and risk assessment
4. Add checkpoint criteria

### If Requirements Are Unclear (No Spec)
1. Start with Part 1: Discovery & Brainstorming
2. Clarify through questions
3. Define scope and assumptions
4. Then create phased plan

### Output Location

Save plan to: `PLAN.md` or location specified by user

### Plan Document Structure

```markdown
# Implementation Plan: [Project/Feature Name]

**Created**: YYYY-MM-DD
**Spec**: [link or "defined below"]
**Status**: Draft | Approved | In Progress | Complete

## Overview
[1-2 sentence summary of what we're building]

## Scope
[In scope / Out of scope / Assumptions]

## Approach
[Which option was chosen and why]

## Dependencies
[Dependency graph and table]

## Risk Summary
[Overall risk and high-risk phases]

## Phases

### Phase 0: Setup
[...]

### Phase 1: [Name]
[...]

## Checkpoint Protocol
[Standard checkpoint criteria]
```

---

## Style Guide

- **Succinct**: No fluff, every word counts
- **Specific**: "src/auth/login.ts" not "the login file"
- **Active voice**: "Create user model" not "User model should be created"
- **Trust the agent**: Tell what to accomplish, not how to code it
- **Stoppable**: Each phase is self-sufficient
- **Visual**: Include dependency graphs where helpful
