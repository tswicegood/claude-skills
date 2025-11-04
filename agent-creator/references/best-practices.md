# Agent Best Practices & Troubleshooting

Advanced tips for creating and using Claude Code agents effectively.

## Design Principles

### 1. Start with "Why"

Before creating an agent, understand:
- What problem are you solving?
- What's the cost of not having this agent?
- Will your team actually use it?

**Example**: Don't create a code review agent just because it's cool. Create it because:
- Your team ships bugs that could be caught by review
- Manual reviews are bottlenecking velocity
- You want consistent review standards across teams

### 2. One Job, One Agent

**❌ Bad**: "Full-stack developer agent that reviews code, writes tests, documents APIs, and deploys"

**✅ Good**: 
- Code Review Agent (focused role)
- TDD Implementation Agent (focused role)
- API Documentation Agent (focused role)

Why? Focused agents are easier to:
- Understand and use
- Debug when they fail
- Update and maintain
- Combine strategically

### 3. Progressive Complexity

Start simple, add complexity based on usage:

**Version 1**: Basic workflow, minimal constraints
**Version 2**: Add quality gates after seeing what breaks
**Version 3**: Add automation after manual steps prove valuable
**Version 4**: Optimize for speed after team adopts it

### 4. Trust but Verify

Good agents have:
- Quality gates (checkboxes)
- Verification steps (run tests, check output)
- Guardrails (when to ask for approval)

**Example workflow**:
```
1. Generate code
2. Run tests (verify correctness)
3. Run linter (verify style)
4. Check coverage (verify quality)
5. Commit (only if all gates pass)
```

## Common Pitfalls & Solutions

### Pitfall 1: Agent Too Verbose

**Symptom**: Agent writes novels instead of code

**Causes**:
- No guidance on communication style
- No examples of good output
- Over-explaining obvious things

**Fix**:
```markdown
<communication_style>
## Concise Output

- Use bullet points, not paragraphs
- Show code, don't describe code
- No preamble ("I'll help you...")
- No postamble ("I hope this helps...")
- Get to the point

**Good response**:
```
✅ Tests pass
✅ Coverage: 95%
✅ Linting: passed

Ready to commit.
```

**Bad response**:
"I've carefully analyzed your code and run a comprehensive 
test suite to ensure quality. After thorough examination, 
I'm pleased to report that everything looks great! ..."
```
</communication_style>
```

### Pitfall 2: Agent Asks Too Many Questions

**Symptom**: Agent won't proceed without constant clarification

**Causes**:
- Autonomy level too low
- Too many "ask before" constraints
- Not enough context in CLAUDE.md

**Fix**:
1. Raise autonomy level for routine decisions
2. Provide default assumptions in CLAUDE.md
3. Add examples so agent can infer patterns

**Example**:
```markdown
<constraints>
## Default Assumptions

When not explicitly told otherwise:
- Use existing test framework (discover from package.json)
- Follow code style in surrounding files
- Create files in standard locations (/src, /tests)
- Use team's linting rules (run lint before asking)

Only ask when:
- Multiple valid approaches with trade-offs
- Decision has architectural implications
- Breaking existing patterns
</constraints>
```

### Pitfall 3: Agent Ignores Instructions

**Symptom**: Agent doesn't follow CLAUDE.md rules

**Causes**:
- Instructions too abstract
- No examples provided
- Competing instructions in chat prompts
- CLAUDE.md too long (agent misses key parts)

**Fix**:
1. **Be specific**: "Check test coverage ≥80%" not "ensure good testing"
2. **Show examples**: Include actual code from your codebase
3. **Use XML tags**: Makes sections easy to find
4. **Keep it under 1000 lines**: Long CLAUDE.md files get partially missed

### Pitfall 4: Context Window Fills Up

**Symptom**: "Context running low" warnings, agent forgets earlier context

**Causes**:
- Not using `/clear` between features
- CLAUDE.md too long
- Agent reading too many files unnecessarily

**Fix**:
1. **Use `/clear` aggressively**: Start fresh for each feature
2. **Nested CLAUDE.md files**: Directory-specific configs
3. **Reference docs instead of inlining**: Point to files, don't copy content
4. **Minimize CLAUDE.md**: Keep it under 500 lines if possible

**Example**:
```markdown
## Reference Materials

