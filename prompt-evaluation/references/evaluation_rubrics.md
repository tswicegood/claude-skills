# Evaluation Rubrics

Three rubrics — one per artifact type. Each rubric defines dimensions, what to check, and example evidence. Use the rubric that matches what you're evaluating. For hybrid artifacts (e.g., a `CLAUDE.md` that behaves like a skill), use both and dedupe overlapping findings.

Every dimension produces:

- **Score**: ✅ Strong / ⚠️ Needs work / ❌ Broken / ➖ N/A
- **Evidence**: direct quote (with line reference where possible) or explicit note that the dimension is absent
- **Best-practice reference**: citation from `research_sources.md`
- **Concrete fix**: replacement text or structural change

---

## Rubric 1: Prompt

For system prompts, user prompts, prompt templates, and tool-use instructions.

### Core dimensions

#### 1. Goal clarity

- Does the prompt state its objective in one or two unambiguous sentences near the top?
- Can a reader articulate "this prompt succeeds when ___" after reading only the first 200 tokens?
- **Watch for:** multi-goal prompts with no prioritization; objectives buried below examples.

#### 2. Instruction correctness

- Are instructions internally consistent? (No "always do X" + "never do X" conflicts.)
- Are negative instructions backed by positive ones? ("Don't do X, instead do Y.")
- Are there unreachable branches or dead instructions?
- **Watch for:** contradictions between system prompt and few-shot examples; instructions that presuppose capabilities the model doesn't have for the target version.

#### 3. Specificity

- Abstract directives replaced with measurable ones.
  - ❌ "Be thorough." → ✅ "Cite at least 3 sources; quote 1–2 sentences from each."
  - ❌ "Respond concisely." → ✅ "Maximum 150 words, no preamble."
- Numeric constraints (counts, lengths, budgets) explicit.

#### 4. Structure & placement

- **Stable content first, variable content last** (prompt-caching friendly). See Anthropic's caching guidance.
- Clear sections: role / context / task / constraints / examples / output format. XML tags or headers make boundaries explicit.
- Most critical rule near the top *and* near the output boundary.

#### 5. Few-shot examples

- Present where the task is non-trivial.
- Examples cover the edge cases, not just the happy path.
- Examples match the requested output format exactly.
- **Watch for:** examples that implicitly contradict the instructions; 10 near-identical examples where 3 diverse ones would do.

#### 6. Output contract

- Output format is specified explicitly (free text vs. JSON vs. XML vs. schema).
- For structured output: schema is provided or referenced; invalid cases are handled.
- For free text: length, tone, and section shape are specified.

#### 7. Failure & ambiguity handling

- What should the model do on empty, malformed, or hostile input?
- What should it do when information is insufficient? (Refuse? Ask? Best-effort with a caveat?)
- **Watch for:** prompts that are silent on ambiguity — the model will improvise, often badly.

#### 8. Model fit (current version)

- Uses current-model features appropriately (extended thinking, prompt caching, tool use, memory, citations).
- Avoids anti-patterns left over from older models (e.g., heavy-handed "You are a helpful AI assistant…" preamble when not needed; excessive role-play framing; stale XML-tag recommendations superseded by feature-specific guidance).
- Honors current best practice on system vs. user message placement.

#### 9. Token economy

- No filler ("I'll now carefully consider…").
- No re-explaining what the model already knows.
- No duplicate instructions across sections.

#### 10. Safety & refusal behavior

- Clear about what the model should refuse.
- No instructions that attempt to override model safety training.
- For API prompts used in production: handles prompt-injection risk from user-supplied inputs.

### Common prompt failure patterns

- **The buried lede** — the actual task is in paragraph 5.
- **The contradictory examples** — few-shot examples don't follow the instructions.
- **The vague adjective stack** — "thorough, comprehensive, detailed, rigorous."
- **The untested negative** — "Don't hallucinate" without positive guidance on what to do when uncertain.
- **The ancient preamble** — role-play scaffolding written for a model version two generations old.

