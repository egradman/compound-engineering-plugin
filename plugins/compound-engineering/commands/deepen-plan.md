---
name: deepen-plan
description: Enhance a plan with parallel research agents for each section to add depth, best practices, and implementation details
argument-hint: "[path to plan file]"
---

# Deepen Plan - Power Enhancement Mode

## Introduction

**Note: The current year is 2025.** Use this when searching for recent documentation and best practices.

This command takes an existing plan (from `/workflows:plan`) and enhances each section with parallel research agents. Each major element gets its own dedicated research sub-agent to find:
- Best practices and industry patterns
- Performance optimizations
- UI/UX improvements (if applicable)
- Quality enhancements and edge cases
- Real-world implementation examples

The result is a deeply grounded, production-ready plan with concrete implementation details.

## Plan File

<plan_path> #$ARGUMENTS </plan_path>

**If the plan path above is empty:**
1. Check for recent plans: `ls -la plans/`
2. Ask the user: "Which plan would you like to deepen? Please provide the path (e.g., `plans/my-feature.md`)."

Do not proceed until you have a valid plan file path.

## Main Tasks

### 1. Parse and Analyze Plan Structure

<thinking>
First, read and parse the plan to identify each major section that can be enhanced with research.
</thinking>

**Read the plan file and extract:**
- [ ] Overview/Problem Statement
- [ ] Proposed Solution sections
- [ ] Technical Approach/Architecture
- [ ] Implementation phases/steps
- [ ] Code examples and file references
- [ ] Acceptance criteria
- [ ] Any UI/UX components mentioned

**Create a section manifest:**
```
Section 1: [Title] - [Brief description of what to research]
Section 2: [Title] - [Brief description of what to research]
...
```

### 2. Launch Parallel Research Agents

<thinking>
For each major section, spawn a dedicated sub-agent to research improvements. Run all agents in parallel for maximum efficiency.
</thinking>

**For each identified section, launch a parallel Task agent:**

Launch these agents **simultaneously** using Task tool with appropriate subagent_type:

#### Agent Categories by Section Type:

**For Architecture/Technical sections:**
- Task best-practices-researcher: "Research best practices for [section topic]. Find industry patterns, performance considerations, and common pitfalls."
- Task framework-docs-researcher: "Find official documentation and examples for [technologies mentioned]."

**For Implementation/Code sections:**
- Task pattern-recognition-specialist: "Analyze patterns for [implementation approach]. Find optimal code structures and anti-patterns to avoid."
- Task performance-oracle: "Research performance implications of [approach]. Find optimization strategies and benchmarks."

**For UI/UX sections:**
- Task best-practices-researcher: "Research UI/UX best practices for [component type]. Find accessibility standards, responsive patterns, and user experience improvements."

**For Data/Models sections:**
- Task data-integrity-guardian: "Research data modeling best practices for [model type]. Find validation patterns, indexing strategies, and migration safety."

**For Security-sensitive sections:**
- Task security-sentinel: "Research security considerations for [feature]. Find OWASP patterns, authentication best practices, and vulnerability prevention."

### 3. Collect and Synthesize Research

<thinking>
Wait for all parallel agents to complete, then synthesize their findings into actionable enhancements for each section.
</thinking>

**For each agent's findings:**
- [ ] Extract concrete recommendations
- [ ] Note specific code patterns or examples
- [ ] Identify performance metrics or benchmarks
- [ ] List relevant documentation links
- [ ] Capture edge cases discovered

### 4. Enhance Plan Sections

<thinking>
Merge research findings back into the plan, adding depth without changing the original structure.
</thinking>

**Enhancement format for each section:**

```markdown
## [Original Section Title]

[Original content preserved]

### Research Insights

**Best Practices:**
- [Concrete recommendation 1]
- [Concrete recommendation 2]

**Performance Considerations:**
- [Optimization opportunity]
- [Benchmark or metric to target]

**Implementation Details:**
```[language]
// Concrete code example from research
```

**Edge Cases:**
- [Edge case 1 and how to handle]
- [Edge case 2 and how to handle]

**References:**
- [Documentation URL 1]
- [Documentation URL 2]
```

### 5. Add Enhancement Summary

At the top of the plan, add a summary section:

```markdown
## Enhancement Summary

**Deepened on:** [Date]
**Sections enhanced:** [Count]
**Research agents used:** [List]

### Key Improvements
1. [Major improvement 1]
2. [Major improvement 2]
3. [Major improvement 3]

### New Considerations Discovered
- [Important finding 1]
- [Important finding 2]
```

### 6. Update Plan File

**Write the enhanced plan:**
- Preserve original filename
- Add `-deepened` suffix if user prefers a new file
- Update any timestamps or metadata

## Output Format

Update the plan file in place (or create `plans/<original-name>-deepened.md` if requested).

## Quality Checks

Before finalizing:
- [ ] All original content preserved
- [ ] Research insights clearly marked and attributed
- [ ] Code examples are syntactically correct
- [ ] Links are valid and relevant
- [ ] No contradictions between sections
- [ ] Enhancement summary accurately reflects changes

## Post-Enhancement Options

After writing the enhanced plan, use the **AskUserQuestion tool** to present these options:

**Question:** "Plan deepened at `[plan_path]`. What would you like to do next?"

**Options:**
1. **View diff** - Show what was added/changed
2. **Run `/plan_review`** - Get feedback from reviewers on enhanced plan
3. **Start `/workflows:work`** - Begin implementing this enhanced plan
4. **Deepen further** - Run another round of research on specific sections
5. **Revert** - Restore original plan (if backup exists)

Based on selection:
- **View diff** → Run `git diff [plan_path]` or show before/after
- **`/plan_review`** → Call the /plan_review command with the plan file path
- **`/workflows:work`** → Call the /workflows:work command with the plan file path
- **Deepen further** → Ask which sections need more research, then re-run those agents
- **Revert** → Restore from git or backup

## Example Enhancement

**Before (from /workflows:plan):**
```markdown
## Technical Approach

Use React Query for data fetching with optimistic updates.
```

**After (from /workflows:deepen-plan):**
```markdown
## Technical Approach

Use React Query for data fetching with optimistic updates.

### Research Insights

**Best Practices:**
- Configure `staleTime` and `cacheTime` based on data freshness requirements
- Use `queryKey` factories for consistent cache invalidation
- Implement error boundaries around query-dependent components

**Performance Considerations:**
- Enable `refetchOnWindowFocus: false` for stable data to reduce unnecessary requests
- Use `select` option to transform and memoize data at query level
- Consider `placeholderData` for instant perceived loading

**Implementation Details:**
```typescript
// Recommended query configuration
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      retry: 2,
      refetchOnWindowFocus: false,
    },
  },
});
```

**Edge Cases:**
- Handle race conditions with `cancelQueries` on component unmount
- Implement retry logic for transient network failures
- Consider offline support with `persistQueryClient`

**References:**
- https://tanstack.com/query/latest/docs/react/guides/optimistic-updates
- https://tkdodo.eu/blog/practical-react-query
```

NEVER CODE! Just research and enhance the plan.
