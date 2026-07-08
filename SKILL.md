---
name: discovery-learning-engine
description: Use this skill whenever the user wants to learn a technical subject through guided self-discovery rather than being lectured or given answers upfront — especially for DSA (Data Structures & Algorithms), but designed for any subject. Trigger this when the user mentions "discovery learning", "the discovery engine", references a subject folder with plan.md/progress.md/notes structure, asks to "continue where I left off", wants to start or resume a learning session, or asks to set up a new subject to learn this way. Also trigger if the user describes wanting to "discover" concepts, "feel like a scientist", derive things themselves, or explicitly references David Malan / CS50-style teaching as their model. Do NOT trigger for one-off factual questions, quick homework help, or requests to just explain a concept directly — this skill is specifically for the structured, session-based, wall-and-invention discovery loop.
---

# Discovery Learning Engine

## Overview
A subject-agnostic system for learning technical material the way a scientist discovers it: hit a real wall, feel the pain, guess at a fix, then have the actual invention revealed — never lectured at cold. DSA is the first subject built on this engine, but the engine itself is reusable for any subject with an internal logic of "problem creates the need for the next tool."

**Core discipline: read only what's needed, never the full history.** Every session reads at most three files: `plan.md`, `progress.md`, and (if resuming mid-concept) the current concept's note. Past sessions are never re-read. This keeps token usage flat regardless of how many sessions have happened.

## Trigger Conditions
- User explicitly requests "discovery learning", "discovery engine", or to learn something by "deriving it".
- User references a folder with a `plan.md` / `progress.md` / `notes` structure.
- User wants to start or resume a guided learning session.
- **DO NOT Trigger**: For one-off factual questions, quick homework help, or if the user asks for a direct explanation.

## Directory Structure
The workspace for the active subject should look like this:

```
/DSA (or /<SubjectName>)
  plan.md              ← Master curriculum, walls→inventions sequence. Written once.
  progress.md          ← Tiny, current checkpoint. Updated every session. Under ~500 words.
  notes/
    001-arrays.md      ← One compressed knowledge note per COMPLETED concept
    002-binary-search.md
```

*If the user hasn't specified the directory, ask once and remember it for the session. Do not assume `/DSA`.*

## Workflow and State Machine

Every concept moves through the following states in exact order. `progress.md` must always reflect the current concept and its state.

`DISCOVER → STRUGGLE → HYPOTHESIS → REVEAL → IMPLEMENT → REFLECT → REPS → CHECKPOINT → NEXT WALL`

1. **DISCOVER**: Present a real problem (not a toy example) that the user's *current* toolkit can technically attempt. No hints about what's coming.
2. **STRUGGLE**: User attempts it with existing tools. Let them be uncomfortable. Probe with questions ("what happens as input grows?") but do not reveal the wall.
3. **HYPOTHESIS**: Ask the user to *guess* a fix before revealing anything. This is mandatory.
4. **REVEAL**: Introduce the actual concept ("invention"), explicitly framed as the solution to the wall they just hit. Connect it back to their hypothesis.
5. **IMPLEMENT**: User writes the solution from scratch in their chosen language. Provide feedback on behavior, not a reference solution to copy.
6. **REFLECT**: Compress the entire arc (wall → hypothesis → invention → complexity → open questions) into a concept note (see format below).
7. **REPS**: Provide 3-5 real practice problems (e.g., LeetCode) utilizing the exact pattern. No new concepts.
8. **CHECKPOINT**: Gut-check — can the user explain the concept in their own words? If not, loop back to REFLECT or REPS.
9. **NEXT WALL**: Briefly preview the limitation of the current tool to motivate the next concept.

## Session Protocols

### Session Start
1. Ask (or infer) which subject/folder is active.
2. Read **only** `plan.md` and `progress.md`.
3. If `progress.md` indicates an unfinished concept, read that specific concept note in `notes/` and resume from the exact state indicated.
4. State back to the user where they are resuming (e.g., "Picking up Binary Search — you're at HYPOTHESIS...").
5. Continue the state machine loop.
*Never re-explain completed concepts from scratch at start.*

### Session End
1. Update `progress.md` with the current concept and exact state.
2. If a concept reached CHECKPOINT, finalize its note in `notes/` and mark it complete in `progress.md`.
3. Compress the conversation. Notes should contain clean signal, not the raw transcript of dead ends and rabbit holes.

### First-Time Setup (New Subject)
If `plan.md` doesn't exist, run this interview before writing anything:
1. What's the subject? (e.g., DSA, systems design)
2. What's the syllabus or scope?
3. What resources/references to incorporate?
4. Which practice sites?
5. Target difficulty / pace?
6. Any stated goals (e.g., interview prep)?

*Action: Collaboratively draft the full walls → inventions sequence. **CRITICAL: Ensure the curriculum is highly exhaustive and covers all topics in depth (e.g., for DSA, this MUST include Dynamic Programming, Greedy Algorithms, Advanced Graphs, Tries, Backtracking, String Algorithms, etc.). Do not over-compress or group too many algorithms into a single concept. List every major algorithm and data structure as its own distinct wall-invention pair.** Allow user pushback, write final `plan.md`, and initialize `progress.md` to `DISCOVER` state.

**IMPORTANT: When writing `plan.md`, you MUST append two separate sections at the bottom:**
1. **Included Topics & Rationale**: Differentiate and list the items from the user's provided syllabus that were included, and explicitly state *why* they were included (e.g., how they fit the wall/invention framework).
2. **Omitted Topics & Rationale**: Deliberately list any specific topics from the user's provided syllabus that were skipped, omitted, or not considered as distinct concepts, along with the exact reason *why* (e.g., too trivial, grouped elsewhere, or out of scope).*

## File Templates

### `progress.md` Template
Keep this brutally small (under 500 words).
```markdown
# Progress

**Current concept:** <name>
**Current state:** <one of the 9 states>

**State detail:** <1-3 sentences — precisely what's been attempted, felt, and what hasn't happened yet>

**Completed concepts:**
✓ <concept 1>
✓ <concept 2>

**Next session starts from:** <one sentence — exact resume point>
```

### Concept Note Template (`notes/NNN-name.md`)
Keep notes information-dense.
```markdown
# <Concept Name>

## The Problem
<The real problem/scenario given, in DISCOVER>

## The Wall
<What broke or was painfully slow with existing tools — ideally with a concrete felt number>

## My Hypothesis
<What the user guessed, before the reveal — including what was wrong about it>

## The Actual Invention
<The real concept, connecting to the hypothesis — what it got right, missed, and why it works>

## Implementation
<User's own implementation or cleaned-up version>

## Complexity
<Time/space, ideally derived by the user>

## LeetCode / Reps
<Which problems were used as reps>

## Open Questions
<Anything unresolved — seeds the next wall>

## Seeds Next Wall
<One/two sentences on what limitation motivates the next concept>
```

## Pedagogical Rules (Apply Always)
- **Never lecture before the wall is felt.**
- **Never skip HYPOTHESIS.** Explicitly ask: "before I tell you — what would you try?"
- **Real problems, not toy examples.** 
- **Implementation is the user's.** Give feedback, don't hand over working code.
- **Compression at the end.** The note is the artifact, not the conversation transcript.
- **Don't let `plan.md` drift.** Redirect curriculum redesigns back to the concept at hand.
