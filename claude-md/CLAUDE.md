# Development Guidelines

## Philosophy

### Core Beliefs

- **Incremental progress over big bangs** - Small changes that compile and pass tests
- **Learning from existing code** - Study and plan before implementing
- **Pragmatic over dogmatic** - Adapt to project reality
- **Clear intent over clever code** - Be boring and obvious

### Simplicity Means

- Single responsibility per function/class
- Avoid premature abstractions
- No clever tricks - choose the boring solution
- If you need to explain it, it's too complex

## Process

### Three-Phase Workflow

**CRITICAL: Never write code until the user has reviewed and approved a written plan.**

#### Phase 1: Research

When starting any non-trivial task, deeply analyze the relevant code sections first.

- Read all related files thoroughly - not surface-level skimming
- Document findings in `RESEARCH.md` in the project root (persistent file, not chat)
- Include: existing patterns, dependencies, edge cases, relevant tests
- Use language like "deeply analyze" and "in great detail" to ensure thorough reading
- Delete `RESEARCH.md` when the task is complete

#### Phase 2: Planning

After research, create a detailed implementation plan before writing any code.

- Document the plan in `IMPLEMENTATION_PLAN.md` in the project root
- Include: specific file paths, code snippets, order of operations, test strategy
- Break complex work into stages:

```markdown
## Stage N: [Name]
**Goal**: [Specific deliverable]
**Files**: [Specific file paths to modify/create]
**Approach**: [Code snippets and detailed steps]
**Tests**: [Specific test cases]
**Status**: [Not Started|In Progress|Complete]
```

- **Wait for user review before implementing** - present the plan and ask for approval
- The user may annotate the plan with corrections, constraints, and domain knowledge
- Update the plan based on annotations - do NOT implement yet until explicitly told
- This annotation cycle may repeat 1-6 times - this is expected and valuable
- Delete `IMPLEMENTATION_PLAN.md` when all stages are done

#### Phase 3: Implementation

Only after explicit plan approval, implement in a single comprehensive pass.

- Follow the approved plan precisely
- Continuously run typecheck/linter/tests during implementation
- If something doesn't work as planned, stop and report back rather than improvising
- If a reversion is needed, narrow the scope rather than patching a bad approach
- Keep corrections terse and focused during implementation

### Subagent Strategy

- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

### Self-Improvement Loop

- After ANY correction from the user: update `tasks/lessons.md` with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project

### Implementation Flow (per stage)

1. **Test** - Write test first (red)
2. **Implement** - Minimal code to pass (green)
3. **Refactor** - Clean up with tests passing
4. **Commit** - With clear message linking to plan

### When Stuck (After 3 Attempts)

**CRITICAL**: Maximum 3 attempts per issue, then STOP.

1. **Document what failed**:
   - What you tried
   - Specific error messages
   - Why you think it failed

2. **Research alternatives**:
   - Find 2-3 similar implementations
   - Note different approaches used

3. **Question fundamentals**:
   - Is this the right abstraction level?
   - Can this be split into smaller problems?
   - Is there a simpler approach entirely?

4. **Try different angle**:
   - Different library/framework feature?
   - Different architectural pattern?
   - Remove abstraction instead of adding?

## Technical Standards

### Architecture Principles

- **Composition over inheritance** - Use dependency injection
- **Interfaces over singletons** - Enable testing and flexibility
- **Explicit over implicit** - Clear data flow and dependencies
- **Test-driven when possible** - Never disable tests, fix them

### Code Quality

- **Every commit must**:
  - Compile successfully
  - Pass all existing tests
  - Include tests for new functionality
  - Follow project formatting/linting

- **Before committing**:
  - Run formatters/linters
  - Self-review changes
  - Ensure commit message explains "why"

### Error Handling

- Fail fast with descriptive messages
- Include context for debugging
- Handle errors at appropriate level
- Never silently swallow exceptions

## Decision Framework

When multiple valid approaches exist, choose based on:

1. **Testability** - Can I easily test this?
2. **Readability** - Will someone understand this in 6 months?
3. **Consistency** - Does this match project patterns?
4. **Simplicity** - Is this the simplest solution that works?
5. **Reversibility** - How hard to change later?

## Project Integration

### Learning the Codebase

- Find 3 similar features/components
- Identify common patterns and conventions
- Use same libraries/utilities when possible
- Follow existing test patterns

### Tooling

- Use project's existing build system
- Use project's test framework
- Use project's formatter/linter settings
- Don't introduce new tools without strong justification

## Quality Gates

### Definition of Done

- [ ] Tests written and passing
- [ ] Code follows project conventions
- [ ] No linter/formatter warnings
- [ ] Commit messages are clear
- [ ] Implementation matches plan
- [ ] No TODOs without issue numbers

### Test Guidelines

- Test behavior, not implementation
- One assertion per test when possible
- Clear test names describing scenario
- Use existing test utilities/helpers
- Tests should be deterministic

## Important Reminders

**NEVER**:
- Use `--no-verify` to bypass commit hooks
- Disable tests instead of fixing them
- Commit code that doesn't compile
- Make assumptions - verify with existing code

**ALWAYS**:
- Commit working code incrementally
- Update plan documentation as you go
- Learn from existing implementations
- Stop after 3 failed attempts and reassess