### API Documentation
See `/docs/api-reference.md` for complete API docs.

### Common Patterns
See `/docs/patterns/*.md` for code examples.

// Don't inline 500 lines of API docs in CLAUDE.md
```

### Pitfall 5: Agent Modifies Wrong Files

**Symptom**: Agent touches files it shouldn't

**Causes**:
- No explicit protected areas list
- Vague constraints about scope
- Agent can't tell what's "important"

**Fix**:
```markdown
<files_to_never_modify>
## Protected Areas

NEVER modify these files/directories:
- `/legacy/*` - Being rewritten, frozen
- `/db/migrations/*` - DBA-owned
- `/config/production.json` - Ops-owned
- `/.github/workflows/*` - DevOps-owned
- Any file with "GENERATED" in first line

If work requires modifying protected files, STOP and ask.
</files_to_never_modify>
```

### Pitfall 6: Agent Over-Engineers Solutions

**Symptom**: Simple feature becomes complex architecture

**Causes**:
- No guidance on starting simple
- No examples of "just enough" solutions
- Lacks pragmatism constraints

**Fix**:
```markdown
<core_philosophy>
## Pragmatic Development

Start with the simplest solution that works:
1. Hard-code first, abstract later
2. Duplicate twice, extract third time
3. Optimize only when measured
4. Perfect is enemy of done

**Anti-pattern**:
For "add logout button":
❌ Building abstract AuthenticationStrategy with Factory pattern

**Good pattern**:
For "add logout button":
✅ Simple logout function that clears session

Complexity should be justified by actual requirements, 
not anticipated future needs.
</core_philosophy>
```

### Pitfall 7: Quality Gates Get Skipped

**Symptom**: Agent commits code that fails tests, linting, etc.

**Causes**:
- No explicit gates in workflow
- Gates not enforced
- Unclear what "done" means

**Fix**:
```markdown
<workflow>
## Before Committing

CRITICAL: All gates must pass before commit.

Run these checks:
```bash
npm test              # Must show "All tests passed"
npm run lint          # Must show "0 errors, 0 warnings"  
npm run typecheck     # Must show "0 errors"
npm run test:coverage # Must show ≥80% coverage
```

Quality Gates:
- [ ] All new tests pass
- [ ] Existing tests unchanged (same pass/fail count)
- [ ] Linting: 0 errors, 0 warnings
- [ ] Type checking: 0 errors
- [ ] Coverage: ≥80% for new code

If ANY gate fails:
1. Fix the issue
2. Re-run all gates
3. Only commit when ALL pass

Never skip gates with "will fix later" or "not important".
</workflow>
```

## Advanced Patterns

### Pattern 1: Layered Agents

Use directory-level CLAUDE.md files for specialization:

```
/project-root/
  CLAUDE.md (org-wide standards, security baseline)
  
  /services/api/
    CLAUDE.md (inherits root + API-specific rules)
    
  /services/ml-pipeline/
    CLAUDE.md (inherits root + ML-specific rules)
```

Agent reads from most specific (deepest directory) upward.

### Pattern 2: Agent Chaining

Create sequences of specialized agents:

```
1. Architecture Review Agent
   ↓ (validates design)
2. TDD Implementation Agent
   ↓ (builds with tests)
3. Performance Review Agent
   ↓ (validates performance)
4. Security Audit Agent
   ↓ (validates security)
5. Documentation Agent
   ↓ (documents everything)
```

Each agent specializes in one aspect of quality.

### Pattern 3: Context-Aware Agents

Agents that adapt based on file type:

```markdown
<workflow>
## Adapt to File Type

When working with:
- `*.test.ts` files → Focus on test clarity
- `*.api.ts` files → Focus on API contracts
- `*.db.ts` files → Focus on query optimization
- `*.component.tsx` → Focus on accessibility

Discover file type from extension and adapt workflow.
</workflow>
```

### Pattern 4: Progressive Learning

Agents that build institutional knowledge:

```markdown
## Project-Specific Patterns (Living Section)

As you discover patterns, document them here:

