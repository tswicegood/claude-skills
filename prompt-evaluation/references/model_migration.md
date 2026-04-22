# Model Migration Reference

When evaluating a prompt, skill, or agent that was written for one Claude model version and is now running (or will run) on a different version, the delta between the two versions matters more than generic best practices.

This doc is a **checklist of what to look for during migration** — not a definitive change log. Always verify specific claims against the current Anthropic release notes, system cards, and `anthropic.com/engineering` posts as described in `research_sources.md`.

## Migration Workflow

When the user names both an original and a target model:

1. **Identify the generations involved.**
   - Same generation, different size (e.g., Sonnet 4.6 → Haiku 4.5): mostly capability-scaling issues.
   - Same size, version bump (e.g., Opus 4 → Opus 4.7): behavioral and best-practice drift is the main risk.
   - Cross-generation (e.g., Claude 3.5 Sonnet → Sonnet 4.6, or any pre-4 model → any 4.x model): expect larger shifts in prompting style, tool use, and default behavior.

2. **Research the delta.** Required searches:
   - Release notes / news post for the target model.
   - Release notes for every intermediate version, if >1 version jump.
   - System card for the target model.
   - Any `anthropic.com/engineering` prompting post dated after the target model's release.
   - Any deprecation notices between the two versions.

3. **For every finding that is migration-driven, cite the specific change.** Don't just say "this is stale"; say "this pattern was recommended for Opus 4 ([source]) but updated guidance for Opus 4.7 recommends X ([source])."

## Universal Migration Checks

Apply these regardless of which versions are involved:

### Model ID references

- Any hardcoded model ID in the artifact (e.g., `claude-opus-4-20240229`) is a migration-relevant finding. Replace with the target model ID and note the change.
- API version strings may also need updating if referenced.

### Deprecated API fields / parameters

- Anthropic occasionally renames or deprecates API parameters. Check the current API reference for any parameter referenced in the artifact.

### Tool-use patterns

- Tool-use schema, invocation style, and recommended prompting around tools has evolved across model generations. Verify the artifact's tool-use instructions against the current tool-use documentation for the target model.

### Extended thinking / reasoning

- Availability, defaults, and prompting guidance for extended thinking differ across models and sizes.
- Artifacts that either assume thinking is on (and it isn't) or fight against thinking (and it's now on by default) will behave surprisingly.

### Prompt caching

- Cache-friendly layout (stable content first, variable content last) matters more as caching adoption grows.
- Older prompts that randomize section order will leave cache savings on the table.
- Verify that the target model supports caching for the intended content type.

### Tool & feature introductions

- Features like computer use, memory tool, files API, citations, structured output, JSON mode, batch API, and Claude Code skills / subagents / hooks were introduced at specific points. Artifacts that predate a feature can't use it; artifacts written while a feature was in beta may reference APIs that have since changed.

### Safety / refusal behavior

- Refusal thresholds and the phrasing of refusals shift between model versions. A system prompt that was tuned to unlock specific behavior on an older model may hit different boundaries on a newer one.

### Default verbosity / style

- Different model generations have different default verbosity and style defaults. A prompt whose only brevity instruction was "be concise" may produce long output on a chattier new model and terse output on a quieter one. Favor concrete constraints (word/line limits, section lists) over vibes.

### Role-play / persona framing

- Heavy persona framing ("You are an expert X with 20 years of experience…") was more impactful on earlier models. Newer models respond better to direct, concrete instructions. Excess persona scaffolding is a common migration cleanup target.

## Version-Family Specific Flags

This section flags **categories of things to investigate** during migration. Treat these as prompts for research, not as settled facts — verify against current sources.

### Claude 3.x → Claude 4.x (any size)

Investigate:

- Default behaviors around tool use and tool-choice.
- Changes in recommended structure for system vs. user messages.
- Availability of new features (extended thinking, prompt caching, memory, files API, computer use, citations, batch API).
- Shifts in refusal style and safety posture.
- Token-counting and cost-profile differences that may change prompt budgets.
- Stylistic defaults (verbosity, preamble/postamble, formatting).

Common migration-era findings:

- Prompts that front-load heavy persona framing can often be trimmed.
- Prompts that explicitly disable features the target model now enables by default may need inversion.
- XML-tag conventions that were loose in Claude 3.x should be tightened — Claude 4.x tends to honor structural markers more literally.

### Claude 4.x same-size version bumps (e.g., Opus 4 → Opus 4.5 → Opus 4.6 → Opus 4.7)

Investigate:

- Per-version release notes for prompt-engineering implications.
- Any new features documented between the two versions.
- Agentic capability improvements (longer-horizon task completion, tool-use reliability, subagent improvements) — may let you simplify workflows that were compensating for past limitations.
- Caching and context-window behavior changes.
- Default reasoning behavior (when applicable).

Common migration-era findings:

- Workarounds for earlier-version limitations may be obsolete and safely removable.
- Prompts that aggressively constrain reasoning steps may over-constrain newer versions that handle reasoning better.
- Agent workflows that enumerated fine-grained steps may be simplifiable on newer, more capable versions — but verify against documentation rather than assuming.

### Cross-size within a generation (e.g., Opus 4.x → Sonnet 4.x → Haiku 4.x)

Investigate:

- Capability-tier documentation: what Sonnet or Haiku does well vs. what requires Opus.
- Model-size-specific prompting tips (shorter context budgets, more explicit instructions for smaller models, different default styles).

Common migration-era findings:

- Smaller models benefit from more explicit few-shot examples and tighter output contracts.
- Implicit reasoning that works on Opus may need to be made explicit (or routed to a thinking-enabled mode) on Sonnet/Haiku.

### Claude Code version migrations

Claude Code itself evolves independently of the underlying model. Migration checks:

- `settings.json` schema changes between Claude Code versions.
- Hooks API changes.
- Subagent definition format changes.
- `allowed-tools` syntax changes.
- MCP server configuration changes.
- Skills spec updates.
- Deprecated or renamed commands.

Check the Claude Code release notes and `docs.claude.com` Claude Code section for the current schema before grading any agent artifact.

## Migration Report Additions

When the evaluation is in migration mode, add to the standard report:

### Migration summary

- **From:** original model (version, size)
- **To:** target model (version, size)
- **Key behavioral changes affecting this artifact:** 3–5 bullets, each citing a specific Anthropic source.
- **Features newly available:** features the target model supports that the artifact could exploit (extended thinking, caching, tool use upgrades, etc.) — list with source citations.
- **Features removed / deprecated:** anything the artifact relies on that's no longer available — list with source citations.

### Migration-specific findings

Tag findings driven by the migration with `[migration]` in the title so they're distinguishable from generic best-practice findings. Example:

> 🟡 **[migration] Remove manual step-by-step reasoning scaffolding**
>
> The artifact instructs the model to "first think, then respond, then revise." Opus 4.7 handles this decomposition well without explicit scaffolding (see anthropic.com/engineering, "Building effective agents," 2026). The scaffolding now over-constrains reasoning.

## What to Cite During Migration

Bare minimum for migration findings:

1. The target model's release notes or news post.
2. One prompt-engineering or best-practices post dated after the target model's release.
3. (If applicable) The target model's system card.

If you can't find dated, model-specific guidance for a migration claim, downgrade the finding's priority and mark it as "pattern observed in community sources; not confirmed in official docs."