---

## Rubric 2: Skill (`SKILL.md` + `references/`)

For skills intended to be loaded by Claude Code or Claude.ai. Evaluate `SKILL.md` *and* every file in `references/`.

### Core dimensions

#### 1. Frontmatter quality

- `name`: kebab-case, unique, descriptive.
- `description`: The most important field. Evaluate whether it will reliably *trigger* the skill when it should and *not trigger* when it shouldn't.
  - Contains concrete trigger phrases the user would say.
  - States when to use and (implicitly) when not to.
  - ≤ ~400 characters (long descriptions get truncated in selection UIs).
- **Watch for:** descriptions so generic that every user message triggers them; descriptions that read like a summary of what the skill does but don't mention *when* to use it.

#### 2. Trigger phrase coverage

- Explicit trigger-phrase list near the top.
- Covers synonyms and rephrasings the user might actually say.
- **Watch for:** trigger phrases that are too narrow (single exact phrase) or too broad (matches any request).

#### 3. Workflow structure

- Numbered steps with clear names.
- Each step has a definite start and end condition.
- Decision points (branches, checkpoints) are explicit.
- **Watch for:** workflows with implicit ordering; steps that say "then do the thing" without defining "the thing."

#### 4. Progressive disclosure

- `SKILL.md` contains the high-frequency content.
- Low-frequency detail lives in `references/*.md` and is **referenced by name** from `SKILL.md`.
- Reference files are read only when needed, not speculatively.
- **Watch for:** 2000-line `SKILL.md` that inlines everything; reference files that are never pointed to.

#### 5. Context adaptation

- Skill adapts to relevant user/project contexts (e.g., business vs. personal vs. academic).
- Adaptation guidance is concrete (not "use your judgment").

#### 6. Output schema

- Deliverable format is specified: artifact file, structured response, sections, length.
- Required sections and their content are spelled out.
- Examples of good output are provided or pointed to.

#### 7. Quality standards section

- States thoroughness expectations (e.g., "min 3 sources," "max 15 searches").
- Defines when the skill is "done."

#### 8. Common pitfalls

- Lists known failure modes of this skill specifically, not generic AI warnings.

#### 9. Token / context efficiency

- Skill doesn't duplicate content already in the Claude base system prompt.
- Doesn't inline content that could live in a reference file.

#### 10. Consistency with official `anthropics/skills` examples

- Structure matches canonical examples (frontmatter shape, section ordering, reference pattern).
- Divergences from canonical structure are justified, not accidental.

### Skill-specific failure patterns

- **Description mismatch** — frontmatter description doesn't match what the skill actually does.
- **Dead references** — `references/foo.md` exists but nothing in `SKILL.md` points to it.
- **Invisible triggers** — trigger phrases are only implied, never listed.
- **The monolith** — everything crammed into `SKILL.md` with no progressive disclosure.
- **The ghost skill** — no concrete output schema, so deliverables vary wildly.

---

## Rubric 3: Agent (`CLAUDE.md` / subagent / custom command)

For Claude Code agents: the project-level `CLAUDE.md`, subagent definitions, custom command files, and tool configurations.

### Core dimensions

#### 1. Role clarity

- The agent's identity, purpose, and the gap it fills are stated up front.
- "This agent IS X" and "this agent IS NOT Y" are both explicit.
- **Watch for:** multi-role agents that blur responsibilities.

#### 2. Responsibilities (MUST / SHOULD / MUST NOT)

- Core duties (MUST do every time) are listed.
- Secondary duties (SHOULD do when relevant) are distinguished.
- Explicit out-of-scope list (MUST NOT do).
- **Watch for:** implicit boundaries ("don't be bad") instead of explicit ones.

#### 3. Autonomy calibration

- Autonomy level is explicit: analyze-only, propose-then-approve, act-independently.
- Autonomy matches the action's blast radius. (High autonomy for editing tests is fine; high autonomy for force-pushing is not.)
- When to stop and ask is defined.

#### 4. Protected areas / blast radius

