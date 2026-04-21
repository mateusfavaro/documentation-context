# documentation-context

Skill para gerar **documentação didática de tasks já implementadas**, em português, focada em aprendizado e revisão futura.

---

## O que essa skill faz

Você terminou uma feature, um refactor ou corrigiu um bug. A skill lê o código alterado + os arquivos de planejamento que você usou e gera um `README.md` organizado, explicando:

- **O que** foi feito
- **Por que** foi feito (quando o motivo existe no código ou no planning)
- **Como** o código funciona na prática
- **Como as peças se relacionam** entre si

Tudo em **Português (pt-BR)**, com tom didático, pra você (ou outro dev) conseguir reconstruir o contexto mesmo 6 meses depois.

---

## Qual problema ela resolve

Sem documentação, o contexto de uma task se perde rápido:

- O commit sozinho não explica **por que** certas decisões foram tomadas
- O diff não mostra **como** as partes se conectam
- Planning files descrevem o **plano**, não o que realmente foi entregue
- Tasks divididas em fases perdem a visão geral

Essa skill consolida tudo em uma documentação única, fiel ao que existe no código.

---

## Como funciona

A skill segue 4 fases adaptativas — o tamanho da documentação cresce com a complexidade da task:

```
┌──────────┐   ┌──────────┐   ┌───────────┐   ┌─────────┐
│ DISCOVER │ → │ ANALYZE  │ → │ STRUCTURE │ → │  WRITE  │
└──────────┘   └──────────┘   └───────────┘   └─────────┘
```

### 1. Discover — Perguntas iniciais

A skill faz 3 perguntas obrigatórias em pt-BR:

1. Qual task você quer documentar?
2. Há arquivos de planejamento relacionados?
3. Qual o escopo (feature, refactor, bug fix, config)?

Mais uma quarta pergunta: **onde** a documentação deve ser criada.

### 2. Analyze — Análise do que foi feito

A skill:

- Lê git diff / git log para identificar os arquivos alterados
- Lê os arquivos de código alterados e criados
- Lê **todos** os planning files (se a task foi multi-fase, não apenas o último)
- Identifica relações entre módulos, decisões técnicas registradas e o fluxo do runtime

### 3. Structure — Classificação do escopo

A skill classifica a task em uma de 4 categorias:

| Escopo          | Quando                                         | Template aplicado                 |
| --------------- | ---------------------------------------------- | --------------------------------- |
| **Quick**       | 1-3 arquivos, mudança pontual, sem planning    | Documentação enxuta (≤300 palavras) |
| **Standard**    | Feature, refactor ou bug fix de porte médio   | Template específico (feature / refactor / bugfix) |
| **Multi-phase** | Task dividida em 2+ planning files            | Doc consolidada cobrindo todas as fases |
| **Exploratory** | Escopo inicialmente confuso                   | Decide o template após analisar   |

### 4. Write — Geração da documentação

A skill:

1. Cria uma pasta com nome descritivo em pt-BR kebab-case (ex: `implementacao-login-social/`)
2. Gera um `README.md` dentro dela, usando o template da categoria escolhida
3. Aplica três filtros de qualidade em cada parágrafo:
   - **Fidelidade:** está respaldado no código ou no planning?
   - **Eficiência:** poderia ser mais curto?
   - **Estilo:** está claro e didático em pt-BR?
4. Roda um checklist final antes de entregar

---

## Como instalar

Clone o repositório para dentro do seu projeto, na pasta `.claude/skills`:

```bash
git clone https://github.com/mateusfavaro/documentation-context  .claude/skills/documentation-context
```

A estrutura final deve ficar assim:

```
seu-projeto/
└── .claude/
    └── skills/
        └── documentation-context/
            ├── SKILL.md
            ├── README.md
            └── references/
                └── ...
```

Se a pasta `.claude/skills/` ainda não existir no seu projeto, crie antes do clone:

```bash
mkdir -p .claude/skills
git clone https://github.com/mateusfavaro/documentation-context .claude/skills/documentation-context
```

Depois disso, o Claude Code detecta a skill automaticamente pelo nome `documentation-context`.

---

## Como usar

### Pré-requisito

A skill precisa estar instalada no Claude Code e detectável pelo nome `documentation-context`.

### Passo a passo

1. **Termine a task** que quer documentar (implemente, commite, chegue no estado final).

2. **Acione a skill** com um dos triggers:

   - `documentar task`
   - `gerar documentação`
   - `documentar implementação`
   - `documentar commit`
   - `documentar PR`
   - `document the last feature`

   Ou invoque diretamente como `/documentation-context` se estiver como slash command.

3. **Responda as 3 perguntas obrigatórias** em pt-BR. Exemplo:

   ```
   1. Qual task você quer documentar?
      → A implementação de login social que terminei agora.

   2. Há arquivos de planejamento?
      → Sim, em .planning/05-login-social.md.

   3. Qual é o escopo?
      → Nova feature.
   ```

4. **Informe onde** a documentação deve ser criada:

   ```
   4. Em qual diretório a documentação deve ser criada?
      → C:/Users/meu-user/projetos/meu-app/docs/
   ```

5. **Aguarde a skill analisar** o código e o planning. Ela pode fazer perguntas de follow-up se detectar ambiguidade (ex: "não achei referência a X no código — foi implementado?").

6. **Receba a documentação pronta.** A skill responde com uma mensagem curta:

   ```
   Pronto. A documentação foi criada em:
   C:/Users/.../docs/implementacao-login-social/README.md

   Cobri: objetivo, arquivos alterados, fluxo na prática,
   decisões técnicas e como testar.
   ```

---

## Triggers de uso

