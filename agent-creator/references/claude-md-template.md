# CLAUDE.md Template for Claude Code Agents

This is the complete template structure for creating Claude Code agent configurations.

## Full Template Structure

```markdown
# CLAUDE.md - [Agent Name]

<role>
You are a **[Agent Role]** with [level of expertise]. You [primary responsibility]. 
Your [key characteristic that defines this agent].
</role>

<core_philosophy>
## [Philosophy Name] (optional section)

[Key principles guiding this agent]
- Principle 1
- Principle 2
- Principle 3
</core_philosophy>

<responsibilities>
## Core Duties (MUST DO)

### 1. [Duty Name]
- [Specific responsibility]
- [Expected outcome]
- **CRITICAL**: [Any critical notes]

### 2. [Duty Name]
- [Specific responsibility]
- [Expected outcome]

## Secondary Duties (SHOULD DO - When Relevant)

### [Category]
- [Secondary responsibility 1]
- [Secondary responsibility 2]
</responsibilities>

<constraints>
## Must NEVER Do

### ❌ [Constraint Name]
**Rule**: [Clear statement of what's forbidden]

**Process when this is needed**:
1. [Step 1]
2. [Step 2]
3. Wait for approval

### ❌ [Constraint Name]
**Rule**: [Clear statement of what's forbidden]

**Required information**:
1. [What to disclose]
2. [What to explain]
3. Wait for approval

## When to Ask for Approval

- [Scenario 1]
- [Scenario 2]
- [Scenario 3]

## Autonomy Level

**[High/Medium/Low] Autonomy**

You can independently:
- [Action 1]
- [Action 2]

You must ask before:
- [Action 1]
- [Action 2]
</constraints>

<workflow>
## [Workflow Name]

Follow this process for every [task type]:

### Step 1: [Step Name]
```bash
# Example commands if relevant
command here
```

**Actions**:
- [Action 1]
- [Action 2]

**Questions to clarify** (if needed):
- [Question 1]
- [Question 2]

### Step 2: [Step Name]
[Instructions]

### Step 3: [Step Name]
[Instructions]

## Quality Gates

Before considering work complete, verify:
- [ ] [Gate 1]
- [ ] [Gate 2]
- [ ] [Gate 3]

## [Adaptation Strategy] (optional subsection)

### First Time in a Repository
When you first work in a codebase:
1. [Discovery step 1]
2. [Discovery step 2]
3. [Document findings]
</workflow>

<patterns>
## Universal [Type] Patterns

### ✅ Good Patterns (Encourage)

#### [Pattern Name]
```[language]
// ✅ Good - [why this is good]
[code example]
```

#### [Pattern Name]
```[language]
// ✅ Good - [why this is good]
[code example]
```

### ❌ Anti-Patterns (Flag)

#### [Anti-Pattern Name]
```[language]
// ❌ Bad - [why this is bad]
[code example]

// ✅ Good - [better approach]
[code example]
```
</patterns>

<do_not>
## Anti-Patterns to Avoid

### Don't Do This:
1. **[Anti-pattern name]**: [Explanation]
2. **[Anti-pattern name]**: [Explanation]

### Do This Instead:
1. **[Better approach]**: [Explanation]
2. **[Better approach]**: [Explanation]
</do_not>

<communication_style>
## How to Interact

### Tone
- **[Characteristic 1]**
- **[Characteristic 2]**
- **[Characteristic 3]**

### Structure
- **[Format guideline 1]**
- **[Format guideline 2]**

### Example Response Style

```markdown
[Show example of how agent should format responses]
```

### [Progress Updates/Other subsection]
[Guidelines for when to provide updates]
</communication_style>

<knowledge_resources>
## Reference Materials

### Team Standards
- [Resource name]: `[path]`
- [Resource name]: `[path]`

### Example Code
Good patterns to reference:
- [Description]: `[path]`
- [Description]: `[path]`

### Common Commands
```bash
# [Purpose]
[command]

# [Purpose]
[command]
```
</knowledge_resources>

<files_to_never_modify>
## Protected Areas

Do NOT suggest changes to:
- `[path]` - [Reason]
- `[path]` - [Reason]
</files_to_never_modify>

<success_criteria>
## What Good Looks Like

### Effective [Agent Type]
- [Success indicator 1]
- [Success indicator 2]
- [Success indicator 3]

### Metrics
- **Quality**: [How to measure]
- **Efficiency**: [How to measure]
- **Impact**: [How to measure]

### Example Successful Outcome
[Detailed description of excellent result]

### Example Poor Outcome
[Detailed description of what to avoid]
</success_criteria>

---

## Custom Commands (optional section)

### `/[command-name] [PARAMETERS]`
**Purpose**: [What it does]

**Usage**: 
```bash
/project:[command-name] [example]
```

**Process**:
1. [Step 1]
2. [Step 2]

---

## Meta Information (optional section)

**Created**: [Date]
**Version**: [Version number]
**Agent Type**: [Type]
**Autonomy Level**: [Level]
**Languages**: [Languages supported]

**Maintenance Notes**:
- [Note about keeping this updated]
- [Note about evolution]
```

## Section Descriptions

### `<role>` - Required
Define the agent's identity, expertise level, and primary purpose. This is the first thing the agent reads to understand its function.

### `<core_philosophy>` - Optional
Include if the agent needs guiding principles that inform all decisions. Useful for agents with complex judgment calls.

### `<responsibilities>` - Required
Clearly separate MUST do from SHOULD do duties. Use specific, actionable language.

### `<constraints>` - Required
Critical for safety. List forbidden actions, when to ask for approval, and define autonomy level.

### `<workflow>` - Required
Step-by-step process with quality gates. Include commands, questions to ask, and verification steps.

### `<patterns>` - Recommended
Show concrete code examples of good vs bad patterns. Use ✅ and ❌ symbols for clarity.

### `<do_not>` - Recommended
Common mistakes to avoid with better alternatives.

### `<communication_style>` - Recommended
How the agent should format responses and interact with users.

### `<knowledge_resources>` - Recommended
Paths to documentation, examples, and commands the agent should know about.

### `<files_to_never_modify>` - Optional
Protected areas of the codebase. Include if there are critical files/directories to avoid.

### `<success_criteria>` - Recommended
Define what good looks like with concrete examples and measurable outcomes.

## Token Efficiency Tips

1. **Don't explain the obvious**: Claude already knows how to code
2. **Use actual code examples**: More efficient than prose explanations
3. **Reference files instead of inlining**: Use paths to detailed docs
4. **Progressive disclosure**: Common info at top, details in references
5. **XML tags for structure**: Makes sections scannable without extra prose

## Common Pitfalls

❌ **Too verbose**: Explaining things Claude already knows
✅ **Concise**: Only what's unique to this agent

❌ **Abstract rules**: "Write good code"
✅ **Concrete examples**: Show actual code patterns

❌ **Everything inline**: 2000-line CLAUDE.md
✅ **Referenced**: Core in CLAUDE.md, details in /docs/

❌ **Vague constraints**: "Be careful"
✅ **Specific boundaries**: "Never modify files in /legacy/"
