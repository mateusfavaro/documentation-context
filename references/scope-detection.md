# Scope Detection

**Goal:** Classify the task into one of four scopes — **Quick**, **Standard**, **Multi-phase**, or **Exploratory** — so the right template is chosen and the documentation doesn't over- or under-document the work.

**When:** Right after Discover, before Write. Analyze feeds this decision.

---

## The Four Scopes

| Scope           | Size            | Planning files   | Template                                          |
| --------------- | --------------- | ---------------- | ------------------------------------------------- |
| **Quick**       | 1-3 files       | 0                | [template-quick.md](template-quick.md)           |
| **Standard**    | 4-15 files      | 0-1              | feature / refactor / bugfix (see below)          |
| **Multi-phase** | Varies          | 2+               | [template-multi-phase.md](template-multi-phase.md) |
| **Exploratory** | Unknown         | Unknown          | Decide after Analyze completes                   |

---

## Signals — What Tells You Which Scope

### Signals for Quick

- User said "fix pequeno", "mudança de config", "só alterei o .env"
- Git diff shows ≤3 files modified
- No planning file exists or was mentioned
- The whole change fits in one sentence
- Runtime behavior change is narrow (one function, one value)

### Signals for Standard

- A single feature, refactor, or substantial bug fix
- 4-15 files touched
- At most ONE planning file
- Has a clear, single "theme" (e.g., "add social login", "extract auth into service")
- Would take one developer 1-3 days

### Signals for Multi-phase

- User mentioned `.planning/`, `.specs/`, or multiple numbered files
- User said "dividi em fases", "cheguei na fase 5", "tenho vários planning"
- Changes span distinct layers (data + api + ui, etc.) and were built iteratively
- Planning files reference each other ("depende do 01-...")

### Signals for Exploratory

- User can't articulate the scope cleanly
- "Não sei bem se foi refactor ou feature"
- Mixed signals (some quick-looking, some large-feeling)
- Task summary doesn't fit in one sentence

---

## Picking the Standard Sub-Template

When scope is Standard, you still need to pick among three templates. Use these signals:

### Feature → [template-feature.md](template-feature.md)

- New capability exposed to users or to other code
- Net new files created, possibly with new types/routes/components
- User-visible or system-visible behavior added

### Refactor → [template-refactor.md](template-refactor.md)

- No behavior change intended (or minimal)
- Internal structure reorganized
- Same public API, different internals
- Or: renaming / splitting / merging modules

### Bug Fix → [template-bugfix.md](template-bugfix.md)

- Task name says "fix", "correção", "bug"
- Existing behavior was wrong; now it's right
- Usually small number of files, but the "before/after" and root cause matter more than the file list

---

## Decision Tree

```
Is there a planning file (or multiple)?
├── Yes, 2+ planning files → MULTI-PHASE
├── Yes, 1 planning file → STANDARD (pick feature/refactor/bugfix)
└── No
    ├── Is the change ≤3 files and one-sentence scope? → QUICK
    ├── Is it a clear feature/refactor/bugfix across 4-15 files? → STANDARD
    └── Still unclear? → EXPLORATORY
```

---

## Handling Exploratory Scope

If classified as Exploratory, DO NOT pick a template yet. Instead:

1. Run [analyze-task.md](analyze-task.md) with whatever partial info you have
2. After analysis, propose a scope to the user:

   > "Olhei os arquivos alterados. Isso parece na verdade um **refactor médio** (escopo Standard) — mexeu em 7 arquivos dentro de `src/auth/` e não há planning file. Vou usar o template de refactor, tudo bem?"

3. Wait for user confirmation before writing

---

## Escalation Rules

**Upgrade Quick → Standard when:**

- Analyze reveals the change touches more files than the user mentioned
- A decision worth documenting (library choice, pattern change) is found

**Upgrade Standard → Multi-phase when:**

- Analyze reveals multiple distinct phases of work in git history
- Multiple planning files exist that the user forgot to mention

**Downgrade Multi-phase → Standard when:**

- Two planning files exist but they describe a single, cohesive feature with one theme

**Downgrade Standard → Quick when:**

- Analyze reveals the "feature" is really just a config tweak hiding behind a big narrative

**Always inform the user** before escalating/downgrading. Scope changes affect the shape of the doc — the user should confirm.

---

## Template Quick-Reference

| Scope                 | Folder name pattern                      | Main `.md` size target | Sections                                                                |
| --------------------- | ---------------------------------------- | ---------------------- | ----------------------------------------------------------------------- |
| Quick                 | `<tipo>-<slug>` (e.g., `fix-login-401`)  | ≤300 words             | Contexto / O que mudou / Por quê / Verificação                         |
| Standard (feature)    | `implementacao-<slug>`                   | 400-900 words          | Objetivo / Arquivos / Fluxo / Decisões / Como funciona na prática      |
| Standard (refactor)   | `refatoracao-<slug>`                     | 400-900 words          | Objetivo / Antes / Depois / Motivação / O que continua igual           |
| Standard (bugfix)     | `correcao-<slug>`                        | 300-700 words          | Sintoma / Causa raiz / Correção / Verificação / Regressão              |
| Multi-phase           | `implementacao-<slug>-completa`          | 800-2000 words         | Visão geral / Timeline / Por fase / Decisões transversais / Estado final |

---

## Tips

- **When in doubt, go smaller** — A Quick doc that grows is better than a bloated Standard doc
- **Planning file count beats file count** — 2 planning files almost always = Multi-phase
- **One sentence scope = Quick** — If you can't describe the task in one sentence, it's not Quick
- **Confirm escalations with the user** — Don't silently change templates mid-flow
- **Template choice is reversible** — If Write reveals the template is wrong, stop and switch
