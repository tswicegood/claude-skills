---
name: prompt-evaluation
description: Evaluate prompts, skills, and Claude Code agents for correctness and optimization against the latest Claude best practices. Use when the user asks to "evaluate," "review," "audit," "optimize," "modernize," or "upgrade" a prompt, system prompt, SKILL.md, agent definition, or CLAUDE.md, or says things like "I wrote this for Opus 4, is it still good on 4.7?" Researches current official guidance first, then community reports of changes, then produces a prioritized findings report and (on request) a rewritten artifact.
---

# Prompt / Skill / Agent Evaluation

Evaluate a prompt, skill, or Claude Code agent for **correctness** (does it actually do what it claims, unambiguously?) and **optimization** (is it aligned with current best practices for the target Claude model?). Produce a prioritized findings report with concrete rewrites.

## Trigger Phrases

Activate this skill when the user says things like:

- "Evaluate / review / audit this prompt"
- "Is this prompt still good on Opus 4.7?" / "Modernize this for Sonnet 4.6"
- "Optimize this SKILL.md" / "Upgrade this agent / CLAUDE.md"
- "Check my system prompt" / "How would you improve this?"
- Pastes a prompt, `SKILL.md`, or `CLAUDE.md` and asks for feedback

## Artifact Types This Skill Handles

Three artifact types, each with its own rubric:

1. **Prompt** — system prompts, user prompts, prompt templates, tool-use instructions. Evaluated against Anthropic prompt engineering guidance.
2. **Skill** — a `SKILL.md` (plus any `references/*.md`) intended for Claude Code / Claude.ai Skills. Evaluated against the Anthropic Skills spec and the `anthropics/skills` examples.
3. **Agent** — a Claude Code agent: `CLAUDE.md`, subagent definitions, or custom command files. Evaluated against Claude Code documentation and agent best practices.

If the input could reasonably be more than one type (common for `CLAUDE.md` that behaves like a skill), say so and evaluate against both rubrics.

## Context Adaptation

Adapt depth and tone to the user's purpose:

- **Quick check** ("does this look right?") → top 3–5 findings, no full rewrite unless asked.
- **Formal evaluation** ("audit this") → full rubric, every section scored, rewrite provided.
- **Migration** ("I wrote this for Opus 4 — upgrade for 4.7") → emphasize changes between the model versions (see `references/model_migration.md`), not generic advice.
- **Defensive posture** (production prompts, agents with write access) → emphasize safety, ambiguity, and failure modes over stylistic polish.

Infer target model from:
1. Explicit user statement ("I use this with Sonnet 4.6").
2. Model ID strings in the artifact (e.g., `claude-opus-4-5-20250101`).
3. References to model-specific features (extended thinking, prompt caching, memory tool, computer use, etc.).
4. If ambiguous, **ask once** before doing research — picking the wrong target model wastes the rest of the workflow.

## Workflow

### Step 1: Intake & Classify

1. Identify the artifact: paste, file path, URL, or directory. Read the full text (and any referenced files — for a `SKILL.md` with `references/`, read those too).
2. Classify as **prompt**, **skill**, or **agent** (or multiple).
3. Extract or ask:
   - **Target Claude model** (e.g., Opus 4.7, Sonnet 4.6, Haiku 4.5, or "latest").
   - **Original model it was written for**, if different (triggers migration-mode).
   - **Deployment context** (API, Claude Code, Claude.ai, SDK, autonomous agent).
   - **User's primary concern**: correctness, optimization, migration, or safety.
4. Present a one-line summary of what you're about to evaluate and against what target, and **pause for confirmation** unless the user has already been explicit. Example:

   > I'll evaluate this as a **Claude Code agent** (`CLAUDE.md`) targeting **Opus 4.7**, migrating from **Opus 4**, with emphasis on **optimization**. Proceed?

### Step 2: Research Current Best Practices

Do not evaluate from memory alone. Best practices shift between model releases; ground your review in current sources.

Use the tiered source list in `references/research_sources.md`. Summary:

