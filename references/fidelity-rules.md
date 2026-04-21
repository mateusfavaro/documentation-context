# Fidelity Rules

**Goal:** Ensure the documentation describes what **was actually implemented**, never what **could be** or **should be**. Every sentence must trace back to code or to a planning file. When in doubt, cut.

**This is the most important reference in the skill.** Re-read it before every Write phase.

---

## The Prime Directive

> **Documentar não é reimplementar. Documentar não é sugerir. Documentar é descrever.**
>
> Você descreve o que existe. Se não existe, não documenta.

Every time you're tempted to write "deveria", "poderia", "seria melhor", "uma boa ideia seria" — stop. That sentence does not belong in this documentation.

---

## What is Allowed

- ✅ Stating what was done — "A lógica foi movida para `AuthService`"
- ✅ Stating why, when rationale exists in code comments or planning files — "Escolhemos Zustand porque o planejamento exigia persistência local simples (ver `.planning/02.md`)"
- ✅ Describing flows that actually execute in the code
- ✅ Noting known limitations that are documented in code or planning ("O rate limit não é enforçado — ver TODO em `RateLimit.ts:12`")
- ✅ Referencing real tests, real files, real decisions
- ✅ Saying "A motivação não está documentada" when it's true

## What is NOT Allowed

- ❌ Suggesting improvements ("Um próximo passo seria adicionar cache")
- ❌ Describing behavior that wasn't implemented ("O sistema também valida X")
- ❌ Inventing rationale ("Esta escolha foi feita por performance" — se isso não está em lugar nenhum)
- ❌ Proposing refactors ("Idealmente o service seria dividido em dois")
- ❌ Listing features that "poderiam" existir
- ❌ Adding "considerações futuras" a menos que estejam em planejamento real
- ❌ Exaggerating ("robusto", "escalável") sem evidência concreta
- ❌ Fill-in-the-blank bullshit quando você não sabe ("Melhoria significativa em X" sem dados)

---

## The Trace-Back Test

For every non-trivial claim in the documentation, you must be able to answer:

> **"Where in the code or in the planning files does this come from?"**

If the answer is:

- ✅ "Linha X do arquivo Y" → OK
- ✅ "Trecho Z do planning file W" → OK
- ✅ "Inferido claramente do diff entre A e B" → OK
- ❌ "Eu achei que fazia sentido" → DELETE
- ❌ "É o padrão comum" → DELETE
- ❌ "Geralmente projetos fazem assim" → DELETE
- ❌ "Acho que foi por isso" → DELETE or rewrite as "Motivo não registrado"

---

## Handling Uncertainty Honestly

Sometimes you can't find the rationale. Sometimes the code is silent and planning files are silent. That's fine.

**Good:**

> A escolha pela biblioteca `axios` sobre `fetch` não está documentada. O código simplesmente usa `axios` em todas as chamadas HTTP.

**Bad (invented):**

> Escolhemos `axios` pela sua API mais ergonômica e melhor suporte a interceptors.

The second version **sounds** plausible. That's why it's dangerous. If it's not in the code or planning, don't write it.

---

## Distinguishing Fact from Inference

Some inference is necessary — documentation would be sterile without it. The rule is: **label inferences as inferences**.

**Fact (from planning file):**

> "Conforme registrado em `.planning/02-api.md`, escolhemos REST sobre GraphQL pela simplicidade do client."

**Fair inference (from code structure):**

> "A separação entre `controller` e `service` segue o padrão já usado em outros módulos do projeto (ver `src/users/`)."

**Unfair invention (no basis):**

> "Essa arquitetura foi pensada para suportar escala horizontal futura."

The second is labeled as inference ("segue o padrão já usado") — it's observation-grounded. The third is pure speculation — delete.

---

## The "Could Have Been" Trap

This is the single most common failure mode. Symptoms:

- Sections titled "Próximos passos" without source material
- Sentences starting with "Uma possível melhoria seria..."
- Paragraphs about "considerações para o futuro"
- "Observações" that are actually suggestions

**Every time you write one of these, cut it.** It doesn't belong in the documentation of a completed task.

**Exception:** If the planning file has a `## Próximos passos` section with concrete items, you can cite those:

> Segundo `.planning/05.md`, os próximos passos planejados são: [lista do arquivo].

That's faithful citation. Just don't invent next steps.

---

## Scope Creep Detection

If you're writing the documentation and you find yourself:

- Explaining how a **related** feature works (that wasn't part of this task)
- Describing the **overall architecture** of the module (not the change)
- Giving a **tutorial** on the technology used
- Comparing with **other approaches** not considered

→ Stop. These are signs of scope creep in the documentation itself. Delete them.

Documentation for a task describes **the task**, not the surrounding system or the broader context. If the task integrated with an existing service, name the service and link to its docs — don't re-document it.

---

## Phrases to Audit

If your documentation contains any of these, double-check they're backed by reality:

| Phrase                             | Risk                                                  |
| ---------------------------------- | ----------------------------------------------------- |
| "Para garantir escalabilidade..."  | Often invention; requires concrete basis              |
| "Esta solução é robusta..."        | Hype; rarely backed by anything                       |
| "Seguimos as melhores práticas..." | Vague; delete or specify which practice and where     |
| "Um próximo passo seria..."        | Usually speculative; only valid if planning says so   |
| "Uma alternativa seria..."         | Speculative; if alternative was evaluated, say where  |
| "Poderíamos ter feito..."          | Meta-commentary; delete                               |
| "Este padrão foi escolhido por..." | Only valid if choice is documented                    |
| "Em princípio, isto permite..."    | Red flag; either it permits or it doesn't            |

---

## Self-Audit Checklist

Before delivering, scan the doc and ask for each paragraph:

- [ ] Is every factual claim here traceable to code or planning?
- [ ] Are inferences labeled as inferences (via hedging like "segue o padrão de..." or "como observável em...")?
- [ ] Did I invent a rationale anywhere?
- [ ] Am I suggesting improvements? (if yes → cut)
- [ ] Am I describing behavior that wasn't implemented? (if yes → cut)
- [ ] Am I filling a template section with content just to fill it? (if yes → cut or shrink section)

---

## Honesty Over Completeness

A doc with 5 well-grounded sections beats a doc with 10 sections where 5 are invented. Incompleteness is fixable; invented content is actively harmful — it becomes "truth" when the original author forgets what was real.

**Principles:**

- Empty section > invented section
- "Motivo não documentado" > fabricated motive
- "Não foram adicionados testes" > fake test list
- "Fluxo não totalmente mapeado" > invented flow

---

## When to Escalate to the User

If during Write you realize:

- A key decision's rationale is unclear and matters for the doc
- The planning file contradicts the code
- You can't tell if a file belongs to this task or not

→ Stop writing. Ask the user:

> "Para essa seção, preciso saber: [pergunta específica]. Posso prosseguir com a melhor inferência ou você prefere esclarecer?"

Do not write on assumption and move on.

---

## Tips

- **When in doubt, cut** — less but true beats more but invented
- **"Melhoria futura" is almost never yours to write** — it belongs in backlog, not in the doc
- **Inference is OK if labeled** — "segue o padrão de..." is honest
- **Silence is valid** — "O motivo não está documentado" é melhor que chute
- **Planning files are ground truth for intent; code is ground truth for reality** — quando divergem, priorize código e mencione a divergência
- **Read the doc back and hunt for hype words** — robusto, elegante, escalável, moderno são red flags