### Uso típico — task simples

```
> Acabei de terminar o refactor do middleware de auth.
> Documenta pra mim.
```

A skill pergunta as 3 perguntas, analisa, e gera um `refatoracao-auth-middleware/README.md`.

### Uso típico — task multi-fase

```
> Implementei o sistema de moderação de posts em 5 fases,
> tudo em .planning/01-...md até 05-...md.
> Gera a documentação consolidada.
```

A skill lê **todos os 5 planning files em ordem**, mapeia cada fase para os arquivos reais no código, e gera um `implementacao-moderacao-posts-completa/README.md` com timeline, breakdown por fase, e decisões transversais.

### Uso típico — bug fix

```
> Corrigi o bug do carrinho vazio após login. Documenta.
```

A skill identifica o fix, pergunta o que você precisa (sintoma, causa raiz), e gera um `correcao-carrinho-vazio-pos-login/README.md`.

---

## O que a skill entrega

Sempre a mesma estrutura:

```
<diretório-que-você-escolheu>/
└── <slug-descritivo>/
    ├── README.md               ← a documentação (pt-BR)
    └── assets/                 ← opcional, só se houver diagramas ou imagens
```

**Exemplos de slugs gerados:**

- `implementacao-login-social`
- `refatoracao-camada-auth`
- `correcao-bug-sessao-expirada`
- `implementacao-moderacao-posts-completa`
- `config-deploy-staging`

---

## O que cada tipo de documentação contém

### Feature

Objetivo · Arquivos alterados · Como funciona na prática · Componentes principais · Decisões técnicas · Como testar

### Refactor

Motivação · Antes e depois · Mudança de responsabilidades · O que continua igual · Riscos · Como verificar

### Bug Fix

Sintoma · Causa raiz · Correção · Como validar · Prevenção de regressão

### Multi-phase

Visão geral · Linha do tempo · Breakdown por fase · Decisões transversais · Divergências entre plano e código · Arquitetura final

### Quick

Contexto · O que mudou · Por quê · Como verificar (≤300 palavras total)

---

## Princípios da skill

### 1. Fidelidade ao código

A skill **nunca inventa**:

- Não sugere melhorias
- Não descreve comportamento que não foi implementado
- Não cria "próximos passos" sem base em planning files reais
- Se uma decisão não está documentada, ela escreve "motivo não registrado" em vez de chutar

### 2. Eficiência de tokens

A skill **resume, não copia**:

- Linka arquivos em vez de colar trechos longos de código
- Usa tabelas pra listas de arquivos em vez de parágrafos
- Evita linguagem corporativa desnecessária

### 3. Tom didático em pt-BR

A skill escreve como um colega explicando pra outro:

- Segunda pessoa informal (`você`)
- Parágrafos curtos (2-4 frases)
- Zero jargão vazio ("robusto", "elegante", "escalável")
- Títulos e corpo sempre em português

---

## Estrutura interna da skill

Pra quem quer entender como a skill foi construída ou ajustá-la:

```
documentation-context/
├── SKILL.md                          ← ponto de entrada, define as 4 fases
├── .skill-meta.json                  ← metadados
├── README.md                         ← este arquivo
└── references/
    ├── discover.md                   ← fase 1: perguntas obrigatórias
    ├── analyze-task.md               ← fase 2: análise de código
    ├── analyze-phases.md             ← fase 2: tasks multi-fase
    ├── scope-detection.md            ← fase 3: classifica escopo
    ├── write-documentation.md        ← fase 4: processo de escrita
    ├── template-feature.md           ← template nova feature
    ├── template-refactor.md          ← template refatoração
    ├── template-bugfix.md            ← template correção de bug
    ├── template-multi-phase.md       ← template task em fases
    ├── template-quick.md             ← template minimalista
    ├── writing-style.md              ← regras de estilo pt-BR
    ├── code-explanation.md           ← como explicar código didaticamente
    ├── token-efficiency.md           ← compressão sem perder significado
    ├── fidelity-rules.md             ← regras de fidelidade (não inventar)
    ├── file-organization.md          ← convenções de pastas e slugs
    └── quality-checklist.md          ← checklist final antes de entregar
```

O `SKILL.md` é o que o Claude Code carrega primeiro. Os arquivos em `references/` são carregados sob demanda conforme a fase em execução — isso mantém o contexto enxuto.

---

## Exemplo de output

Pra uma task hipotética de adicionar login social via OAuth, a skill produziria:

```
C:/Users/meu-user/projetos/meu-app/docs/
└── implementacao-login-social/
    └── README.md
```

E o `README.md` teria seções assim (resumido):

```markdown
# Implementação: Login Social

> **Resumo:** Adiciona login com Google e GitHub via OAuth.

**Tipo:** Nova feature
**Data:** 2026-04-20
**Planning:** .planning/05-login-social.md

## Objetivo
...

## Arquivos alterados
| Arquivo | Ação | Papel |
|---------|------|-------|
| src/auth/SocialLoginService.ts | Criado | Troca código OAuth por token |
...

## Como funciona na prática
1. Usuário clica em "Entrar com Google"
2. Handler chama SocialLoginService.iniciar('google')
...

## Decisões técnicas
### Escolha do next-auth
**Por quê:** Registrado no planning como alternativa a `passport` pela integração nativa com o App Router.
...

## Como testar
...
```

---

## Quando **não** usar essa skill

Essa skill é pra documentar tasks **já implementadas**. Não use ela pra:

- Gerar comentários inline no código
- Escrever READMEs genéricos de projeto
- Documentação de API (OpenAPI / JSDoc)
- Tutoriais ou guias de onboarding

---

## Autor

- Mateus Favaro
