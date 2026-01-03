# How to Write Product Requirements Documents (PRD)

## What is a PRD?

A PRD tells humans and AI what problem you're solving, what cannot break, and how you'll measure success—before writing a single line of code. It's your system's blueprint.

## The Core Mental Model

Before you start, reframe your thinking:

**❌ Weak framing:** "I'm building an AI D&D game."

**✅ Strong framing:** "I'm building a system that reliably acts as a Dungeon Master under specific constraints."

This shift in perspective is the entire point of writing a good PRD.

---

## PRD Template: AI Dungeon Master Example

We'll use an AI Dungeon Master system as an example. You can apply this structure to any project.

### 1. Title & One-Line Summary

**AI Dungeon Master (AI DM)**

An AI system that runs tabletop-style roleplaying sessions with consistent world state, fair rules enforcement, and bounded behavior.

**Why this matters:** A clear title that implies constraints sets expectations immediately.

### 2. Problem Statement

This is your anchor. Don't skip it.

The problem is **not** "we don't have an AI DM." The real problems are:

- AI-generated D&D games drift in rules, lore, and tone
- World state is forgotten or contradicted
- Players can exploit or derail the system
- Sessions become inconsistent and untrustworthy

**Problem Statement:**

Current AI-driven roleplaying experiences lack state consistency, rule enforcement, and narrative continuity, resulting in broken immersion and unreliable gameplay.

**Teaching point:** If this paragraph is weak, your entire PRD collapses.

### 3. Goals & Non-Goals

#### Goals

When the system ships, it will:

- Maintain persistent world state across sessions
- Enforce D&D-style rules consistently
- Produce predictable, explainable outcomes for player actions
- Support pausing and resuming without losing context

#### Non-Goals

- This is **not** a complete D&D rules engine
- This does **not** simulate dice physics or graphics
- This does **not** guarantee perfect rule coverage
- This does **not** replace human creativity

**Teaching point:** Non-goals protect you from scope creep and the "just one more feature" trap.

### 4. Users & Operators

Name your actors. If you can't, you don't understand your system.

- **Players:** Interact with the system via chat or UI
- **AI DM:** The system you're building
- **Operators:** Configure rules, worlds, and constraints
- **System components:** State management, memory, controls

### 5. Success Criteria

Don't measure features. Measure outcomes.

- The same player action in the same game state always yields the same result
- Rule violations are detected and clearly explained
- World state contradictions are identifiable
- The AI rejects actions that violate constraints
- Sessions can be compared for consistency

**Teaching point:** If you can't measure it, you can't claim success.

### 6. System Overview

Describe the shape of your system, not the implementation.

**System Flow:**

1. Player submits an action
2. System validates action against rules and current state
3. AI generates narrative response
4. System updates world state
5. Response returns to player

This should be drawable on a whiteboard in under a minute.

### 7. Functional Requirements

Use "SHALL" language. Be specific about behavior.

- **FR-1:** The system SHALL maintain persistent world state across sessions
- **FR-2:** The system SHALL enforce defined game rules
- **FR-3:** The system SHALL reject invalid player actions
- **FR-4:** The system SHALL explain why an action was rejected
- **FR-5:** The system SHALL version and track world state changes

**Important:** No implementation details here. Focus on what the system must do.

### 8. Non-Functional Requirements

This is where senior-level thinking appears.

#### Reliability
- World state must survive application restarts
- No data loss between sessions

#### Observability
- All state mutations are logged
- Rule violations trigger structured events
- System behavior is traceable

#### Security & Safety
- Players cannot directly modify world state
- Prompt injection attempts are detected and blocked
- Unintended system behavior is constrained

#### Cost & Control
- Session length is bounded
- Token usage is capped
- Infinite loops are prevented

### 9. Failure Modes & Containment

Staff-level PRDs always include this. If you can't explain failure, you don't own the system.

| Failure Mode | How to Detect | How to Fix |
|---|---|---|
| Lore contradiction | State mismatch detected | Roll back to previous turn |
| Rule bypass attempt | Validation fails | Reject the action |
| Infinite narrative loop | Token counter exceeds limit | Force turn to end |
| Memory drift | State hash mismatch | Restore state from backup |

### 10. Out of Scope / Deferred

Explicitly list what you're **not** building:

- Multiplayer synchronization
- Voice acting
- Custom rule authoring UI
- Advanced NPC AI behavior

**Why this matters:** Scope clearly defined means faster delivery and fewer surprises.

### 11. Open Questions

Document what you don't know yet:

- How strict should rule enforcement be?
- Should players see hidden rolls?
- How much improvisation should be allowed?

**Teaching point:** Unanswered questions are fine. Undocumented ones are not.

---

## How to Use Your PRD

Once you have a solid PRD, you can work with AI safely and effectively.

### ✅ What You Can Ask AI

- "Generate a system architecture that satisfies this PRD"
- "Propose a state schema aligned with FR-1 and FR-5"
- "Identify risks based on the failure modes"
- "Draft an API that enforces these constraints"

### ❌ What You Should NOT Ask

- "Build me an app"
- "Design a D&D game"
- "Make it cool"

**The difference:** You're not asking for a feature. You're setting boundaries and asking for a solution within them.

---

## What Comes After the PRD

The PRD is the spine of your project, not the whole body. Next, you'll create:

1. **Architecture document** – How the system is structured
2. **Control mapping** – Which design choices enforce which requirements
3. **Scaffolding repo** – A minimum working implementation
4. **Test strategy** – Especially tests for failure modes
5. **Iteration plan** – What can change without breaking guarantees

---

## Key Takeaways

- A PRD forces clarity **before** code
- A PRD keeps AI from building something impressive but wrong
- A PRD should be whiteboardable and measurable
- A PRD is how senior engineers think

**Final reminder:** A good PRD means everyone—human and AI—is solving the same problem, with the same constraints, in the same direction.