### Discovered [Date]
- Pattern: [What you learned]
- Location: [Where it's used]
- Rationale: [Why this pattern exists]

This section grows over time as agent learns the codebase.
```

### Pattern 5: Team Coordination

Agents that understand team structure:

```markdown
<knowledge_resources>
## Team Ownership

- `/api-gateway/*` → @platform-team
- `/ml-pipeline/*` → @ml-team
- `/frontend/*` → @web-team

When changes span multiple owners:
1. Identify all affected teams
2. Note which teams need to review
3. Suggest PR workflow (parallel vs sequential reviews)
</knowledge_resources>
```

## Optimization Tips

### Tip 1: Minimize Token Usage

**Before**:
```markdown
The agent should read the file carefully and analyze the code 
to understand what it does. After reading and understanding, 
the agent should think about potential improvements...
(200 words of instructions)
```

**After**:
```markdown
1. Read file
2. Identify improvements
3. Suggest changes
(10 words)
```

Claude already knows how to analyze code.

### Tip 2: Use Code Examples

**Before**:
```markdown
Functions should be small and focused on a single responsibility. 
They should have clear names that describe what they do. Avoid 
long parameter lists...
(100 words)
```

**After**:
```typescript
// ✅ Good
function calculateTax(amount: number, rate: number): number {
  return amount * rate;
}

// ❌ Bad  
function process(a: number, b: number, c: string, d: boolean): any {
  // Does everything
}
```

Examples are more efficient and clearer.

### Tip 3: Smart Defaults

Provide default behaviors to avoid constant questions:

```markdown
<constraints>
## Default Behaviors (no need to ask)

- Test framework: Auto-detect from package.json
- File location: Co-locate tests with source
- Import style: Match existing imports in file
- Error handling: Follow pattern in similar functions

Only ask when:
- Multiple patterns exist with no clear winner
- Decision has non-local impact
- Unsure about business logic
</constraints>
```

## Testing Your Agent

### 1. Start Small

Test with simple tasks first:
```
"Add a utility function that validates email addresses"
```

Does the agent follow its workflow? Are quality gates enforced?

### 2. Test Edge Cases

Push boundaries:
```
"Refactor this legacy code" (should ask before touching legacy/)
"Add this cool new library" (should disclose license)
"Skip tests for now" (should refuse)
```

### 3. Test Failure Modes

See how agent handles problems:
```
"The build is broken" (how does it debug?)
"Tests are failing" (does it find root cause?)
"Performance is slow" (does it profile first?)
```

### 4. Iterate Based on Usage

After each use:
1. What worked well?
2. What was frustrating?
3. What could be clearer?
4. What examples would help?

Update CLAUDE.md based on real usage.

## Metrics to Track

### Adoption Metrics
- How often is agent used?
- Who uses it? (individuals vs whole team)
- What tasks is it used for?

### Quality Metrics
- Bugs caught before production
- Test coverage trends
- Code review findings (fewer issues = working)

### Efficiency Metrics
- Time saved on tasks
- Faster PR turnaround
- Reduced back-and-forth on reviews

### Sentiment Metrics
- Do developers like using it?
- Do they trust its output?
- Would they recommend it?

## Troubleshooting Checklist

Agent not working? Check:

- [ ] Is CLAUDE.md in the right directory?
- [ ] Is CLAUDE.md under 1000 lines?
- [ ] Are instructions specific with examples?
- [ ] Are constraints clear and explicit?
- [ ] Does workflow have quality gates?
- [ ] Are success criteria measurable?
- [ ] Have you tested with real tasks?
- [ ] Are you using `/clear` between features?
- [ ] Does agent have proper context?

## Getting Help

### Ask the Agent

The agent can explain itself:
```
"What are your constraints?"
"What should I know before using you?"
"What's your workflow?"
```

### Review Existing Agents

Look at successful agents for patterns:
- What structure do they use?
- How verbose are they?
- What examples do they include?
- How do they handle edge cases?

### Iterate Continuously

Agents improve over time:
1. Use in real work
2. Notice friction points
3. Update CLAUDE.md
4. Test changes
5. Repeat

Best agents are evolved, not designed perfectly upfront.
