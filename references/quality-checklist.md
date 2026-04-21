# Quality Checklist

**Goal:** Before handing the documentation back to the user, run this checklist. It catches the common failure modes — fidelity violations, bloat, language slip, and missing links.

**When:** Final step of [write-documentation.md](write-documentation.md), right before presenting the result.

---

## How to Run

1. Open the generated `README.md`
2. Read it top to bottom in the role of "future me, returning in 6 months"
3. Tick each checklist item below as you go
4. Fix every unticked item before delivery

Do not skip items. "I'll fix it later" becomes "it ships broken".

---

## Language & Tone

- [ ] Every heading is in pt-BR
- [ ] Every body paragraph is in pt-BR
- [ ] Every table header is in pt-BR
- [ ] No untranslated English prose (technical jargon like `commit`, `endpoint`, `hook` is OK)
- [ ] Second-person informal (`você`), not formal (`o senhor`, `nós`)
- [ ] No filler phrases ("é importante mencionar", "vale ressaltar")
- [ ] No hype adjectives without basis ("robusto", "elegante", "poderoso")
- [ ] Paragraphs are ≤5 sentences each
- [ ] No sentence starts with "Neste capítulo/seção..."

---

## Fidelity

- [ ] Every factual claim traces back to code or planning files
- [ ] No invented rationale for decisions
- [ ] No "próximos passos" that aren't documented in a real planning file
- [ ] No suggestions for improvements ("poderíamos ter...")
- [ ] No mentions of features that weren't implemented
- [ ] Inferences are labeled as inferences (hedged language: "segue o padrão de...")
- [ ] If something wasn't documented, the doc says so explicitly (instead of making it up)

See [fidelity-rules.md](fidelity-rules.md).

---

## Efficiency

- [ ] No entire files pasted as code blocks
- [ ] Code snippets are ≤5 lines (when present)
- [ ] Links used instead of re-pasting code where possible
- [ ] No paragraph repeats what an earlier paragraph already said
- [ ] Parallel items use tables or lists, not prose
- [ ] Doc size is within target for its scope:
  - Quick: ≤300 words
  - Standard: 400-900 words
  - Multi-phase: 800-2000 words
- [ ] No "filler" sections (sections you couldn't justify in one sentence)

See [token-efficiency.md](token-efficiency.md).

---

## Structure

- [ ] Folder name follows [file-organization.md](file-organization.md) rules
- [ ] File is named `README.md` (not `doc.md`, `documentation.md`, etc.)
- [ ] Folder path matches what the user requested
- [ ] No extra files in the folder (no `NOTES.md`, `TODO.md`, drafts)
- [ ] `assets/` folder created only if diagrams/images are actually present
- [ ] Template structure from the chosen template was followed (feature/refactor/bugfix/multi-phase/quick)

---

## Opening Paragraph

The first paragraph / summary should tell the whole story in miniature.

- [ ] There is a `> **Resumo:**` or equivalent lead sentence
- [ ] The lead sentence describes the outcome, not the process
- [ ] A reader who reads ONLY the first paragraph understands what was done
- [ ] No clichés ("Este documento descreve a implementação de...")

---

## Code & Links

- [ ] Every file mention is linked (markdown `[text](path)`)
- [ ] Relative paths resolve correctly from the doc folder
- [ ] Code blocks are language-tagged (` ```ts `, ` ```bash `, etc.)
- [ ] No pasted diffs (describe the change in prose/table, link to history)
- [ ] No pasted test files (describe coverage instead)

---

## Didactic Quality

- [ ] A peer reading this in 6 months can rebuild the mental model in <5 min
- [ ] Every decision documented has a "por quê" tied to reality
- [ ] Key flows are described (not just file lists)
- [ ] Relationships between components are explicit
- [ ] Relevant "na prática" examples where behavior isn't obvious from the code

---

## Multi-Phase Specific (skip if not multi-phase)

- [ ] All phases are listed in order
- [ ] Each phase has a one-sentence deliverable
- [ ] Timeline diagram or table is present
- [ ] Cross-phase decisions have their own section (if any exist)
- [ ] Divergences between plan and code are documented (if any exist)
- [ ] Final architecture is described at the end

---

## Bug Fix Specific (skip if not bug fix)

- [ ] Sintoma, Causa raiz, Correção are distinct sections
- [ ] Causa raiz is concrete (specific code behavior), not vague
- [ ] Reproduction steps are listed if the bug was reproducible
- [ ] Regressão prevention is honestly stated (teste novo OR "não há teste novo")

---

## Refactor Specific (skip if not refactor)

- [ ] "Antes" and "Depois" sections are present and visual (tree diffs, not just prose)
- [ ] "O que continua igual" is explicit
- [ ] Motivation is concrete (not "para seguir SRP")
- [ ] Risks are real (not generic)

---

## Final Self-Test: The Grandma Test

Read the summary paragraph + the section called "Como funciona na prática" (or equivalent).

> **Could you explain the task to a technical friend based ONLY on those two sections?**

- Yes → the doc is doing its job
- No → rewrite the summary and/or "Como funciona" — those are the load-bearing sections

---

## What to Do if Items Fail

**For language failures:**

- Rewrite the offending paragraph in pt-BR
- Never leave mixed-language prose

**For fidelity failures:**

- Delete the invented content
- If the section now feels empty, that's fine — omit it entirely
- Don't try to replace invented content with other invented content

**For efficiency failures:**

- Compress paragraphs using techniques from [token-efficiency.md](token-efficiency.md)
- Replace pasted code with links
- Convert prose lists to tables

**For structure failures:**

- Rename folders / files to match [file-organization.md](file-organization.md)
- Consolidate multiple `.md` files into `README.md`

---

## After the Checklist

When every item is ticked:

1. Save the final `README.md`
2. Announce completion to the user briefly (see [write-documentation.md](write-documentation.md) — section 8)
3. Do NOT paste the doc contents into chat
4. Do NOT add "observações finais" or self-review commentary in chat

The file speaks for itself. Your chat message just tells the user **where** it is and **what it covers** (2-3 bullets, max).

---

## Tips

- **The checklist takes 2-3 minutes** — always run it, no exceptions
- **Cutting is faster than rewriting** — when in doubt, delete
- **The opening paragraph is worth 30 seconds alone** — polish it
- **Links > prose describing location** — always
- **Fidelity > completeness** — partial but true beats complete and invented
- **If you find a major issue**, don't patch — rewrite the section
