# /research - Gather External Information

Research technologies, libraries, patterns, and best practices before making decisions.

---

## When to Use

- Evaluating libraries or frameworks
- Understanding best practices
- Comparing approaches
- Finding how others solved similar problems
- Checking for gotchas before adopting something new

**Not for**: Internal ideation (use `/brainstorm`) or codebase exploration (use `/next`)

---

## Process

### Step 1: Clarify the Question

Before searching, understand:

1. **What do you need to know?** - State the specific question
2. **What decision will this inform?** - Context for relevance filtering
3. **What constraints matter?** - License, performance, compatibility, etc.

If unclear, ask: "What specifically do you want to learn about [topic]?"

### Step 2: Search

Use built-in tools to gather information:

```
WebSearch - Find relevant sources
WebFetch - Read documentation, articles, discussions
```

**Search strategy by topic type:**

| Topic | Search Approach |
|-------|-----------------|
| Library selection | "[need] library [language] comparison 2024" |
| Best practices | "[topic] best practices [framework]" |
| How to implement | "how to [task] [framework] example" |
| Gotchas/issues | "[library] common issues site:stackoverflow.com" |
| Official docs | "[library] documentation official" |

### Step 3: Evaluate Sources

Prioritize by credibility:

1. **Official documentation** - Most authoritative
2. **GitHub repos** - Check stars, recent activity, issues
3. **Stack Overflow** - Check votes, accepted answers, dates
4. **Blog posts** - Check author credibility, date, depth
5. **Random forums** - Use with caution, verify claims

**Red flags:**
- Outdated (>2 years for fast-moving tech)
- No sources or examples
- Contradicts official docs
- Single opinion without evidence

### Step 4: Synthesize Findings

Organize research into clear sections:

```markdown
## Research: [Topic]

### Question
[What we're trying to learn]

### Context
[Why this matters, what constraints exist]

### Findings

#### [Option/Topic 1]
- **What**: [Brief description]
- **Source**: [Link to official docs]
- **Pros**: [Benefits]
- **Cons**: [Drawbacks]
- **Gotchas**: [Common issues found]

#### [Option/Topic 2]
...

### Comparison (if applicable)
| Aspect | Option A | Option B |
|--------|----------|----------|
| Performance | Fast | Faster |
| Bundle size | 10kb | 50kb |
| Learning curve | Low | Medium |

### Recommendation
[Your recommendation based on findings and context]

### Sources
1. [Title](URL) - [Brief note on what it covers]
2. [Title](URL) - [Brief note]
```

### Step 5: Save Research

Save valuable research for future reference:

```bash
mkdir -p docs/research
```

Save to: `docs/research/[topic]-YYYY-MM.md`

**When to save:**
- Significant decision (library choice, architecture pattern)
- Findings will be referenced later
- Team might need this information

**When NOT to save:**
- Quick lookup (just report findings)
- Information is trivial or obvious
- Already documented elsewhere

---

## Output Formats

### Quick Mode (default)
Report findings directly, no file created.

### Save Mode
Create `docs/research/[topic]-YYYY-MM.md` with full findings.

---

## Examples

### Example 1: Library Comparison
```
User: /research state management options for React

Claude:
1. Searches: "React state management comparison 2024"
2. Fetches: Redux docs, Zustand docs, Jotai docs, comparison articles
3. Synthesizes: Comparison table with bundle size, learning curve, use cases
4. Recommends: Based on project constraints
5. Saves: docs/research/react-state-management-2024-01.md
```

### Example 2: Best Practices
```
User: /research JWT refresh token best practices

Claude:
1. Searches: "JWT refresh token security best practices"
2. Fetches: OWASP guidelines, Auth0 docs, security blogs
3. Synthesizes: Key recommendations, common mistakes, implementation patterns
4. Reports: Findings with sources
```

### Example 3: Quick Lookup
```
User: /research how to handle file uploads in FastAPI

Claude:
1. Searches: "FastAPI file upload example"
2. Fetches: Official FastAPI docs
3. Reports: Code pattern with explanation
4. No save: Simple lookup, official docs sufficient
```

---

## Integration with Other Commands

```
/research  →  Gather external information
    ↓
/brainstorm  →  Generate options (informed by research)
    ↓
/spec  →  Create specification
    ↓
/plan  →  Create implementation plan
```

Research can also feed into:
- `/learn` - Document decisions made based on research
- `docs/decisions/` - ADRs referencing research findings

---

## Boundaries

**This command WILL:**
- Search web for relevant information
- Read and synthesize documentation
- Compare options with evidence
- Provide recommendations with reasoning
- Save research for future reference

**This command will NOT:**
- Make decisions without presenting options
- Implement anything (use implementation commands)
- Guess when information isn't found (will say "couldn't find")
- Use outdated sources without noting the date

---

## Research Quality Checklist

Before presenting findings:
- [ ] Multiple sources consulted (not just one)
- [ ] Official documentation checked
- [ ] Source dates verified (recent enough?)
- [ ] Gotchas and issues researched
- [ ] Sources cited with links
- [ ] Recommendation explains reasoning