- Explicit list of files/directories/commands the agent must not touch.
- Destructive operation policy is explicit (rm, force-push, dropping tables, etc.).
- Agent has guidance on handling unexpected state (unfamiliar files, branches) rather than nuking it.

#### 5. Workflow & quality gates

- Step-by-step workflow defined (e.g., research → plan → implement → verify).
- Definition of "done" is checkable (tests pass, lint clean, coverage threshold, reviewer-approved).
- Gates are *enforced* — the agent doesn't skip them with "will fix later."

#### 6. Tool configuration

- `allowed-tools` / tool permissions match the workflow (no unused tools exposed; no needed tools missing).
- Tool-use instructions are specific (e.g., "use Grep tool, not `grep` bash").
- Hooks, MCP servers, subagent definitions are coherent with the stated workflow.
- **Watch for:** agents that list "Bash" in allowed tools but never specify safe/unsafe commands.

#### 7. Communication style

- Output style is specified (concise bullets, verbose explanations, code-only).
- Preamble / postamble policy is explicit.
- Update cadence during long tasks is defined.

#### 8. Failure handling

- Behavior on failing tests, broken build, merge conflicts, missing dependencies is defined.
- Root-cause-first vs. workaround-first policy is explicit.
- When to escalate to the user is defined.

#### 9. Model fit (current Claude Code version)

- Uses current Claude Code features correctly: subagents, hooks, MCP, skills, settings schema, `allowed-tools`, slash commands.
- References correct settings file paths and schema for the current version.
- Avoids patterns that were valid in earlier Claude Code versions but have since changed.
- Model ID references (if any) are current and valid.

#### 10. Security posture

- Secrets handling is explicit (no committing `.env`, credential files, etc.).
- Prompt-injection from untrusted inputs is considered if the agent reads external content.
- Git safety practices: no force-push to protected branches, no skipping hooks without authorization, no config modifications.

#### 11. Length discipline

- `CLAUDE.md` is as short as it can be while meeting its goals (rough target: <1000 lines, ideally <500).
- Low-frequency detail is in referenced docs, not inlined.
- **Watch for:** 3000-line `CLAUDE.md` files that the agent will only partially consume.

#### 12. Measurable success criteria

- Defines what a "good outcome" looks like concretely.
- Defines what a "bad outcome" looks like concretely.
- Metrics / signals the user can watch are listed.

### Agent-specific failure patterns

- **The omnipotent agent** — no protected areas, unlimited autonomy, no quality gates.
- **The scared agent** — asks for approval on every step, blocking progress.
- **Tool sprawl** — every tool allowed, with no guidance on when to use which.
- **Silent failure** — no definition of "done," no gates, happily commits broken code.
- **Version drift** — references settings paths, commands, or features from old Claude Code versions.
- **Over-inlining** — 500 lines of documentation pasted into `CLAUDE.md` instead of referenced.

---

## Hybrid Artifacts

Some files wear multiple hats. Common cases:

- **`CLAUDE.md` that behaves like a skill** — includes workflow + output spec, not just agent config. Evaluate against both Rubric 2 and Rubric 3; dedupe overlapping findings (usually structure, examples, failure handling).
- **A prompt file packaged as a skill** — `SKILL.md` whose body is a long system prompt. Evaluate body with Rubric 1, frontmatter and references with Rubric 2.
- **A subagent definition** — evaluate the system-prompt portion with Rubric 1 and the scoping/tool config with Rubric 3.

When using multiple rubrics, state which dimensions came from which rubric in the scorecard so the user can see the mapping.

---

## Scoring Shorthand

Use consistently across reports:

- ✅ **Strong** — meets or exceeds best practice; no change needed.
- ⚠️ **Needs work** — partially meets best practice; specific improvement available.
- ❌ **Broken** — violates best practice in a way that will cause a concrete failure.
- ➖ **N/A** — dimension doesn't apply to this artifact (note why).

A dimension is never scored without **evidence** and a **best-practice reference**.
