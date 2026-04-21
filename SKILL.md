---
name: documentation-context
description: Generate learning-focused documentation for an already-implemented task with 4 adaptive phases — Discover, Analyze, Structure, Write. Auto-sizes depth by task complexity. Creates descriptive folders with a single .md file explaining what was done, why, and how the code works. Always writes user-facing content and output in Brazilian Portuguese (pt-BR). Stays faithful to implementation — never invents improvements or logic that was not implemented. Use when (1) Documenting a just-finished feature, (2) Documenting a bug fix or refactor, (3) Documenting a task split across multiple planning files or phases, (4) Creating didactic study material from a recent commit or PR, (5) Capturing the "why" behind a change for future review. Triggers on "documentar task", "gerar documentação", "document feature", "document refactor", "document bug fix", "criar documentação", "documentar implementação", "documentar commit", "documentar PR". Do NOT use for API reference generation, inline code comments, or README files (those are separate concerns).
license: CC-BY-4.0
metadata:
  author: Mateus Favaro
  version: 2.0.0
---

# Documentation Context — Task Documentation Generator

Generate didactic documentation for completed tasks. Faithful to implementation. Written in pt-BR. Focused on learning and future review.

```
┌──────────┐   ┌──────────┐   ┌───────────┐   ┌─────────┐
│ DISCOVER │ → │ ANALYZE  │ → │ STRUCTURE │ → │  WRITE  │
└──────────┘   └──────────┘   └───────────┘   └─────────┘
   required      required       required*      required

* Structure reduces to template selection for small tasks
```

## Language Rule (Non-Negotiable)

**All user-facing output MUST be written in Brazilian Portuguese (pt-BR).** This includes:

- Every question asked to the user during Discover
- Every section, heading, and body text inside the generated `.md` file
- Folder names (use descriptive pt-BR slugs in kebab-case)
- Bullet points, tables, captions

Internal reasoning, tool calls, and reference files remain in English. Only the **deliverables and conversation with the user** are pt-BR. See [writing-style.md](references/writing-style.md).

## Auto-Sizing: The Core Principle

**The task's complexity determines the documentation depth, not a fixed template.** Before writing, assess the scope and apply only what's needed:

| Scope            | What                                          | Discover                                              | Analyze                                             | Structure                     | Write                                                    |
| ---------------- | --------------------------------------------- | ----------------------------------------------------- | --------------------------------------------------- | ----------------------------- | -------------------------------------------------------- |
| **Quick**        | 1-3 files, bug fix or config change           | 3 mandatory questions, brief                          | Git diff + changed files only                       | [template-quick](references/template-quick.md) | 1 folder, 1 `.md`, ≤300 words               |
| **Standard**     | Single feature or refactor, one planning file | 3 mandatory + scope-specific follow-ups               | Full code + single planning file review             | Pick feature/refactor/bugfix  | Full template with all sections                          |
| **Multi-phase**  | Task split across planning files / phases     | 3 mandatory + phase inventory                         | All planning files, chronological reconstruction    | [template-multi-phase](references/template-multi-phase.md) | Consolidated doc covering every phase |
| **Exploratory**  | Unclear scope, user wants help classifying    | 3 mandatory + probing questions until scope is clear  | Iterative — expand as scope clarifies               | Decided after Analyze         | After confirming template with user                       |

**Rules:**

- **Discover, Analyze, and Write are always required** — even in Quick mode
- **Structure is condensed** for Quick mode (template is auto-picked)
- **Multi-phase ALWAYS reviews all previous phases**, never just the last step
- **Exploratory mode** defers template choice until scope is understood
- **Quick mode** is the express lane — for 1-3 files, no planning files involved

**Safety valve:** If during Write you realize the task actually touches more than you documented, STOP and escalate to the next scope tier. Do not force a small template onto a large task. See [scope-detection.md](references/scope-detection.md).

## Output Structure

```
<user-specified-directory>/
└── <descriptive-slug>/
    ├── README.md               # The main documentation file (pt-BR)
    └── assets/                 # Optional: diagrams, screenshots
        └── ...
```

**Folder naming (see [file-organization.md](references/file-organization.md)):**

- Descriptive kebab-case slug in pt-BR
- Verb + noun (e.g., `implementacao-login-social`, `refatoracao-auth-middleware`, `correcao-bug-carrinho-vazio`)
- No dates, no phase numbers, no ticket IDs in the folder name — those go inside the `.md`

**File naming:**