**PHASE A — Official sources first (required, minimum 3 searches):**
- `docs.claude.com` (Claude docs, including prompt engineering, tool use, skills, Claude Code)
- `docs.anthropic.com` (Anthropic API reference, cookbook)
- `anthropic.com/engineering` and `anthropic.com/news` (release notes, prompting guides, research posts)
- `github.com/anthropics/skills` and `github.com/anthropics/anthropic-cookbook` (canonical examples)
- Claude Code documentation for agents / skills / hooks / subagents

**PHASE B — Authoritative community (required, minimum 2 searches):**
- Anthropic staff posts (engineering blog posts, conference talks, verified X/LinkedIn accounts)
- Claude Code GitHub issues and release notes for behavioral changes
- Peer-reviewed or well-cited engineering write-ups

**PHASE C — Broader community (optional, bounded to 2–3 searches):**
- Reddit (`r/ClaudeAI`, `r/Anthropic`), Hacker News, reputable engineering blogs, newsletters
- Treat as **directional signal, not ground truth**. Cross-reference any claim from Phase C against Phase A before acting on it.

**Model-migration research (if target model ≠ original model):**
- Search specifically for: `"{original model} to {target model}" changes`, `"{target model}" prompt engineering`, `"{target model}" system card`, deprecations and behavioral diffs.
- Consult `references/model_migration.md` for known version-to-version shifts.

**Research discipline:**
- Stop at 10–15 searches unless material is genuinely sparse.
- Record sources you'll cite (URL + 1-line relevance). Prefer ≤6 citations in the final report.
- If authoritative sources contradict community sources, **the authoritative source wins** and the contradiction is worth flagging explicitly.

### Step 3: Evaluate Against Rubric

Open `references/evaluation_rubrics.md` and use the rubric matching the artifact type. Each rubric has the same shape:

For every dimension, produce:
- **Score**: ✅ Strong / ⚠️ Needs work / ❌ Broken / ➖ N/A
- **Evidence**: direct quote from the artifact (with line reference if file-based) OR explicit note that the dimension is absent
- **Best-practice reference**: cite the source from Step 2
- **Concrete fix**: a specific rewrite, not "consider clarifying" — show the replacement text

**Core dimensions (all artifact types):**
1. **Correctness** — Does the artifact, read literally, produce the claimed behavior? Hidden contradictions, dead instructions, unreachable branches.
2. **Specificity & concreteness** — Abstract directives ("be thorough") replaced with measurable ones ("cite ≥3 sources").
3. **Structure** — Headings, XML tags, ordering, progressive disclosure. Is the most critical content near the top?
4. **Model fit** — Uses current-model affordances (extended thinking, tool-use patterns, system-prompt placement, caching-friendly layout). No anti-patterns from older models.
5. **Ambiguity & failure modes** — What happens on edge cases, empty input, hostile input, contradictory instructions?
6. **Token economy** — Repetition, filler, over-explaining what Claude already knows.
7. **Examples** — Are few-shot examples present where they'd help? Are present examples high-quality and diverse?
8. **Safety & guardrails** — For artifacts that cause actions (agents, tool use): explicit protected areas, approval gates, refusal conditions.

**Skill-specific dimensions** (see rubric for detail):
- Trigger description quality (does the frontmatter `description` reliably cause activation?)
- Reference-file usage (is progressive disclosure used properly?)
- Output schema clarity

**Agent-specific dimensions** (see rubric for detail):
- Autonomy calibration
- Quality gates / definition of done
- Tool permissions and `allowed-tools`
- Behavior on failure

### Step 4: Prioritize Findings

Sort all findings into three buckets:

- **🔴 Must fix** — correctness bugs, contradictions, unsafe behavior, broken triggers. Ship-blocking.
- **🟡 Should fix** — optimization gaps, missing best practices, stale patterns, ambiguity that will bite.
- **🟢 Nice to have** — stylistic polish, minor token savings, optional extensions.

A finding only earns 🔴 if you can articulate a concrete failure scenario it causes. If you can't, downgrade it.

