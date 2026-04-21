# Template: Quick Documentation

**When to use:** Quick scope — 1-3 files, change fits in one sentence, no planning file.

**Target length:** ≤300 words. Aim for a single screen.

**All content in pt-BR.**

---

## Template — Copy This Structure

````markdown
# [Tipo]: [Descrição em 3-7 palavras]

> **Resumo:** [Uma frase.]

**Tipo:** [Fix / Config / Ajuste / Atualização]
**Data:** [YYYY-MM-DD]
**Arquivos alterados:** [1 / 2 / 3]

---

## Contexto

[2-4 frases. Qual era o estado antes, por que precisou mudar.]

---

## O que mudou

| Arquivo                    | Mudança                                       |
| -------------------------- | --------------------------------------------- |
| `src/caminho/arquivo.ts`   | [Mudança em 1 linha]                          |
| `.env.example`             | [Variável adicionada]                         |

[Se for só 1 arquivo, pode ser 1 parágrafo ao invés de tabela.]

**Trecho relevante (se ≤5 linhas):**

```ts
// antes
if (x) { ... }

// depois
if (x && y) { ... }
```

---

## Por quê

[1-3 frases. Motivo concreto da mudança. Se não for óbvio pelo diff, explique. Se for óbvio, omita esta seção.]

---

## Como verificar

[1-3 passos para confirmar que funciona. Comando, endpoint, ação no UI.]

```bash
npm run test -- arquivo.test.ts
```

ou

- Reproduza [cenário]
- Observe [resultado esperado]
````

---

## Pruning Guide

Even this template should be trimmed further if the task allows:

- **Só 1 arquivo, mudança óbvia de 1 linha:** remove "Contexto" (junte no Resumo)
- **Config change sem lógica nova:** remove "Como verificar" se reinicialização é óbvia
- **Não há trecho relevante (mudança distribuída):** remove o bloco de código; linka o arquivo

**Minimum viable quick doc:**

```markdown
# Fix: [descrição curta]

> Resumo em uma frase.

**Arquivo:** `src/x.ts`
**Data:** YYYY-MM-DD

## O que mudou
[1 parágrafo]

## Por quê
[1 parágrafo]
```

Esse minimo é aceitável se o escopo genuinamente for pequeno.

---

## When to Escalate to Standard Template

If while writing you realize:

- Há mais de 3 arquivos de fato alterados
- A correção envolve decisão técnica não trivial
- Você se pegou escrevendo mais de 300 palavras

→ Pare. Reavalie o escopo usando [scope-detection.md](scope-detection.md). Provavelmente é Standard, não Quick. Mude para [template-bugfix.md](template-bugfix.md), [template-feature.md](template-feature.md) ou [template-refactor.md](template-refactor.md).

---

## Tips

- **Uma frase no topo já entrega metade do valor** — Esforce-se nela
- **Evite seções vazias** — Se "Por quê" é "porque o bug existia", omita
- **Tabela vs parágrafo** — 1 arquivo = parágrafo; 2-3 arquivos = tabela
- **Trecho de código só se ≤5 linhas** — Caso contrário, linka
- **Quick não é descartável** — É a documentação mínima viável, mas precisa ser fiel e clara
