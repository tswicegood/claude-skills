# Research Sources for Prompt / Skill / Agent Evaluation

The evaluation workflow grounds every "best practice" claim in an external source. This document ranks sources by authority and tells you *what each source is good for*, so you can search efficiently and cite cleanly.

## Tier 1 — Official Anthropic Sources (Always Consult First)

These are the ground truth. When they contradict anything else, they win.

### Primary documentation

- **`docs.claude.com`** — Claude product documentation. The top-level hub for prompt engineering, tool use, skills, and Claude Code. Start here.
  - *Good for:* prompt engineering guide, tool use patterns, extended thinking, prompt caching, citations, files API, skills spec, Claude Code reference, subagents, hooks, slash commands, MCP.
- **`docs.anthropic.com`** — Anthropic API reference.
  - *Good for:* API parameters, model IDs, versioning, SDK behavior, rate limits, capability matrices.
- **Prompt engineering guide** (inside the docs) — Anthropic's canonical instructions for writing prompts.
  - *Good for:* XML tag conventions, role prompts, chain-of-thought, prefilling, structured output, few-shot patterns.
- **Claude Code documentation** — the specific guide for Claude Code as a product.
  - *Good for:* agent config (`CLAUDE.md`), `allowed-tools`, hooks, subagents, MCP servers, settings schema, slash commands.

### Primary examples / reference implementations

- **`github.com/anthropics/skills`** — official skills repository. Canonical reference for `SKILL.md` shape and `references/` usage.
- **`github.com/anthropics/anthropic-cookbook`** — runnable examples for the API.
- **`github.com/anthropics/claude-code`** — Claude Code source and issues (surfaces real behavioral changes).
- **`github.com/anthropics/prompt-eng-interactive-tutorial`** — interactive prompt engineering lessons.

### Release / change communication

- **`anthropic.com/news`** — model releases, feature launches.
  - *Good for:* date-stamped release notes, which model introduced which capability.
- **`anthropic.com/engineering`** — engineering write-ups, prompting deep-dives, research posts, case studies (e.g., "writing effective tools for AI agents," agent building patterns).
- **Model system cards** (PDFs linked from news/engineering posts).
  - *Good for:* documented behavior changes, refusal patterns, tool-use training, safety posture.
- **Claude Code release notes / changelog** — behavioral changes between CLI versions.

### How to cite Tier 1

Cite with a full URL and a dated phrase when possible. Example:

> "Anthropic's prompt engineering guide (docs.claude.com, 2026-03 update) recommends placing long, stable context above short, variable instructions for cache efficiency."

## Tier 2 — Authoritative Community (Consult Second)

Credible but not canonical. Use to fill gaps in Tier 1 or surface changes that aren't yet documented.

- **Anthropic staff posts** on X/Twitter, LinkedIn, personal blogs, conference talks. Verify the account actually belongs to an Anthropic employee before citing.
- **Claude Code issue tracker** (`github.com/anthropics/claude-code/issues`). Useful for confirming whether a behavior is intentional, a bug, or a regression.
- **Well-cited engineering write-ups** from organizations that have built production systems on Claude (e.g., detailed engineering blog posts with code).
- **Anthropic Discord / community forums** (when posts are from staff or widely upvoted and corroborated).

### How to treat Tier 2

- Cite with URL, author name, and date.
- If a Tier 2 claim disagrees with Tier 1, Tier 1 wins — but flag the disagreement, because it might signal that Tier 1 is out of date.

## Tier 3 — Broader Community (Directional Signal Only)

- Reddit (`r/ClaudeAI`, `r/Anthropic`, `r/LocalLLaMA` for comparative context)
- Hacker News threads on Anthropic releases
- Medium / Substack posts from practitioners
- YouTube walkthroughs and conference talks by practitioners
- Twitter/X discussions from non-Anthropic power users

### How to treat Tier 3

- **Never the sole source for a finding.** Community consensus is a *hypothesis*; verify it against Tier 1/2 before marking something as a best-practice violation.
- Useful for discovering *what changed* so you know what to go look up officially.
- Useful for sentiment ("this pattern started getting worse around version X") — but follow up with an official source.

## Search Strategy

### Default sequence (10–15 searches max)

1. **Official docs, feature-specific** (2–3 searches)
   - `"{feature} docs.claude.com"`, `"{feature} claude prompt engineering"`
2. **Official examples** (1–2 searches)
   - `"anthropics/skills {pattern}"`, `"anthropic cookbook {use case}"`
3. **Release / change notes for the target model** (1–2 searches)
   - `"Claude Opus 4.7 release notes"`, `"Claude Opus 4.7 vs 4.6"`, `"Claude {model} system card"`
4. **Authoritative community** (2 searches)
   - Anthropic staff names + topic; Claude Code issues for the feature
5. **Community signal** (0–3 searches, only if gaps remain)
   - `"r/ClaudeAI {topic}"`, `"Claude {model} best practices {year}"`

Stop as soon as searches return no new information.

### Migration-mode additional searches

When the user names both an original and a target model, add:

- `"{original} to {target} migration"`
- `"{target} behavioral changes"` / `"{target} prompt caching"` / `"{target} tool use"`
- Deprecations and removals between versions
- Anthropic engineering posts dated after the target model's release that reference the original model

### Red flags while searching

- **Undated content** — best-practice posts from 2023/2024 may describe obsolete patterns. Look for date stamps.
- **Unversioned advice** — "Claude prefers XML" is meaningless without a model version. Prefer sources that name the model.
- **Repackaged documentation** — many community posts paraphrase the official docs. Go to the source.
- **Contradictions with current docs** — if a community post contradicts current official docs, default to the docs.

## Quoting & Citation Format

Every "best practice" claim in the final report gets a citation. Preferred format in findings:

> **Best-practice reference:** Anthropic prompt engineering guide, "Use XML tags to structure prompts" (docs.claude.com, accessed YYYY-MM-DD).

Short inline form when space is tight:

> (Anthropic docs, prompt engineering guide)

Avoid:

- "It is widely agreed…" (no source)
- "Best practice is…" (no source)
- Citations to your own training data

## When Sources Are Sparse

Some topics (very new features, Claude Code niches) have thin documentation. If Tier 1 is genuinely silent:

1. State this explicitly in the Research Summary ("Tier 1 documentation does not cover X; findings on X rely on Tier 2 sources, noted inline").
2. Lean on Tier 2 with conservative claims ("*appears to be* best practice", not "is best practice").
3. Flag the uncertainty in the report so the user can weight the finding appropriately.
