# Discovery Learning Engine (Antigravity Skill)

A custom skill for the Google Antigravity (AGY) system that transforms the AI into a strict Socratic tutor. Instead of lecturing, it guides you to learn technical subjects (like Data Structures & Algorithms) through a cycle of guided self-discovery: hitting a wall, hypothesizing a solution, and then revealing the true invention.

## Features

- **State Machine Guided Learning:** Forces the user through a strict pedagogical loop (`DISCOVER → STRUGGLE → HYPOTHESIS → REVEAL → IMPLEMENT → REFLECT → REPS`).
- **Token Efficient:** Designed to keep LLM context windows small by maintaining a minimal `progress.md` state file and writing dense concept notes rather than re-reading long chat histories.
- **Subject Agnostic:** Originally designed for DSA, but adaptable to any technical subject where problems organically motivate the need for new tools.

## Installation

Clone this repository into your local Antigravity skills directory:

```bash
git clone git@github.com:codebyvimal/discovery-learning-engine.git ~/.gemini/config/skills/discovery-learning-engine
```

## Usage

Once installed, trigger the skill in a new Antigravity session by explicitly mentioning it:
- *"I want to learn [Subject] using discovery learning."*
- *"Start a discovery learning session on [Topic]."*

The agent will automatically initiate the setup interview and begin the discovery loop.