- Main doc is always `README.md` (so it renders as the folder's default view on GitHub/VS Code)
- Assets in `assets/` subfolder only if genuinely needed

## Workflow

**Every task follows the same loop:**

1. **Discover** → Ask the 3 mandatory questions in pt-BR, plus scope-specific follow-ups ([discover.md](references/discover.md))
2. **Analyze** → Read the implementation (code, git diff, planning files). For multi-phase tasks, reconstruct the full story ([analyze-task.md](references/analyze-task.md), [analyze-phases.md](references/analyze-phases.md))
3. **Structure** → Classify scope and pick the right template ([scope-detection.md](references/scope-detection.md))
4. **Write** → Create folder + `README.md`, apply fidelity and efficiency rules ([write-documentation.md](references/write-documentation.md))

**After writing:** Run the [quality-checklist.md](references/quality-checklist.md) before presenting to the user.

## Context Loading Strategy

**Base load:**

- User's current request
- 3 mandatory Discover answers

**On-demand load:**

- Planning files (all of them, if multi-phase — see [analyze-phases.md](references/analyze-phases.md))
- Changed source files (only the ones touched by the task)
- Related files the changes depend on (imports, types, interfaces)
- Git log / diff for the commit or PR range

**Never load:**

- Unrelated files "just in case"
- Entire directories when only a few files changed
- Dependency source (node_modules, vendor, etc.)

**Target:** <30k tokens loaded. Documentation is a synthesis task — reading too much dilutes the output.

## Commands

| Trigger Pattern                                                                     | Reference                                                    |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Documentar task, gerar documentação, document the last task                         | [discover.md](references/discover.md)                        |
| Analisar código, revisar implementação, what was changed                            | [analyze-task.md](references/analyze-task.md)                |
| Task dividida em fases, planning files, documentar fluxo completo                   | [analyze-phases.md](references/analyze-phases.md)            |
| Classificar escopo, decidir template, é feature ou refactor                         | [scope-detection.md](references/scope-detection.md)          |
| Escrever documentação, gerar arquivo final                                          | [write-documentation.md](references/write-documentation.md)  |
| Template de feature                                                                 | [template-feature.md](references/template-feature.md)        |
| Template de refactor                                                                | [template-refactor.md](references/template-refactor.md)      |
| Template de bug fix                                                                 | [template-bugfix.md](references/template-bugfix.md)          |
| Template multi-fase                                                                 | [template-multi-phase.md](references/template-multi-phase.md) |
| Template quick                                                                      | [template-quick.md](references/template-quick.md)            |
| Regras de escrita, tom didático, pt-BR                                              | [writing-style.md](references/writing-style.md)              |
| Como explicar código de forma didática                                              | [code-explanation.md](references/code-explanation.md)        |
| Eficiência de tokens, resumir vs copiar                                             | [token-efficiency.md](references/token-efficiency.md)        |
| Não inventar, ficar fiel ao código                                                  | [fidelity-rules.md](references/fidelity-rules.md)            |
| Organização de arquivos e pastas                                                    | [file-organization.md](references/file-organization.md)      |
| Checklist final de qualidade                                                        | [quality-checklist.md](references/quality-checklist.md)      |

## Core Principles (Load Before Writing)

Read the first three before every Write phase. They define the behavioral bias of the skill.

1. **[fidelity-rules.md](references/fidelity-rules.md)** — Never invent, never suggest improvements, never document behavior that wasn't implemented. Documentation describes what **is**, not what **could be**.

2. **[token-efficiency.md](references/token-efficiency.md)** — Explain behavior, do not reproduce code. Summarize instead of copying. Link to the file, don't paste 80 lines of it.

3. **[writing-style.md](references/writing-style.md)** — pt-BR, second-person informal (`você`), didactic tone, clear paragraphs, human language. Write like you're teaching a peer who will revisit this in 6 months.

## Integration with Other Skills

### Code Exploration → codenavi / Explore subagent

When analyzing an unfamiliar area of the codebase (multi-phase tasks, large features), prefer delegating exploration to the `Explore` subagent or the `codenavi` skill if installed. This keeps the main context lean for synthesis.

### Diagrams → mermaid-studio

When the documentation benefits from a visual (architecture, data flow, sequence of phases), check if `mermaid-studio` is installed. If yes, delegate diagram creation. Otherwise, use inline mermaid code blocks. Keep diagrams minimal — one per doc is usually enough.

## Output Behavior

**Token economy matters.** The goal is clear, didactic, concise documentation for personal study and future review. A reader should be able to:

- Understand the task's **objective** in the first paragraph
- Understand **what changed** without reading the source code
- Understand **why** the solution works and how parts relate
- Return 6 months later and rebuild the mental model in under 5 minutes

If the documentation can't do that, rewrite it before delivering.

## The Prime Directive

> **Documenting is not re-implementing.** You are describing what already exists. Every sentence must trace back to something actually in the codebase or the planning files. If you are tempted to add "we should also..." or "a better approach would be..." — stop. That belongs in a separate task, not in this documentation.

See [fidelity-rules.md](references/fidelity-rules.md) for the full guardrails.
