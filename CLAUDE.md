# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains custom Claude skills. Skills are reusable prompts that extend Claude's capabilities with specialized workflows and domain expertise.

## Repository Structure

```
claude-skills/
├── {skill-name}/
│   ├── SKILL.md              # Main skill definition with frontmatter metadata
│   └── references/           # Optional: Additional guidance documents
│       └── *.md              # Referenced by the main SKILL.md
```

## Skill Architecture

### SKILL.md Structure

Each skill must have a `SKILL.md` file at the root of its directory. The file follows this structure:

**Frontmatter (YAML):**
```yaml
---
name: skill-name
description: Brief description shown in skill selection. Should include trigger phrases.
---
```

**Content Sections:**
1. **Trigger Phrases**: When the skill should activate
2. **Context Adaptation**: How the skill adapts to different project contexts
3. **Workflow**: Step-by-step execution flow (use Step 1, Step 2, etc.)
4. **Output Structure**: Detailed format of deliverables
5. **Quality Standards**: Requirements for thoroughness and quality
6. **Response Format**: How to present results to the user
7. **Common Pitfalls**: Things to avoid

### Reference Documents

Reference documents in `references/` provide detailed guidance that would clutter the main SKILL.md. They are explicitly referenced by the main skill file (e.g., "read `references/context_adaptation.md` for detailed guidance").

## Creating New Skills

When creating a new skill:

1. Create a directory with a descriptive name (use kebab-case)
2. Create `SKILL.md` with proper frontmatter
3. Include clear trigger phrases and context adaptation guidance
4. Structure workflows with numbered steps
5. Add reference documents only if needed for detailed guidance
6. Ensure the description in frontmatter mentions when to use the skill

## Example: book-report Skill

The `book-report` skill demonstrates key patterns:

- **Confirmation step**: Verifies book details before proceeding with research
- **Multi-source research**: Uses web_search extensively (5+ searches)
- **Context adaptation**: Tailors recommendations to professional/personal/academic/hobby contexts
- **Structured deliverables**: Creates markdown artifact with 8 standardized sections
- **Reference documents**: `context_adaptation.md` provides detailed adaptation strategies

## Key Skill Design Principles

1. **Two-phase workflows**: When appropriate, confirm details before executing (see book-report Steps 1 & 2)
2. **Context awareness**: Skills should adapt to the user's project context (business vs personal vs academic)
3. **Structured output**: Define clear output formats (artifacts, sections, formatting)
4. **Quality standards**: Specify research depth, source requirements, and thoroughness expectations
5. **Actionability**: Focus on implementable insights, not just information gathering

## Git Workflow

This repository uses a simple git workflow:
- Main branch: `main`
- Standard commit and push workflow
