# File Organization

**Goal:** Ensure every documentation artifact lives at a predictable path with a descriptive name that makes sense both today and six months later.

---

## Output Structure

Every task produces this structure:

```
<user-specified-directory>/
└── <descriptive-slug>/
    ├── README.md               # main documentation (pt-BR)
    └── assets/                 # optional — only if diagrams/images/referenced files
        ├── diagrama-fluxo.svg
        └── screenshot-login.png
```

**Never create:**

- Multiple `.md` files (use sections inside `README.md` instead)
- `notes.md`, `draft.md`, `TODO.md` files
- Nested folders beyond `assets/`

---

## Folder Naming: The Slug

### Format

- **kebab-case** (lowercase, hyphens)
- **pt-BR** words
- **verb + noun** or **noun + qualifier**
- **no dates**, **no ticket IDs**, **no phase numbers** in the folder name
- **3-6 words** typically, never more than 8
- **no accents in the slug** (slug é ASCII — "implementacao", não "implementação")

### Patterns by Scope

| Scope / Template           | Prefix                  | Example                                               |
| -------------------------- | ----------------------- | ----------------------------------------------------- |
| Feature (Standard)         | `implementacao-`        | `implementacao-login-social`                          |
| Refactor                   | `refatoracao-`          | `refatoracao-camada-auth`                             |
| Bug fix                    | `correcao-`             | `correcao-carrinho-vazio-pos-login`                   |
| Config change              | `config-`               | `config-ci-matriz-node-versions`                      |
| Multi-phase                | `implementacao-...-completa` / `projeto-` | `implementacao-moderacao-posts-completa` |
| Generic update             | `atualizacao-`          | `atualizacao-dependencias-react-19`                   |

### Good Examples

- `implementacao-login-social`
- `refatoracao-auth-middleware`
- `correcao-bug-sessao-expirada`
- `config-deploy-staging`
- `atualizacao-prisma-5-para-6`
- `implementacao-moderacao-posts-completa`
- `refatoracao-store-carrinho`

### Bad Examples

| Bad                             | Why it's bad                          | Better                               |
| ------------------------------- | ------------------------------------- | ------------------------------------ |
| `LoginSocial`                   | CamelCase, not descriptive as action  | `implementacao-login-social`         |
| `login-social`                  | Vague — is it a feature? refactor?   | `implementacao-login-social`         |
| `feat-login-social-2025-04-20`  | Date pollutes the slug                | `implementacao-login-social`         |
| `JIRA-1234-login`               | Ticket ID — lives inside the doc     | `implementacao-login-social`         |
| `fase5-implementacao`           | Phase number in folder                | `implementacao-moderacao-completa`   |
| `implementação-login-social`    | Accents in slug                       | `implementacao-login-social`         |
| `implement-social-login`        | English                               | `implementacao-login-social`         |

---

## Where Metadata Goes (Inside README.md, Not in the Folder Name)

Move all of these into the README's frontmatter or opening section:

- Data (YYYY-MM-DD)
- Ticket / issue ID (JIRA-123, GH-456)
- Phase / fase number
- Branch / PR number
- Commit hash

Example in README.md:

```markdown
# Implementação: Login Social

> **Resumo:** Adiciona login com Google e GitHub via OAuth.

**Data:** 2026-04-20
**PR:** #342
**Ticket:** JIRA-1234
**Commit:** abc123
**Planning:** [.planning/05-login-social.md](../.planning/05-login-social.md)
```

---

## The `assets/` Subfolder

Use sparingly. Most docs don't need one.

**When to create `assets/`:**

- You have a diagram that benefits from being an SVG/PNG (e.g., a mermaid rendered by `mermaid-studio`)
- You have a screenshot that illustrates before/after
- You have a referenced image or file that the user needs to see

**When NOT to create `assets/`:**

- Your mermaid diagram is inline in the README (keep it inline)
- You have "supporting documents" you want to add — those should be merged into the README as sections

**Naming inside `assets/`:**

- kebab-case, pt-BR
- descriptive noun
- `fluxo-autenticacao.svg`, `antes-refatoracao.png`, `depois-refatoracao.png`

---

## The User-Specified Directory

The user tells you where the doc folder should live (during Discover, Question 4). You then create the subfolder there.

**Always:**

- Use an **absolute path** from the user (or confirm the relative path resolves to the expected absolute)
- Verify the parent directory exists before creating the subfolder
- Ask before overwriting an existing folder with the same slug

**Resolution rule when the user gives a relative-sounding path:**

- "docs/" → ask "relativo ao workspace? ou absoluto?"
- "/docs/" (root absolute on Unix-like) → trust it
- "C:/Users/..." → trust it
- "." or no path → ask again

**Default fallback:** If the user explicitly says "qualquer lugar" or "não sei", propose:

> `<current-workspace>/docs/tasks/<slug>/`

and wait for confirmation.

---

## Handling Duplicate Slugs

If you propose a slug and the folder already exists at the user's path:

1. **Read the existing README** briefly — is it documenting the same thing?
2. If **same task**: ask the user whether to update the existing doc or create a versioned copy
3. If **different task**: propose a more specific slug (add a disambiguator like `-refactor`, `-parte-2`)

Never silently overwrite an existing doc.

---

## Special Cases

### Multi-phase with very long slug

If the slug would exceed 8 words, compress:

- `implementacao-moderacao-de-posts-em-tempo-real-com-ml-completa` (too long)
- → `implementacao-moderacao-posts-completa` (compressed — details go inside the doc)

### When the task has no obvious verb

Use `documentacao-` or `notas-`:

- `documentacao-estrutura-microservicos` (if the task itself was creating docs)
- `notas-migracao-react-19` (if it's a log of a process)

But these are rare — most tasks fit one of the standard prefixes.

### When the task overlaps with an existing doc

If you're adding to an existing multi-phase doc as a new phase:

- Update the existing `README.md` (add a new section for the phase)
- Don't create a new folder

Ask the user to confirm before merging.

---

## Quick Reference Table

| Thing                                    | Rule                                                 |
| ---------------------------------------- | ---------------------------------------------------- |
| Folder name                              | kebab-case pt-BR, no accents, no dates, 3-6 words    |
| Main doc name                            | Always `README.md`                                   |
| Supporting files                         | `assets/` subfolder, only if genuinely needed        |
| Metadata (date, ticket, etc.)            | Inside `README.md`, never in folder name             |
| Multiple `.md` files in the same folder  | No — consolidate into `README.md` sections           |
| User path                                | Always absolute; confirm if ambiguous                |
| Parent directory must exist?             | Yes — verify with `ls` before `mkdir`                |
| Overwriting an existing folder          | Never silently; always ask                           |

---

## Tips

- **Prefix signals scope** — `implementacao-`, `refatoracao-`, `correcao-` are the three workhorses
- **Slug > metadata** — descriptive slug beats packing info into the folder name
- **README.md is non-negotiable** — it's what renders by default in GitHub/VS Code
- **One folder = one task** — if you're tempted to split, it's probably multi-phase (one folder with sections)
- **Accents kill slugs** — browsers, terminals, and file systems handle them inconsistently; stick to ASCII
- **When unsure about slug, propose two and let the user pick** — takes 10 seconds, avoids renaming later
