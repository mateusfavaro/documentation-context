# Analyze: Multi-Phase Tasks

**Goal:** When a task is split across multiple planning files or implementation phases, reconstruct the full story — not just the final step. The documentation must reflect the complete journey, even if the user just finished Phase 5.

**Trigger:** Scope is Multi-phase (identified in [discover.md](discover.md)) OR the user mentions ".planning/", ".specs/", phase numbers, or multiple planning files.

---

## Why This Matters

A task divided into phases has context that a single commit does not capture:

- **Phase 1** might define the data model
- **Phase 2** might set up the infrastructure
- **Phase 3** might implement the core logic
- **Phase 4** might add the UI
- **Phase 5** might wire everything together

If you only document Phase 5, the reader sees the final wiring but has no idea why the pieces were built the way they were. Documentation must preserve the full arc.

**Rule:** If planning files 1 through N exist, read ALL of them. Do not skip to the last one.

---

## Process

### 1. Inventory All Planning Files

Ask the user (during Discover) or discover them yourself:

```bash
# Common locations — use Glob
.planning/**/*.md
.specs/**/*.md
docs/planning/**/*.md
```

Output: a complete, ordered list:

```
.planning/01-data-model.md
.planning/02-api-endpoints.md
.planning/03-business-logic.md
.planning/04-ui-components.md
.planning/05-integration.md
```

**If phase numbers are missing or files aren't ordered**, infer order from:

- File modification dates
- References inside the files ("antes de seguir, ver 01-data-model.md")
- Content (phase 1 usually introduces concepts; later phases assume them)

Confirm the order with the user if you're unsure.

### 2. Read Every Planning File

Read them **in order**, not in parallel. The point is to follow the progression.

For each file, capture (mentally or in notes):

- **What this phase delivered** — the concrete outcome
- **Decisions made** — architecture, library, pattern, naming
- **Dependencies** — what earlier phases this assumes
- **Blockers resolved** or deferred — if mentioned
- **Files touched** — cross-check against code later

### 3. Reconstruct the Timeline

Build a mental timeline:

```
Phase 1 (data-model)    → defined User, Post, Auth entities
Phase 2 (api-endpoints) → scaffolded CRUD routes using Phase 1 entities
Phase 3 (business-logic)→ added validation + moderation service
Phase 4 (ui-components) → built Post list + composer using Phase 2 API
Phase 5 (integration)   → wired moderation hooks into Post composer flow
```

This timeline becomes the spine of the documentation in [template-multi-phase.md](template-multi-phase.md).

### 4. Cross-Reference with Code

For each phase, identify the **actual files in the codebase** that correspond to it.

Example:

| Phase              | Planning File                 | Implementation Files                           |
| ------------------ | ----------------------------- | ---------------------------------------------- |
| 1. Data model      | `.planning/01-data-model.md`  | `src/entities/`, `prisma/schema.prisma`       |
| 2. API endpoints   | `.planning/02-api-endpoints.md` | `src/api/routes/`, `src/api/controllers/`    |
| 3. Business logic  | `.planning/03-business-logic.md` | `src/services/moderation/`                  |
| 4. UI components   | `.planning/04-ui-components.md` | `src/components/posts/`                     |
| 5. Integration     | `.planning/05-integration.md` | `src/components/posts/Composer.tsx` (modified)|

This mapping is gold — it answers "where is each phase reflected in the code?"

### 5. Identify Cross-Phase Decisions

Some decisions span multiple phases. Examples:

- "We chose Zustand over Redux in Phase 2 and used it throughout Phases 3-5."
- "We renamed `User` to `Account` in Phase 3 after realizing domain language."
- "Moderation was originally planned for Phase 2, deferred to Phase 3, and finally shipped in Phase 3."

These decisions deserve a dedicated section in the final documentation because they explain the shape of multiple phases at once.

### 6. Handle Diverged Plans (Plan ≠ Code)

Sometimes the implementation diverges from the planning file:

- A feature was added that wasn't planned
- A decision was reversed mid-phase
- A phase was combined with another

**When this happens:**

1. Trust the **code** as the source of truth for what was built
2. Use the **planning file** to understand the original intent
3. Note the divergence briefly in the final doc, if it's meaningful
4. If you're unsure why the divergence happened, ask the user before writing

Do not silently document the planned-but-not-implemented behavior. That violates [fidelity-rules.md](fidelity-rules.md).

---

## Output of Multi-Phase Analyze

Before moving to Write, you should have:

1. **Ordered list of all phases** (planning file → implementation files)
2. **One-sentence summary per phase** describing its deliverable
3. **Cross-phase decisions** that shape the overall architecture
4. **Known divergences** between plan and code (if any)
5. **Final state** — what the system does now, post all phases

---

## Template Hand-off

Multi-phase analyses feed directly into [template-multi-phase.md](template-multi-phase.md), which has dedicated sections for:

- Timeline / progression
- Per-phase breakdown
- Cross-phase decisions
- Final architecture

If the scope turns out to be simpler than multi-phase (e.g., only 2 planning files that essentially describe one feature), downgrade to [template-feature.md](template-feature.md) and inline a "Etapas da implementação" section.

---

## Tips

- **Always read in order** — Phase 1 before Phase 5, not the other way around
- **The last phase isn't the whole story** — It's just the most recent commit
- **Divergences are normal** — Plans evolve; document what shipped
- **Cross-phase decisions deserve their own section** — They're what make the doc valuable for future review
- **When a phase is thin (e.g., a 3-line planning file)**, merge it into the adjacent phase in the doc instead of inventing content to fill it
- **If 6+ phases exist**, consider whether to group them. For example, Phases 1-2 "Foundation" + Phases 3-4 "Core" + Phase 5 "Integration"
- **Escalate to user if phases contradict each other** — Don't paper over real contradictions with invented reconciliation
