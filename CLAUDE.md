# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code plugin** that distributes behavioral guidelines for better LLM-assisted coding, inspired by Andrej Karpathy's observations on LLM coding pitfalls. It is a documentation-only project — there are no build, test, or lint commands.

## Repository Structure

- `CLAUDE.md` — The guidelines file (this file). Also the distributable product that users curl into their projects.
- `skills/karpathy-guidelines/SKILL.md` — Skill definition with YAML frontmatter. Content mirrors CLAUDE.md guidelines.
- `.claude-plugin/plugin.json` — Plugin manifest declaring the skill path.
- `.claude-plugin/marketplace.json` — Marketplace listing metadata.
- `EXAMPLES.md` — Detailed before/after code examples demonstrating each principle.
- `README.md` — Installation instructions and principle overview.

## Key Constraint

The guidelines below (sections 1-4) are the product. They appear in three places that must stay in sync:
1. This file (`CLAUDE.md`)
2. `skills/karpathy-guidelines/SKILL.md`
3. `README.md` (expanded form)

When editing guidelines, update all three.

## Plugin System

Users install via `/plugin marketplace add forrestchang/andrej-karpathy-skills` then `/plugin install andrej-karpathy-skills@karpathy-skills`. The `plugin.json` `skills` array points to `./skills/karpathy-guidelines`, which must match the directory name exactly.

---

## Guidelines

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
