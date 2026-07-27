# Domain docs

Este repo é **single-context**: um produto, um monorepo, um vocabulário. Não há bounded contexts com glossários concorrentes, e por isso não há um documento de domínio por contexto. Os boundaries de `apps/api` e `apps/web` particionam **código**, não linguagem: os seis compartilham os mesmos termos.

## Onde está o quê

| Documento | Papel |
| --- | --- |
| [`docs/CONTEXT.md`](../CONTEXT.md) | O glossário e os invariantes de domínio. É a fonte-da-verdade da linguagem: termo novo ou invariante alterado entra ali, no mesmo PR da mudança. |
| [`docs/ARCHITECTURE.md`](../ARCHITECTURE.md) | Como o domínio está distribuído em código: monorepo, camadas, persistência, deploy. |
| [`docs/adr/`](../adr/README.md) | As decisões e o porquê delas. Quando uma regra do domínio parecer arbitrária, o motivo está num ADR. |
| [`docs/design/`](../design/README.md) | O contrato visual as-built da Direção A "Prensa". |
| `docs/agents/` | Como um agente trabalha **neste** repo. |

## Os boundaries

Seis, com interfaces explícitas e sem imports diretos entre si ([ADR-0001](../adr/0001-monorepo-and-boundaries.md)):

- **`catalog`**: Section, Source e Artifact. MDX-native, servido pelo `apps/web`; não passa pela API.
- **`identity`**: usuário, sessão e papel ([ADR-0007](../adr/0007-publicar-e-papel-de-user.md), [ADR-0011](../adr/0011-auth-better-auth.md)).
- **`engagement`**: view, voto e comentário ([ADR-0008](../adr/0008-view-como-entidade-persistida.md)).
- **`narration`**: voz/TTS e RAG/Q&A. **V2 deferida**: nada aqui entra sem ADR novo.
- **`shared`**: o que os outros compartilham sem pertencer a nenhum.
- **`platform`**: infra, borda e observabilidade.

## Onde a linguagem é fácil de errar

`docs/CONTEXT.md` fecha com uma lista de **termos ambíguos a evitar**. Ela existe porque o dano de um sinônimo não aparece na hora: ele aparece meses depois, num nome de tabela que diz uma coisa e num componente que diz outra. Consulte-a antes de nomear entidade, rota ou arquivo novo.

## Fronteira com o padrão da org

O vocabulário do **padrão panlabs** (frota, superfície, tipo, slot, invariante, conforme, deriva) não é domínio deste produto e não mora aqui: ele vive em [`panlabs-tech/.github`](https://github.com/panlabs-tech/.github). O que este repo carrega desse vocabulário são as consequências, e elas estão em [`.github/workflows/pr-checks.yml`](../../.github/workflows/pr-checks.yml) e nos outros arquivos de `docs/agents/`.
