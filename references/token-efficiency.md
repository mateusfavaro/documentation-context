# Token Efficiency

**Goal:** Write documentation that conveys maximum understanding per token. Documentation bloat is the #1 reason study docs get ignored — a doc that takes 15 minutes to read never gets reread. Compressing without losing meaning is the core skill.

---

## The Three Rules

1. **Explain behavior, don't reproduce code.**
2. **Summarize, don't copy.**
3. **If the reader can derive it from the code or from context, don't write it.**

---

## Rule 1: Explain Behavior, Not Code

**Bad (copying):**

> O `SocialLoginService` tem um método `iniciar` que recebe um parâmetro `provider: string`, valida se ele está na lista `SUPPORTED_PROVIDERS`, chama a função `buildOAuthUrl(provider)` passando o parâmetro, e retorna o resultado dessa função.

**Good (behavior):**

> `SocialLoginService.iniciar(provider)` valida o provedor contra a allowlist e retorna a URL OAuth de redirecionamento.

Second version is 60% shorter and more useful. The code has the details; the doc has the concept.

---

## Rule 2: Summarize, Don't Copy

**Bad (copying):**

> Os testes adicionados cobrem: teste 1 — carrinho vazio; teste 2 — carrinho com 1 item; teste 3 — carrinho com múltiplos itens; teste 4 — carrinho após login; teste 5 — carrinho após logout; teste 6 — carrinho após expiração; teste 7 — carrinho com item removido.

**Good (summary):**

> Foram adicionados 7 testes cobrindo os cenários principais de ciclo de vida do carrinho (vazio, com itens, após login/logout, após expiração). Ver [`cart.test.ts`](...).

---

## Rule 3: Don't State the Derivable

**Bad (derivable):**

> O arquivo `UserRepository.ts` está localizado em `src/repositories/UserRepository.ts` e contém a classe `UserRepository` que é exportada como export default.

**Good (link does this):**

> [`UserRepository`](../../src/repositories/UserRepository.ts) encapsula as queries de usuário.

The path is in the link. The filename implies the class. Export default is a detail most readers don't need.

---

## Compression Techniques

### 1. Collapse parallel items

**Bloated:**

> A Fase 1 entregou o modelo de dados. A Fase 2 entregou os endpoints. A Fase 3 entregou a lógica de negócio. A Fase 4 entregou a UI. A Fase 5 integrou tudo.

**Compressed (table):**

| Fase | Entrega                 |
| ---- | ----------------------- |
| 1    | Modelo de dados         |
| 2    | Endpoints               |
| 3    | Lógica de negócio       |
| 4    | UI                      |
| 5    | Integração              |

Same content, one-third the words, easier to scan.

### 2. Cut meta-commentary

**Bloated:**

> É importante notar que, conforme mencionado anteriormente, a escolha pela biblioteca X foi, como já discutido, motivada por...

**Compressed:**

> Escolhemos a biblioteca X por...

### 3. Kill filler words

| Bloated                              | Compressed                |
| ------------------------------------ | ------------------------- |
| "faz-se necessário realizar"         | "precisa"                 |
| "com o objetivo de"                  | "para"                    |
| "no sentido de"                      | "para"                    |
| "em relação a"                       | "sobre"                   |
| "ao longo do processo de"            | "durante"                 |
| "de maneira a garantir que"          | "para garantir que"       |
| "é importante mencionar que"         | (apenas mencione)         |

### 4. Replace prose with structure

If you're writing "primeiro X, em seguida Y, depois Z, por fim W" — use a numbered list.

If you're writing "X mudou para A, Y mudou para B, Z mudou para C" — use a table.

If you're writing "X é sim, Y é sim, Z é não, W é sim" — use a checklist or a table.

### 5. One word > several

| Verbose                          | Concise          |
| -------------------------------- | ---------------- |
| "fazer a chamada ao"             | "chamar"         |
| "realizar a validação"           | "validar"        |
| "efetuar o cálculo"              | "calcular"       |
| "proceder com a execução"        | "executar"       |
| "fazer a persistência"           | "persistir" / "salvar" |

### 6. Cut self-reference

**Bloated:**

> Como será visto adiante na seção "Decisões técnicas", a escolha...

**Compressed:**

> A escolha...

If the next section explains it, the reader gets there naturally. The meta-preview adds nothing.

### 7. Tables for lists of attributes

If each bullet has the same structure (`item: value`), it's a table waiting to happen.

**Bullets (OK but verbose):**

- `USER_ID`: UUID do usuário. Obrigatório.
- `SESSION_TTL`: Tempo de vida da sessão em segundos. Opcional, default 3600.
- `JWT_SECRET`: Chave para assinar JWT. Obrigatório.

**Table (cleaner):**

| Variável       | Uso                              | Obrigatória | Default |
| -------------- | -------------------------------- | ----------- | ------- |
| `USER_ID`      | UUID do usuário                  | Sim         | —       |
| `SESSION_TTL`  | Tempo de vida da sessão (s)      | Não         | 3600    |
| `JWT_SECRET`   | Chave para assinar JWT           | Sim         | —       |

---

## When to Expand vs. Compress

**Expand when:**

- A decision has a subtle rationale worth preserving
- A flow is genuinely complex and needs step-by-step
- A risk or gotcha would bite future-you if buried

**Compress when:**

- The reader can read the code just as fast
- You're filling a template section out of habit
- You're restating what a link already implies

---

## Size Targets by Scope

| Scope           | Target words (README.md) |
| --------------- | ------------------------ |
| Quick           | ≤300                     |
| Standard        | 400-900                  |
| Multi-phase     | 800-2000                 |

**If you blow past the target, audit every paragraph:**

1. Does it contribute something the reader can't get from the code?
2. Could it be compressed to one sentence?
3. Could it be cut entirely?

If two of those answers are "no, no, yes" — cut it.

---

## Red Flags During Writing

Stop and compress when you see:

- 🚩 Paragraph longer than 5 sentences
- 🚩 Two adjacent paragraphs saying similar things
- 🚩 A list with 10+ items — probably needs grouping or a table
- 🚩 A section that feels "filler" — you couldn't articulate its value in one sentence
- 🚩 A sentence using three of: "simplesmente", "basicamente", "essencialmente", "é importante", "vale ressaltar"
- 🚩 A code block you're unsure whether to include — default to linking

---

## The Compression Test

Before finalizing a section, read it and ask:

> **"If I deleted this section, would a future reader be notably worse off?"**

- Clearly yes → keep
- Not sure → probably cut or compress
- Clearly no → cut

---

## Tips

- **Tables beat prose for parallel data** — every time
- **Links beat code snippets** — almost every time
- **One word beats three** — almost every time
- **Summary beats enumeration** — usually
- **Silence beats filler** — always
- **Aim for the target, not below it** — under-documenting is also a failure