### Step 5: Produce Report (and, on request, Rewrite)

Use the output structure below. Default to producing the report inline as markdown. If the user asked for a rewrite, or if more than ~5 findings are 🔴/🟡, also produce a rewritten artifact as a separate file/artifact so they can diff it.

## Output Structure

### 1. BLUF (one paragraph)

Target model, artifact type, overall health (one of: *ship it*, *ship with minor polish*, *needs revision*, *rewrite recommended*), and the single most important finding.

### 2. Scope & Research Summary

- Artifact type, target model, original model (if migration)
- Sources consulted (≤6 bullets, each URL + 1 line relevance)
- Any research limitations (sparse docs, conflicting guidance)

### 3. Rubric Scorecard

A compact table: dimension | score | one-line verdict. This is the skimmable overview.

### 4. Findings

Grouped by priority (🔴 / 🟡 / 🟢). For each finding:

- **Title** (imperative: "Remove contradiction between X and Y")
- **Evidence** (quote from artifact)
- **Why it matters** (failure it causes or best practice violated, with citation)
- **Fix** (concrete replacement text, diff-style when useful)

### 5. Rewritten Artifact (if applicable)

A full rewritten version, same format as the input (prompt / `SKILL.md` / `CLAUDE.md`). Preserve the user's voice and intent; don't smuggle in new features.

### 6. Self-Check

Three-bullet sanity check:
- Did you verify correctness issues with concrete failure scenarios?
- Did you cite authoritative sources for every "best practice" claim?
- Did you preserve the author's intent in the rewrite?

## Quality Standards

- **Evidence over opinion.** Every finding cites the artifact (quote/line) and the best-practice source. No unsourced "best practice says…".
- **Official docs outrank community.** When they disagree, flag the disagreement and follow the official source.
- **Concrete fixes.** Every finding includes replacement text, not just "consider revising."
- **Preserve intent.** A rewrite is not a rebuild — do not change the artifact's purpose, audience, or scope.
- **Model-version aware.** If the user named an original and target model, every finding that's migration-driven must say so explicitly ("this pattern is stale as of Opus 4.6 — see [source]").
- **Bounded research.** 10–15 searches max unless truly necessary. Stop when the next search returns no new information.
- **Distinguish taste from correctness.** Do not dress up stylistic preferences as correctness bugs. 🔴 requires a concrete failure scenario.

## Response Format

After completing evaluation:

1. **Short assistant message**: BLUF paragraph + scorecard + link/pointer to the full report.
2. **Full report**: inline markdown or a file, depending on length. If producing a rewritten artifact, put it in a separate file so the user can diff against the original.

Example assistant message opener:

```
Evaluated as **Claude Code agent (CLAUDE.md)** targeting **Opus 4.7** (migrating from Opus 4). Verdict: **needs revision** — 2 🔴, 5 🟡, 3 🟢.

Biggest issue: the "always commit when tests pass" instruction conflicts with the "ask before destructive actions" constraint; current Opus 4.7 will ask, making the first instruction dead code.

Full report below.
```

## Common Pitfalls to Avoid

- **Evaluating from memory.** Best practices change with each model release. Always do Step 2 research, even if you think you know the answer.
- **Skipping confirmation of the target model.** Wrong target = wrong evaluation. Ask if ambiguous.
- **Treating community posts as ground truth.** Use them for signal, then verify against official docs.
- **Laundry-listing generic advice.** "Add XML tags" is only a finding if the artifact would benefit from them *here*, with a quoted example.
- **Rewriting for style.** Keep the author's voice. Only rewrite where a finding justifies it.
- **Inflating priority.** Do not mark stylistic issues as 🔴. 🔴 must have a concrete failure scenario.
- **Ignoring references/.** For skills, the `references/*.md` files are part of the artifact. Don't evaluate a skill whose reference files you haven't read.
- **Generic "upgrade for latest model" rewrites.** If the user asked for migration advice, every migration-related finding must cite a specific, dated change between the two models.
