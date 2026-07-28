# Desenvolvimento local

Como subir o `ethitorial` na máquina. Carregado sob demanda pelo [AGENTS.md](../../AGENTS.md).

A composição de serviços existe porque este produto tem **dependência local com estado**: um Postgres que carrega schema versionado em migrations Alembic. Sem ele, a API sobe e responde `unconfigured` no `/health`.

## Pré-requisitos

| Ferramenta | Para quê |
| --- | --- |
| `docker` + `docker compose` | a stack completa e o banco local |
| `uv` | a superfície Python de `apps/api` (ele mesmo gerencia o Python 3.13 declarado em `.python-version`) |
| `pnpm` | a superfície Node do workspace |

Antes de qualquer coisa: `cp .env.example .env` e preencha o que for usar. Nada em `.env` é necessário para a stack subir; ele carrega token de MCP e credencial de OAuth social.

## Os três modos

### Stack completa

```bash
docker compose up --build          # web :3000, api :8000, postgres :5432
```

O serviço `api` roda `alembic upgrade head` antes de servir e **falha rápido** se o banco não estiver alcançável, de propósito: servir com schema velho é pior que não servir.

### Só o banco, resto local

É o modo do dia a dia, porque o ciclo de feedback é o do processo local e não o do rebuild de imagem.

```bash
docker compose up -d postgres

# API, em um terminal
cd apps/api
uv sync
export DATABASE_URL=postgresql+asyncpg://ethitorial:ethitorial@localhost:5432/ethitorial
uv run alembic upgrade head
uv run uvicorn ethitorial.main:app --reload      # :8000, GET /health

# web, em outro
cd apps/web
pnpm install                                     # na raiz também serve: é um workspace
pnpm dev                                         # :3000, lê ETHITORIAL_API_URL
```

### Sem banco nenhum

Vale para trabalho de catálogo e de UI. O catálogo é MDX-native e não passa pela API ([ADR-0001](../adr/0001-monorepo-and-boundaries.md)), então `pnpm dev` sozinho já serve o conteúdo de `content/`. Sem `DATABASE_URL`, o `/health` da API responde `unconfigured` em vez de estourar, e voto, view e comentário ficam inertes.

## Migrations

```bash
cd apps/api
uv run alembic upgrade head                      # aplicar
uv run alembic revision --autogenerate -m "..."  # criar, com o banco no ar
uv run alembic downgrade -1                      # voltar uma
```

Migration é escopo de autonomia do agente ([ADR-0010](../adr/0010-desenvolvimento-autonomo-afk.md)): não pare para pedir.

## O portão local

`pnpm install` na raiz roda `lefthook install` pelo script `prepare`. A partir daí, [`lefthook.yml`](../../lefthook.yml) roda `gitleaks` no que está staged e `commitlint` na mensagem, e no `pre-push` roda a mesma sequência que a CI vai rodar.

Os argumentos são deliberadamente os mesmos dos dois lados (`pytest -m "not integration" -q`, `pyright --warnings`), para que "passou aqui" e "passou na CI" signifiquem a mesma coisa. `--no-verify` é proibido sem justificativa: falha de hook se conserta na causa.

O gate real continua sendo o Portão 2, em [`.github/workflows/pr-checks.yml`](../../.github/workflows/pr-checks.yml) ([ADR-0005](../adr/0005-deploy-checks-em-tres-portoes.md)).

## Rodar os testes como a CI roda

```bash
cd apps/api && uv run pytest -m "not integration" -q   # perna checks-python
pnpm run lint && pnpm run typecheck && pnpm test       # perna checks-node, da raiz
```

Os testes marcados `integration` exigem Postgres no ar e ficam fora do gate; rode-os com o banco subido e sem o `-m`.
