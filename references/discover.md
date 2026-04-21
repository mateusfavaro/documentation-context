# Discover

**Goal:** Establish WHAT task is being documented, WHERE the documentation should live, and WHAT shape it should take. Gather just enough context to pick the right scope and template — no more.

**Language rule:** Every question in this phase MUST be written in Brazilian Portuguese (pt-BR). This is non-negotiable. See [writing-style.md](writing-style.md).

---

## The 3 Mandatory Questions

Always ask these first, exactly in this order, in pt-BR. Never skip them — even if the user seems to have already given hints.

```
1. Qual task você quer documentar?
   (ex: um commit específico, uma feature recém-implementada, uma refatoração, um PR?)

2. Há arquivos de planejamento relacionados a essa task?
   (ex: arquivos em .planning/, .specs/, docs/ ou em outra pasta que eu devo revisar?
   Se houver várias fases, me aponte todos — preciso do contexto completo.)

3. Qual é o escopo da task?
   (ex: foi uma nova feature, uma refatoração, um bug fix, mudança de configuração,
   ou um conjunto de mudanças divididas em fases?)
```

Wait for the user's response before moving on. Do not answer for them — even if the scope seems obvious from an open file or a git diff.

---

## Then: The Location Question

Right after the 3 mandatory questions, ask where the documentation should live:

```
4. Em qual diretório você quer que a documentação seja criada?
   Vou criar uma subpasta com nome descritivo e o arquivo README.md dentro dela.
```

**Rules for the location:**

- Must be an absolute path, or a clear relative path from the current working directory
- If the user gives a generic answer like "na pasta de docs", confirm the exact path before writing
- Never assume a default — always confirm

---

## Scope-Specific Follow-ups

Once the user answers the 3 mandatory + location questions, decide if you need follow-ups based on the scope they gave:

### If scope is "Nova feature" or "Refatoração"

Ask only if the information isn't already clear:

- "Quais arquivos principais foram alterados ou criados? (Se preferir, posso rodar `git diff` / `git log` para descobrir sozinho.)"
- "Existe alguma decisão técnica importante que você queira destacar? (Ex: escolha de biblioteca, mudança de padrão, trade-off.)"

### If scope is "Bug fix"

- "Qual era o comportamento incorreto antes?"
- "O que passou a funcionar depois?"
- "A correção foi pontual ou envolveu mudança estrutural?"

### If scope is "Configuração"

- "Que configuração foi alterada e onde? (arquivo, variável de ambiente, setting de build?)"
- "Por que essa mudança era necessária?"

### If scope is "Multi-fase" or "Planning files"

Trigger [analyze-phases.md](analyze-phases.md) immediately. Ask:

- "Quantas fases / arquivos de planejamento existem no total? Pode listar todos?"
- "A documentação deve cobrir todas as fases consolidadas em um único doc, ou você quer separadas?"

**Default:** Consolidate all phases into one documentation file unless the user explicitly wants them separate.

### If scope is unclear ("Exploratory")

Ask probing questions until you can classify the task:

- "Me conta em uma frase o que mudou na prática para o usuário ou para o sistema."
- "Esse foi um trabalho de poucas horas (pontual) ou um projeto que você dividiu em etapas?"
- "Quais arquivos você mexeu? Me dá 3 ou 4 nomes."

Once you can classify it, confirm the scope back to the user before proceeding:

> "Entendi. Isso parece ser uma **[refatoração / feature / bug fix / etc.]** de escopo **[quick / standard / multi-phase]**. Pode confirmar para eu seguir?"

---

## Scope Signals (Internal — Use These to Classify)

Use signals from the user's answers to classify into one of the 4 scopes:

| Signal                                             | Likely Scope  |
| -------------------------------------------------- | ------------- |
| "só mudei 1 arquivo" / "é uma mudança pequena"     | Quick         |
| "fix rápido" / "bug fix pontual"                   | Quick         |
| "configurei X" / "mudei a variável Y"              | Quick         |
| "implementei uma feature" + 1 planning file        | Standard      |
| "refatorei o módulo X" + changes across 5-10 files | Standard      |
| "dividi em fases" / "tenho 3 planning files"       | Multi-phase   |
| ".planning/ tem vários arquivos"                   | Multi-phase   |
| "não sei direito por onde começar"                 | Exploratory   |

Full classification logic lives in [scope-detection.md](scope-detection.md).

---

## What NOT to Do in Discover

- ❌ Do not read code yet — that's Analyze's job
- ❌ Do not write the documentation yet — that's Write's job
- ❌ Do not propose a template before hearing the answers
- ❌ Do not translate the mandatory questions to English "to be more technical"
- ❌ Do not skip Question 2 assuming there are no planning files — the user's answer here changes the whole flow

---

## Output of Discover

By the end of this phase, you MUST have:

1. A clear statement of **what task** is being documented (one sentence)
2. A list of **planning files** to review, if any (with paths)
3. A **classified scope** (Quick / Standard / Multi-phase / Exploratory)
4. An **absolute path** for the output directory
5. A **descriptive slug** proposal for the folder name (confirm with user if unsure)

Example hand-off to Analyze:

> "Ok — vou documentar a implementação do login social (escopo: Standard feature).
> Planning file: `.planning/05-login-social.md`.
> Destino: `C:/Users/.../docs/`.
> Pasta: `implementacao-login-social`.
>
> Agora vou analisar o código e o planejamento. Já volto com a documentação."

Keep the hand-off short. The user doesn't need a wall of text — they need confirmation that you understood before you start.

---

## Tips

- **Ask all mandatory questions in one message** — not one at a time. Respect the user's flow.
- **Confirm the folder slug before writing** — slug in pt-BR kebab-case, not guessed from English.
- **If the user pastes a planning file path, verify it exists** before analysis (use Read, not just Glob).
- **Short questions beat long questions** — one sentence each.
- **Never invent context** — if the user didn't mention a planning file, don't assume there is one. Ask.
