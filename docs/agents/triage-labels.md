# Triage labels

As skills de engenharia falam em cinco papéis de triagem. Este arquivo mapeia esses papéis para as strings de label efetivamente usadas neste tracker.

Este repo **não** usa o vocabulário canônico verbatim: ele nasceu com um dialeto próprio, e o dialeto fica.

| Papel na skill    | Label neste tracker | Significado                                                     |
| ----------------- | ------------------- | --------------------------------------------------------------- |
| `needs-triage`    | `proposed`          | Proposta por agente; aguarda triagem humana                     |
| `needs-info`      | `needs-info`        | Aguardando informação de quem reportou                          |
| `ready-for-agent` | `agent-ready`       | Slice especificada, pronta para um agente AFK pegar             |
| `ready-for-human` | `hitl`              | Humano nas bordas: segredo, DNS, produção, decisão de julgamento |
| `wontfix`         | `wontfix`           | Não será tratada                                                |

> **O vocabulário é um slot, não um invariante.** A anatomia panlabs obriga que todo repo **declare** seu vocabulário neste arquivo; não obriga que declare *este* vocabulário. O repo meta da org usa o canônico verbatim e é igualmente conforme.
>
> Consequência para qualquer script de frota, incluindo o checker de conformidade: **leia o valor declarado, nunca crave o label no código.**

## As famílias ortogonais

Além dos cinco papéis, este tracker carrega três famílias que **não** são triagem e não competem com ela. Uma issue costuma ter uma de cada:

- **Bloqueio:** `blocked` (tem bloqueio aberto; ver o `Blocked by` no corpo).
- **Área:** `infra`, `ci`, `web`, `api`, `docs`, `design`, `content`.
- **Boundary:** `catalog`, `identity`, `engagement`, `narration`, `shared`, `platform` ([ADR-0001](../adr/0001-monorepo-and-boundaries.md)).
- **Natureza:** `feature`, `bug`, `chore`.

`blocked` é o único que altera o despacho: uma issue `agent-ready` **e** `blocked` não é pegável enquanto o bloqueio estiver aberto.
