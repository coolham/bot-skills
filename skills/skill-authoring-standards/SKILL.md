---
name: Skill authoring standards
description: >-
  Use when creating, rewriting, reviewing, or committing a shared bot skill
  so all agents follow one structure in coolham/bot-skills.
---

# Skill authoring standards

Follow this recipe whenever you add or change a skill in the shared library or in `coolham/bot-skills`.

## Before you write
1. Search existing `skills/<skill-id>/` for an overlapping skill — extend it instead of forking.
2. Choose a kebab-case `skill-id` derived from the human `name`.
3. Keep assistant-specific details (bot names, agent ids, Slack channels, cron) out of the skill; put those in the routine or memory.

## Required SKILL.md shape
```yaml
---
name: Human-readable Title
description: >-
  Use when … (WHEN only; this is what matching uses)
---
```
Then markdown body with, as needed and in this order: purpose → caller inputs/defaults → steps (intent, not frozen tool schemas) → output format → failure/fallbacks → delivery (who gets the result; silent vs short note when empty).

## Must / must-not
- Must: generic, configurable defaults, clear delivery, real URLs when linking, no secrets.
- Must-not: freeze MCP tool argument JSON; duplicate near-identical skills; paste private tokens.

## Ship
1. Save into the shared skill library (`UpdateSkill`) so every agent can run it immediately.
2. Commit to `coolham/bot-skills` under `skills/<skill-id>/SKILL.md`; open/merge a PR titled `Add|Update <skill-id> skill`.
3. Point routines at the skill by name/id; do not copy the full body into the routine.

## Full normative text
Agents and humans should treat `docs/skill-spec.md` in this repo as the full checklist (layout, review checklist, boundaries with routines). Keep this skill and that doc aligned when either changes.
