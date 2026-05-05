# Superpowers TDD Workflow Structure Report

## Executive Summary

Superpowers defines a **6-phase macro workflow** that embeds a **6-step RED-GREEN-REFACTOR micro cycle** at phase 5. The framework is not generic TDD—it's TDD embedded within a systematic, plan-driven development lifecycle with mandatory verification gates and agent delegation.

---

## Complete Workflow Phases (Macro: 6 phases)

**Phase 1: Brainstorming**
- Structured design exploration with 9-step checklist
- Outcomes: Design document + user approval
- No code written; prevents scope creep
- Source: `skills/brainstorming/SKILL.md`

**Phase 2: Using Git Worktrees**
- Create isolated workspace on new branch
- Verify project test baseline is clean
- Enables parallel development
- Source: `skills/using-git-worktrees/SKILL.md`

**Phase 3: Writing Plans**
- Breaks work into bite-sized tasks (2–5 minutes each)
- Each task: exact file paths + complete code samples + verification steps
- Produces implementation plan
- Source: `skills/writing-plans/SKILL.md`

**Phase 4: Subagent-Driven Development**
- Dispatches fresh subagent per task
- Two-stage review: specification compliance → code quality
- Enables autonomous execution without plan drift
- Source: `skills/subagent-driven-development/SKILL.md`

**Phase 5: Test-Driven Development** ← *This is where RED-GREEN-REFACTOR activates*
- Enforces strict sequence with verification gates
- Non-negotiable: "NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST"
- Source: `skills/test-driven-development/SKILL.md`

**Phase 6: Requesting Code Review**
- Review against plan + code quality
- Report issues by severity
- Critical issues block progress

**Completion: Finishing a Development Branch**
- Verify all tests pass
- Present merge/PR options
- Source: `skills/finishing-a-development-branch/SKILL.md`

---

## TDD Micro Cycle (Within Phase 5)

### The 6-Step RED-GREEN-REFACTOR Loop

1. **RED** – Write one minimal failing test
   - Clear, descriptive test name
   - Real code (avoid mocks unless unavoidable)
   - Demonstrates desired behavior

2. **Verify RED** – Confirm test fails *correctly*
   - Run: `npm test path/to/test.test.ts`
   - Failure reason: missing feature (NOT syntax error)
   - Must fail before proceeding; else restart

3. **GREEN** – Write minimal code to pass
   - Simplest solution only
   - No refactoring of other code
   - Stop when test passes

4. **Verify GREEN** – Confirm all tests pass
   - No regressions
   - Clean output (no warnings/errors)
   - Else fix immediately

5. **REFACTOR** – Clean while maintaining green
   - Remove duplication
   - Improve naming
   - Extract helpers
   - Tests remain passing throughout

6. **Repeat** – Start next cycle for next feature/bug

### Core Principle
*"If you didn't watch the test fail, you don't know if it tests the right thing."*

### Enforcement Rule
Any production code written *before* tests must be **deleted entirely** and reimplemented through TDD. This is mandatory, non-negotiable.

---

## Agent Names & Involvement

- **brainstorming** – Initial design exploration agent
- **executing-plans** – Plan execution coordinator (references TDD but doesn't directly manage it)
- **subagent-driven-development** – Dispatches per-task subagents
- **test-driven-development** – Enforces RED-GREEN-REFACTOR cycle during implementation

No named "RED agent," "GREEN agent," or "REFACTOR agent"—TDD is a single unified skill that enforces all three phases in sequence.

---

## Visual Conventions & Formatting

### Color Coding (for diagrams)
- **RED phase**: `#ffcccc` (light red)
- **GREEN phase**: `#ccffcc` (light green)
- **REFACTOR phase**: `#ccccff` (light blue)

### Shape Conventions
- **Boxes**: Action steps
- **Diamonds**: Decision points (e.g., "Test fails correctly?")
- **Ellipses**: Continuation/loop back

### Artifacts & Outputs per Step
| Phase | Artifacts |
|-------|-----------|
| Brainstorming | Design document (`.md`), design approval |
| Writing Plans | Implementation plan (`.md`), task list |
| TDD-RED | Failing test file (`.test.ts`) |
| TDD-GREEN | Implementation code (`.ts`) |
| TDD-REFACTOR | Refactored code + passing test suite |
| Code Review | Review report, issue list by severity |
| Finishing | Test report, merge-ready branch |

### Special Markers
- `<Good>` and `<Bad>` code labels for teaching examples
- Verification checklist (8 mandatory items for TDD verification)
- Red flags triggering restart protocol (e.g., "Test passes on first run—restart")

---

## Loops & Feedback Arrows

```
MACRO WORKFLOW (Sequence with mandatory gates):
Brainstorming → Design Approval Gate → Using Git Worktrees 
  → Writing Plans → Subagent-Driven Development 
  → Test-Driven Development [MICRO LOOP] 
  → Requesting Code Review [GATE] 
  → Finishing a Development Branch

MICRO LOOP (TDD Cycle, repeats per feature/bug):
RED → Verify RED [GATE: must fail correctly]
  → GREEN → Verify GREEN [GATE: all pass, no regressions]
  → REFACTOR → [LOOP back to RED for next feature]
  
If gate fails:
  - RED gate fails → Fix test, re-run
  - GREEN gate fails (other tests fail) → Fix immediately, re-verify
  - Code before test detected → Delete code, restart RED
```

---

## Repository References

| File/Skill | URL |
|------------|-----|
| TDD Skill Definition | `skills/test-driven-development/SKILL.md` |
| Brainstorming Skill | `skills/brainstorming/SKILL.md` |
| Executing Plans Skill | `skills/executing-plans/SKILL.md` |
| Subagent-Driven Dev | `skills/subagent-driven-development/SKILL.md` |
| Code Review Skill | `skills/requesting-code-review/SKILL.md` |
| Finishing Branch | `skills/finishing-a-development-branch/SKILL.md` |
| README Overview | `README.md` (main workflow documentation) |

**GitHub Repo**: https://github.com/obra/superpowers

---

## Unresolved Questions

1. **Does the TDD skill auto-commit after each Verify GREEN step, or does that happen at a higher phase level?** (Ref: Phase 5 description mentions "commit" but unclear if per-cycle or per-task)

2. **In subagent-driven-development, which agent actually runs the TDD cycle—the per-task subagent or a separate TDD executor?** (Likely the per-task subagent, but not explicitly stated)

3. **Are there visual diagrams (Mermaid, SVG) in the actual SKILL.md files for the RED-GREEN-REFACTOR cycle?** (Mentioned in analysis but not directly confirmed)

4. **Does the framework define agent behavior for skipped or failed refactoring steps—e.g., if refactoring breaks tests?** (Decision tree for "REFACTOR broke a test" not found)

---

## Recommendation for HTML Diagram

Replace generic "Red-Green-Refactor" boxes with:
1. **6-phase macro loop** (Brainstorming → Plans → Subagents → TDD → Review → Finish)
2. **Within Phase 5 (TDD), embed the 6-step micro cycle** with color-coded boxes and verification gates
3. Use `#ffcccc`, `#ccffcc`, `#ccccff` for RED/GREEN/REFACTOR
4. Show decision diamonds for verification gates: "Test fails correctly?", "All tests pass?", "Regressions?"
5. Add the non-negotiable rule badge: "🚫 NO PRODUCTION CODE WITHOUT FAILING TEST FIRST"
6. Link skill file paths in diagram tooltips/legend for citation
