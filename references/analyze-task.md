# Analyze: The Implemented Task

**Goal:** Reconstruct what was actually implemented — the files touched, the logic added or changed, the relationships between components, and the reasoning behind the decisions (when it can be inferred from code or planning files).

**Rule:** You describe what **is** in the codebase. You do not speculate about what could be. See [fidelity-rules.md](fidelity-rules.md).

---

## When to Use This

- Single-phase tasks (Quick, Standard scope)
- As the first analysis step, before layering on multi-phase reconstruction

For multi-phase tasks, start here and then continue with [analyze-phases.md](analyze-phases.md).

---

## Process

### 1. Identify the Change Set

The goal: a concrete list of files that were created, modified, or deleted by the task.

**Prefer these sources, in this order:**

1. **Explicit user input** — If the user said "documentar o commit abc123" or listed files, use that as ground truth
2. **Git log / diff** — If the task maps to a commit or PR:
   ```bash
   git log --oneline -n 10
   git show <hash> --stat
   git diff <base>..<head> --stat
   ```
3. **Planning file references** — Planning files often list the files to touch. Cross-check against the actual codebase.
4. **Recent file modifications** — As a last resort, look at most recently modified files in the area the user mentioned.

**Output of this step:** A list like:

```
- src/auth/SocialLoginService.ts       [CREATED]
- src/auth/AuthRouter.tsx              [MODIFIED]
- src/types/auth.ts                    [MODIFIED]
- tests/auth/social-login.test.ts      [CREATED]
```

### 2. Read Each Changed File

Read only what's needed — not the entire codebase.

**For CREATED files:** Read the whole file.

**For MODIFIED files:** Read the file AND review the diff. The diff tells you what changed; the full file tells you how it fits in.

**For DELETED files:** Note what was removed (from git history or the planning file) — you don't need the content, just the purpose.

### 3. Identify Relationships

For each changed file, answer:

- **What does it do?** (one sentence, behavior-level, not line-by-line)
- **What does it depend on?** (imports, types, other modules)
- **Who depends on it?** (grep for imports of this module, if relevant)
- **What pattern does it follow?** (is it a service, a hook, a component, a middleware?)

You don't need to write this down yet — you're building the mental model for Write.

### 4. Extract Technical Decisions

Look for decisions worth documenting. Signals include:

- A comment explaining why a certain approach was chosen
- A non-obvious pattern (e.g., using a queue where a direct call would be simpler)
- A library or API introduced for the first time in the codebase
- A trade-off visible in the code (e.g., "we skip validation here because X")
- Planning file sections titled "Decisão", "Trade-off", "Por quê", "Rationale"

**Only document decisions that are actually reflected in the code or planning files.** If you *think* a decision was made but can't find evidence, don't write it down. Ask the user during Discover if you need clarity.

### 5. Map the "Flow"

For any task bigger than a config change, there's a flow — a sequence of things that happen at runtime or at build time.

Examples:

- **Feature:** User clicks button → handler fires → service called → API hit → state updated → UI renders
- **Refactor:** Old entry point still works → internally delegates to new module → new module uses new pattern
- **Bug fix:** Bad input reaches function → validation now catches it → graceful error returned

Sketch the flow mentally (or in a small mermaid diagram if it genuinely clarifies). You'll need it in [code-explanation.md](code-explanation.md).

### 6. Note What's Out of Scope

As you read code, you'll see things that could be improved — dead code, duplicated logic, outdated comments, etc. **Do not document those.** They are not part of this task.

If they seem relevant enough to mention, make a mental note and flag them briefly in your Write phase as "notas fora do escopo" — but keep that section short or omit it entirely. See [fidelity-rules.md](fidelity-rules.md).

---

## What to Look For by Task Type

### Feature

- Entry point: What triggers this feature? (route, button, event, cron)
- Core modules: Which files implement the new capability?
- Data flow: Where does data enter, transform, and exit?
- Integration points: What existing systems does it plug into?

### Refactor

- Before: What was the structure / pattern before?
- After: What's the new structure / pattern?
- Compatibility: Is the public API the same? What broke, if anything?
- Scope: Was it incremental (one module) or sweeping (cross-cutting)?

### Bug Fix

- Root cause: What was wrong?
- Fix location: Which line(s) / function(s) changed?
- Verification: How is the fix proven to work? (test, manual step, logs)
- Regression: Is there a test guarding against this?

### Config Change

- File(s): `.env`, `config.ts`, `package.json`, CI YAML, etc.
- Before / after values
- Effect: What behavior changes at runtime or build time?

---

## Tools to Use

**Primary:**

- `Read` — Read individual files (always prefer over Bash `cat`)
- `Grep` — Find references, imports, usages
- `Glob` — Discover file patterns
- `Bash` (selective) — `git log`, `git diff`, `git show` for history

**Delegate when scope is large:**

- `Explore` subagent — When the change set spans 15+ files or you need to map an unfamiliar area
- `codenavi` skill — If installed, prefer for navigation in a large codebase

**Avoid:**

- Reading directories you don't need
- Dumping entire files into context when only a function matters
- Running `grep -r` over the whole repo when you know the folder

---

## Output of Analyze

By the end, you should be able to answer these in your head (no need to write them down yet):

1. **What was the objective** of this task? (one sentence)
2. **Which files changed** and why? (list with one-line rationale each)
3. **What's the flow** at runtime or build time?
4. **What technical decisions** were made, and why? (only if evidence exists)
5. **What's new vs. what was adapted?** (created files vs modified files)

If you can't answer any of these, go back and read more. If you still can't, ask the user.

---

## Tips

- **Read the planning file first** — It usually tells you what the code intends. Use it as a map for the code.
- **Grep before assuming** — If you think "this is the only place X is used", verify with Grep.
- **Diff is your friend** — Git diffs compress hundreds of lines of file into "here's what changed".
- **Don't read tests unless the task includes them** — Tests are valuable signals but optional context. Read them only if they clarify behavior.
- **Flag uncertainty early** — If a decision rationale isn't clear, note it and ask the user during Write, don't invent one.
