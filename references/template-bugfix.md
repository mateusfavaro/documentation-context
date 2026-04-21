# Template: Bug Fix Documentation

**When to use:** Standard or Quick scope, an existing behavior was incorrect and is now correct.

**Target length:** 300-700 words. Bug fixes are usually tight.

**All content in pt-BR.**

---

## Template — Copy This Structure

````markdown
# Correção: [Descrição curta do bug]

> **Resumo:** [Uma frase: qual era o comportamento errado e qual é o comportamento correto agora.]

**Tipo:** Correção de bug
**Data da implementação:** [YYYY-MM-DD]
**Severidade:** [Bloqueante / Grave / Moderada / Cosmética]
**Arquivos de planejamento:** [se houver; senão "Nenhum"]

---

## Sintoma

[Descrição do que o usuário ou o sistema via quando o bug acontecia. Seja específico:]

- **O que acontecia:** [Ex: "O carrinho exibia total zerado mesmo com itens adicionados."]
- **Quando acontecia:** [Ex: "Após login, ao voltar para a tela de checkout sem recarregar a página."]
- **Onde acontecia:** [Ex: "Componente `CartSummary` em `/checkout`."]

---

## Causa raiz

[Parágrafo explicando a origem real do problema. Não vago, não genérico. Por que o código fazia o que fazia?]

[Exemplo adaptável:]

> O `CartStore` carregava os itens via `useEffect` dependente do `userId`. Após o login, o `userId` ainda era o anterior (guest) por um tick, e o `useEffect` disparava antes do valor novo. O resultado era uma chamada ao backend com o user errado, que retornava carrinho vazio, e a UI renderizava zero.

**Arquivo onde a causa vivia:** `src/stores/CartStore.ts`, especificamente [linha X ou função Y].

---

## Correção

[Explicação do que foi mudado para resolver a causa raiz. Foco no núcleo da correção, não em detalhes de formatação.]

**Arquivos alterados:**

| Arquivo                          | Mudança                                                 |
| -------------------------------- | ------------------------------------------------------- |
| `src/stores/CartStore.ts`        | [Ex: "Adicionado guard para não disparar fetch enquanto `userId` está em transição."] |
| `src/hooks/useAuthTransition.ts` | [Ex: "Novo hook expondo flag `isAuthSettling`."]        |
| `tests/cart/cart-store.test.ts`  | [Novo teste cobrindo o cenário]                         |

**Essência da correção (se útil mostrar ≤5 linhas):**

```ts
if (isAuthSettling) return
const items = await fetchCart(userId)
```

> Evite colar funções inteiras. Foque na linha que realmente resolve o bug.

---

## Como validar

[Passos concretos para reproduzir o cenário original e confirmar que o bug sumiu.]

**Cenário de reprodução original:**

1. Adicione itens ao carrinho como guest
2. Faça login
3. Volte para `/checkout` sem recarregar
4. **Antes:** total = R$ 0,00
5. **Depois:** total correto

**Testes automatizados:**

```bash
npm run test -- cart-store.test.ts
```

- [ ] Teste novo passa
- [ ] Testes que já passavam continuam passando

---

## Prevenção de regressão

[Parágrafo curto explicando o que está no lugar para impedir que o bug volte.]

- **Teste adicionado:** [Nome do teste e o que ele garante]
- **Guard estrutural:** [Se aplicável: a mudança no código impede o cenário por construção]

> Se nenhuma proteção foi adicionada (ex: fix pontual sem teste), diga isso claramente — não invente uma proteção que não existe.

---

## Relacionados

[Opcional — use só se houver ligação real.]

- **Bugs parecidos já corrigidos:** [commit / PR / doc]
- **Áreas vulneráveis a padrão similar:** [Ex: "Outros stores que dependem de `userId` podem ter o mesmo risco. Ver `WishlistStore`, `AddressStore`."]
````

---

## Quick-Mode Variant

Se o bug era muito pequeno (1 arquivo, fix óbvio), use [template-quick.md](template-quick.md) em vez deste.

---

## What to Cut if the Bug is Trivial

- Omit "Relacionados"
- Omit "Prevenção de regressão" se não houver teste novo e for mesmo fix óbvio
- Encurte "Correção" para um parágrafo + tabela

---

## Tips

- **Causa raiz ≠ sintoma** — O sintoma é o que se via. A causa é o motivo técnico. Não confunda.
- **Se a causa não é clara, não invente** — "Causa raiz exata não identificada; fix testado por tentativa e observação" é aceitável quando é verdade
- **Mostre a linha que resolve o bug**, não o arquivo inteiro
- **Reprodução concreta vale ouro** — Se futuro-você encontrar bug parecido, esse passo-a-passo é o maior ganho
- **Se não há teste de regressão, seja honesto** — "Não foi adicionado teste automatizado" é mais útil que fingir que há proteção
- **Severidade é opcional** — Só preencha se fizer sentido no contexto do projeto
