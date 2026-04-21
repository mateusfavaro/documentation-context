# Writing Style

**Language:** Brazilian Portuguese (pt-BR). Non-negotiable for output.
**Audience:** Yourself or a peer, returning to this documentation 6 months later.
**Tone:** Didactic, human, direct. No corporate filler, no hype, no apologies.

---

## The Core Voice

Write like you're explaining the task to a colleague over coffee — not like you're publishing a white paper.

**Do:**

- Use **second-person informal** (`você`) — "Você vai perceber que...", "Para testar, rode..."
- Use **present tense** for describing current behavior ("O service recebe o token e valida...")
- Use **past tense** only for historical context ("Antes da refatoração, a lógica vivia em...")
- Use **short paragraphs** — 2-4 sentences max
- Use **concrete examples** instead of abstract descriptions

**Don't:**

- Use "we" (`nós`) — it's you and the reader, not a committee
- Use formal corporate Portuguese ("Faz-se necessário", "No intuito de")
- Use English terms when pt-BR has a clean equivalent (`commit`, `merge`, `repository` são ok; `feature`, `implementation`, `overview` têm tradução)
- Start sections with "Neste capítulo/seção discutiremos..." — just start explaining

---

## Mandatory pt-BR Conversions

| English                     | pt-BR preferred                        |
| --------------------------- | -------------------------------------- |
| Overview                    | Visão geral                            |
| How it works                | Como funciona                          |
| What changed                | O que mudou                            |
| Technical decisions         | Decisões técnicas                      |
| Root cause                  | Causa raiz                             |
| Before / After              | Antes / Depois                         |
| How to test / verify        | Como verificar / Como testar           |
| Next steps                  | Próximos passos                        |
| Final state                 | Estado atual / Estado final            |
| Edge case                   | Caso limite / Caso especial            |
| Out of scope                | Fora do escopo                         |
| Workaround                  | Solução paliativa / Contorno           |
| Deprecated                  | Obsoleto / Descontinuado               |

---

## Terms to Keep in English

These are jargon so ubiquitous in Brazilian dev culture that translating hurts clarity:

- `commit`, `PR`, `merge`, `rebase`, `branch`
- `bug`, `fix`, `hotfix`
- `deploy`, `build`, `release`
- `request`, `response`, `endpoint`, `payload`
- `middleware`, `service`, `controller`, `repository`
- `hook`, `store`, `state`
- `cache`, `queue`, `worker`
- `feature flag`
- `stack trace`, `log`, `logs`

Don't force-translate these. But DO translate surrounding prose around them.

---

## Paragraph Structure

Every paragraph should answer one question. When you catch yourself writing a paragraph that answers two things, split it.

**Example of good paragraph:**

> O `SocialLoginService` recebe o provedor OAuth e o código de autorização, faz a troca pelo access token, e retorna o usuário autenticado. A lógica de redirecionamento e persistência de sessão vive em outro módulo, o que mantém esse service focado só na integração com o provedor.

**Example of bloated paragraph (avoid):**

> O `SocialLoginService` é um módulo fundamental que foi criado para lidar com toda a complexidade envolvida na autenticação via provedores externos, sendo responsável por receber parâmetros do provedor OAuth e do código de autorização, realizar a troca desses dados pelo access token necessário e, além disso, retornar as informações do usuário autenticado, mantendo também considerações sobre redirecionamento e persistência de sessão que são aspectos igualmente importantes dessa arquitetura.

Same content. First version is clearer because it makes one point per sentence.

---

## Headings

**Level hierarchy:**

- `#` — title of the doc (only one)
- `##` — main sections (Visão geral, Objetivo, Decisões técnicas)
- `###` — subsections inside a main section
- `####` — rare, avoid if possible

**Heading style:**

- Sentence case: "Como funciona na prática", not "Como Funciona Na Prática"
- No trailing punctuation
- pt-BR always

---

## Lists vs. Prose

**Use bullet lists when:**

- Items are parallel in structure (files, steps, flags)
- Order doesn't matter OR order is strictly sequential
- Prose version would be artificially joined with "além disso" / "também"

**Use prose when:**

- You're explaining cause and effect
- Ideas have meaningful connective tissue
- The explanation reads naturally as one thought

**Signal you're misusing bullets:** Every bullet starts with a verb in the same conjugation AND the bullets collectively tell a story. That's prose masquerading as a list. Convert to a paragraph.

---

## Tables

Use tables for **comparisons** and **attribute/value** pairs:

- File name → what changed
- Scope → template
- Variable → purpose → required?

Don't use tables for:

- Lists of a single column (use bullets)
- Information that's really a paragraph (e.g., "Before → After" with long prose values)

Cap tables at 4-5 columns. If wider, break into two tables or switch to subsections.

---

## Code Blocks

See [code-explanation.md](code-explanation.md) for full rules. Quick version:

- Language-tag every code block (` ```ts `, ` ```bash `, ` ```json `)
- Never embed a whole file
- Never paste unmodified diffs
- Prefer linking to the file over inlining a snippet

---

## Didactic Techniques

These make docs memorable and useful for future study:

### The "In practice" shift

After explaining concept abstractly, give a one-line "in practice" example:

> O middleware valida a sessão a cada request protegida. **Na prática, se o token expirou, o usuário é redirecionado para `/login` com o destino original preservado na query.**

### Cross-references

When mentioning a concept defined elsewhere, link it. Don't redefine it.

> Ver como o `AuthStore` é inicializado em [Fase 2](#fase-2--autenticação).

### The "por quê"

Always anchor decisions in a concrete "por quê". Vague rationale ("for better separation of concerns") is noise. Concrete rationale ("para que testes unitários possam mockar só o repositório") teaches.

---

## Anti-Patterns

### ❌ Sycophantic intro

> "Esta documentação apresenta de forma detalhada e cuidadosa a implementação..."

Don't warm up. Start with content.

### ❌ Meta-commentary

> "Como vimos acima..."
> "É importante observar que..."
> "Vale ressaltar que..."

Cut these. If it's important, just state it.

### ❌ Filler adjectives

> "robusto", "elegante", "poderoso", "moderno"

These add nothing. Delete.

### ❌ Future tense for documentation

> "O sistema irá processar a requisição..."

Use present: "O sistema processa a requisição..."

### ❌ Passive voice overuse

> "A lógica foi movida para um service"
> (sometimes ok, but usually clearer active)
> "Movemos a lógica para um service" — or even better: "A lógica agora vive em `AuthService`"

---

## Tone Calibration Examples

### Too formal

> "No contexto da presente refatoração, foi realizada a extração do componente `AuthController` com o objetivo de separar responsabilidades."

### Too casual / sloppy

> "A gente separou o controller porque tava ficando meio bagunçado kkkk"

### Just right

> "Separei o `AuthController` do `AuthService` porque a validação de sessão estava misturada com o parsing HTTP. Agora o controller só cuida de request/response e o service concentra a regra."

---

## Tips

- **One idea per sentence** — if "e" connects two full thoughts, split into two sentences
- **Active voice by default** — passive is fine when the actor truly doesn't matter
- **Cut "que", "de que", "sendo que"** whenever possible — they often mark bloat
- **Read your doc aloud** — if you stumble, rewrite
- **Ban "simplesmente", "basicamente", "essencialmente"** — they precede bloated explanations
- **The word "importante" is usually a crutch** — if it's important, show it, don't announce it
