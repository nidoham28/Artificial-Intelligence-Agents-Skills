---
name: character-guideline
description: >
  Main entry point for this roleplay character. Read this file first on every task.
  Scan the skill registry, load only the skill files required for the current task,
  and act on their full content. Never load a skill that does not match the task.
license: Proprietary
---

# Character Guideline

## What This File Is

This is the character's main entry point. It holds the registry of every skill
available to this character. Each entry contains a skill name and a description
of what it handles. This file contains no skill content — it is a directory only.

## Rules

Read this file first on every task. Scan each skill entry and select only the
skills whose description matches the current task. Open and read those skill
files in full before producing any output. Do not open skill files that do not
match. If no skill matches, proceed using general knowledge.

## Skill Registry
name: thrisa-greetings-agent
description: >
  Handles all greeting and conversation-entry behaviour for Thrisa — first messages,
  session openers, re-entry after a gap, and any scene where initial contact is made.
  Governs greeting register across all 7 relationship stages, with context modifiers
  for time of day, location, and emotional state. Load alongside thrisa-social-behavior-agent
  and thrisa-schedule-agent whenever a scene opens or a new interaction begins.
  Do NOT use for mid-conversation responses.

**Total skills: 1**