# Template: Refactor Documentation

**When to use:** Standard scope, internal restructuring. Behavior should be equivalent (or nearly so) — the value is in the new structure.

**Target length:** 400-900 words.

**All content in pt-BR.**

---

## Template — Copy This Structure

````markdown
# Refatoração: [Área ou módulo refatorado]

> **Resumo:** [Uma frase explicando o que foi reestruturado e qual o ganho principal.]

**Tipo:** Refatoração
**Data da implementação:** [YYYY-MM-DD]
**Arquivos de planejamento:** [se houver, link; senão "Nenhum"]
**Mudança de comportamento:** [Sim / Não / Mínima — descrever]

---

## Motivação

[Parágrafo curto: por que essa refatoração foi feita? Que problema o código anterior tinha?]

[Possíveis motivações reais (incluir apenas se refletirem a realidade):]

- Duplicação de lógica entre módulos
- Acoplamento que impedia mudanças pontuais
- Nome desatualizado em relação ao domínio
- Estrutura que dificultava testes
- Performance inadequada da organização anterior

> Seja honesto: se a motivação não está documentada e não é óbvia pelo diff, diga "A motivação explícita não está registrada no planejamento" em vez de inventar.

---

## Antes e depois

### Antes

[Breve descrição da estrutura anterior. Onde o código vivia, que padrão seguia, como era consumido.]

```
src/
├── auth/
│   ├── authController.ts      # lógica + validação + persistência misturadas
│   └── authUtils.ts           # funções soltas usadas por várias camadas
```

### Depois

[Descrição da nova estrutura. Onde as responsabilidades foram separadas, que padrão foi adotado.]

```
src/
├── auth/
│   ├── controllers/
│   │   └── AuthController.ts      # só orquestração HTTP
│   ├── services/
│   │   └── AuthService.ts         # regras de negócio
│   └── repositories/
│       └── AuthRepository.ts      # persistência
```

---

## Arquivos envolvidos

| Arquivo                                | Ação              | O que mudou                                          |
| -------------------------------------- | ----------------- | ---------------------------------------------------- |
| `src/auth/authController.ts`           | Removido / Movido | [Onde a lógica foi parar]                            |
| `src/auth/controllers/AuthController.ts` | Criado            | [Responsabilidade exclusiva após a separação]        |
| `src/auth/services/AuthService.ts`     | Criado            | [Responsabilidade]                                   |
| `src/auth/repositories/AuthRepository.ts` | Criado         | [Responsabilidade]                                   |
| `src/routes/auth.ts`                   | Modificado        | Atualiza os imports para o novo controller          |

---

## Mudança de responsabilidades

[Explicação em parágrafo de como as responsabilidades foram redistribuídas. Quem faz o quê agora e por quê essa distribuição é melhor.]

**Exemplo (adaptar ao caso real):**

- **Controller** cuida só de request/response — parsing, status codes, serialização
- **Service** concentra regras de negócio — validações, orquestração, decisões
- **Repository** encapsula persistência — queries, mapeamento entidade↔DB

---

## O que continua igual

[Parágrafo enumerando o que explicitamente não mudou. Isso é tão importante quanto o que mudou — dá segurança para o leitor saber onde ele pode confiar que nada quebrou.]

- Rotas HTTP expostas (endpoints e contratos)
- Tipos exportados ao resto da aplicação
- Comportamento de autenticação do ponto de vista do cliente
- Mensagens de erro e códigos HTTP

---

## Riscos e pontos de atenção

> Liste apenas pontos reais, não suposições genéricas. Se não houver riscos conhecidos, remova esta seção.

- **[Risco 1]:** [Descrição breve]
- **[Risco 2]:** [Descrição breve]

Exemplo:

- **Cache de rotas:** [Framework X cacheia rotas em dev — pode precisar reiniciar o servidor após pull]
- **Imports externos:** [Pacote externo Y ainda importa do caminho antigo; criar alias se necessário]

---

## Como verificar que nada quebrou

[Instruções objetivas — o que rodar para confirmar que o comportamento externo continua igual.]

```bash
# suíte de testes afetada
npm run test -- src/auth

# rota de saúde
curl http://localhost:3000/auth/health
```

- [ ] Todos os testes que passavam antes continuam passando
- [ ] [Fluxo X continua funcionando no ambiente Y]
- [ ] [Rota Z retorna mesmo payload para mesma entrada]

---

## Notas finais

[Opcional: deixar pontas soltas documentadas em uma frase. Ex: "Ficou para um próximo refactor separar também o módulo de sessões" — mas somente se esse comentário estiver em algum planning file ou TODO real. Caso contrário, omita.]
````

---

## What to Cut if the Refactor is Small

For a Quick-border refactor (3-4 files):

- Omit "Mudança de responsabilidades" — dá para juntar com "Antes e depois"
- Omit "Riscos e pontos de atenção" se nada concreto
- Omit "Notas finais"

---

## What to Expand if the Refactor Cuts Deep

For refactors that cross many layers:

- Adicionar seção **"Impacto em outros módulos"** listando quem consome e precisou ajustar
- Adicionar seção **"Compatibilidade"** explicando se há deprecation period, aliases temporários, etc.

Se a refatoração foi feita em fases (ex: 3 PRs diferentes), considere trocar para [template-multi-phase.md](template-multi-phase.md).

---

## Tips

- **"Antes e depois" visual > descritivo** — Mostre a árvore de diretórios, não descreva em prosa
- **"O que continua igual" é ouro** — Futuro-você vai agradecer saber o que estava seguro
- **Não documente melhorias que não foram feitas** — Se você pensou "deveria ter separado X também", isso vai para uma task futura, não pra cá
- **Comportamento = contrato** — Se o contrato mudou, isso é feature disfarçada de refactor; use [template-feature.md](template-feature.md)
- **Motivação real > motivação genérica** — "Para seguir SRP" é fraco. "Porque testes exigiam mockar o DB inteiro" é concreto.
