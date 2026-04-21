# Write Documentation

**Goal:** Produce the final `README.md` inside a descriptive folder, in pt-BR, applying the chosen template plus the skill's core principles (fidelity, efficiency, didactic tone).

**Prerequisites:** Discover complete, Analyze complete, scope classified. If any of these are unclear, go back — do not start writing.

---

## MANDATORY: Before Writing a Single Line

Read (or have recently read) these three:

1. [fidelity-rules.md](fidelity-rules.md) — Don't invent
2. [token-efficiency.md](token-efficiency.md) — Don't bloat
3. [writing-style.md](writing-style.md) — pt-BR, didactic, human

State to yourself (or in a brief pre-write checkpoint):

- **Template chosen:** [feature / refactor / bugfix / multi-phase / quick]
- **Folder path:** [absolute path]
- **Files to document:** [list from Analyze]
- **Key decisions to include:** [list from Analyze, only real ones]

---

## Process

### 1. Create the Folder

Use the absolute path the user gave + a descriptive slug in pt-BR kebab-case.

**Slug rules:** See [file-organization.md](file-organization.md).

Quick examples:

- `implementacao-login-social`
- `refatoracao-camada-auth`
- `correcao-bug-carrinho-vazio`
- `implementacao-checkout-completa` (multi-phase)

Use the Bash tool to create the folder — but first verify the parent exists:

```bash
ls "<parent-path>"           # verify parent exists
mkdir "<parent-path>/<slug>" # create the folder
```

Never overwrite an existing folder without asking. If the slug already exists, propose a suffix (`-v2`, or date) and confirm with the user.

### 2. Pick and Load the Template

Based on scope (from [scope-detection.md](scope-detection.md)):

| Scope                   | Template file                                              |
| ----------------------- | ---------------------------------------------------------- |
| Quick                   | [template-quick.md](template-quick.md)                     |
| Standard (feature)      | [template-feature.md](template-feature.md)                 |
| Standard (refactor)     | [template-refactor.md](template-refactor.md)               |
| Standard (bugfix)       | [template-bugfix.md](template-bugfix.md)                   |
| Multi-phase             | [template-multi-phase.md](template-multi-phase.md)         |

Load the chosen template mentally as a skeleton. Follow the structure, but fill it with content grounded in Analyze.

### 3. Write the README.md

Write it top-to-bottom in one pass, following the template.

**Writing order inside each section:**

1. Lead with a one-sentence summary of the section
2. Expand with a short paragraph or bullet list
3. If referring to code, link to the file — don't paste it (see [code-explanation.md](code-explanation.md))
4. Close the section — resist adding filler

**For headings:** Use pt-BR throughout. No English headings in the final doc.

### 4. Apply the Three Core Filters as You Write

| Filter         | Ask yourself                                                       | Fix                              |
| -------------- | ------------------------------------------------------------------ | -------------------------------- |
| **Fidelity**   | Is this sentence backed by code or a planning file?               | If no, delete it                 |
| **Efficiency** | Could I say this in fewer words? Am I repeating an earlier point?  | Compress                         |
| **Style**      | Would a peer reading this in 6 months understand?                  | Rewrite for clarity              |

Run these filters on every paragraph, not at the end.

### 5. Handle Code References

**Default: link, don't paste.**

```markdown
A lógica principal vive em [src/auth/SocialLoginService.ts](../../src/auth/SocialLoginService.ts).
Ela recebe o `provider` e o `authCode`, troca pelo token no provedor e persiste a sessão.
```

**Exceptions where pasting is OK:**

- The snippet is ≤5 lines AND central to understanding
- A specific type, constant, or signature that the reader must see verbatim
- A config value or env var format

**Never paste:**

- Entire files
- Long handlers
- Whole test suites
- Generated code (Prisma schemas, types)

See [code-explanation.md](code-explanation.md) and [token-efficiency.md](token-efficiency.md).

### 6. Handle Diagrams

**Only add a diagram if it genuinely clarifies the task.** Most small tasks don't need one. Multi-phase tasks almost always benefit from one.

**Default:** Inline mermaid block.

```markdown
## Fluxo

\`\`\`mermaid
flowchart LR
  A[Usuário clica em "Login"] --> B[SocialLoginService]
  B --> C[Provedor OAuth]
  C --> B
  B --> D[Persiste sessão]
\`\`\`
```

**Better (if installed):** Delegate to `mermaid-studio` skill to produce an SVG in `assets/`.

Keep diagrams minimal — one flow, not six. If you're tempted to add a second diagram, you probably don't need either.

### 7. Final Read-Through

After writing, read the doc once, top to bottom, in the role of "future me returning in 6 months". Ask:

- Do I understand what was done? (first paragraph test)
- Do I understand why? (decisions section test)
- Can I find the code? (links test)
- Is there anything I'd skim-read as filler? (delete it)

Then run [quality-checklist.md](quality-checklist.md).

### 8. Present the Result

Tell the user briefly what was created and where:

> "Pronto. A documentação foi criada em:
> `<absolute-path>/<slug>/README.md`
>
> Cobri: [lista curta das seções principais].
>
> Se quiser ajustar qualquer parte, me avise."

Do NOT paste the full doc contents into chat. The file is the deliverable. Mentioning sections is enough.

---

## Common Pitfalls

### Pitfall: Copying the Planning File Verbatim

Planning files describe what will be done. Documentation describes what was done. They overlap but are not the same. Restate in your own words, focused on the implementation reality.

### Pitfall: Over-Explaining Trivial Code

Don't write "Este componente é um React component que usa useState para gerenciar o estado local". That's obvious from the code. Write something the code doesn't say.

### Pitfall: Fabricating "Why" Statements

If the code doesn't show why and the planning file is silent, don't invent rationale. Say "A escolha de X não está documentada" or leave it out. See [fidelity-rules.md](fidelity-rules.md).

### Pitfall: Dumping Git Diff Into the Doc

Don't. Describe the change. The diff lives in git.

### Pitfall: Listing Every File in a Giant Table

List only files that matter to understanding. Group the rest ("mais 8 testes em `tests/auth/`").

### Pitfall: English Section Names

The doc is pt-BR. "Overview" → "Visão geral". "How it works" → "Como funciona". Templates already use pt-BR headings.

---

## Output Artifact

At the end of Write, you have produced exactly:

```
<user-path>/<slug>/
├── README.md           # the documentation (pt-BR)
└── assets/             # only if diagrams/images were needed
    └── <files>
```

Nothing else. No `NOTES.md`, no `TODO.md`, no drafts. One folder, one doc, optional assets.

---

## Tips

- **Templates are skeletons, not cages** — Omit sections that don't apply. Never pad to fit a template.
- **First paragraph is the whole doc in miniature** — Nail it.
- **Link > paste** — Always prefer file links over inlined code
- **Write in one pass** — Don't draft and re-draft. Apply filters as you write.
- **If the doc is longer than the source code, the doc is wrong** — Compress
- **The user should never see the raw doc content in chat** — Deliver the file, summarize the sections
