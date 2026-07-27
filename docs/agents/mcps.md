# MCPs locais (Claude Code, Codex, Copilot)

Setup dos MCP servers de produção (Hostinger / Coolify / Cloudflare). Carregado sob demanda pelo [AGENTS.md](../../AGENTS.md). A **fronteira de autonomia** (o que o agente faz sozinho e os 4 casos em que para) está em [workflow.md](workflow.md) e no [ADR-0010](../adr/0010-desenvolvimento-autonomo-afk.md).

- **O `.mcp.json` do Claude Code é versionado**, com placeholder de variável de ambiente no lugar do segredo. Antes ele era gitignored com o token literal dentro, e assim nunca era de fato compartilhado, apesar do nome; a anatomia panlabs cobra a forma versionada (item `app-mcp-config-versioned`).
- O token Hostinger vive no `.env`, lido pelo próprio MCP via dotenv. O token Coolify chega por **variável de ambiente exportada**: o `.mcp.json` traz `${COOLIFY_ACCESS_TOKEN}`, e a expansão lê o ambiente do processo, não o `.env`. Exporte-o no shell (ou no `~/.zshrc`) antes de abrir o agente.
- `COOLIFY_BASE_URL` tem default embutido (`https://vps.panlabs.tech`) e só precisa ser exportado para apontar para outro Coolify.
- Cloudflare é MCP remoto por `http` e autentica por OAuth no primeiro uso, separadamente em cada cliente: nenhum token vai para arquivo.
- Copilot Chat local continua com config próprio e gitignored (`.vscode/mcp.json`), a partir de `.vscode/mcp.json.example`. O Codex não tem mais template versionado aqui: `.codex/` era configuração de ferramenta fora da toolchain decidida, e o conteúdo dele eram exatamente estes mesmos três servidores.
- O Copilot cloud/coding agent **não** recebe estes MCPs de produção. Ele executa ferramentas autonomamente e não compartilha o ambiente local.

Infra registrada em [ADR-0003](../adr/0003-infra-hostinger-vps-coolify.md) (Hostinger + Coolify, incl. substrato compartilhado `panlabs.tech`) e [ADR-0006](../adr/0006-cloudflare-na-frente-da-vps.md) (Cloudflare).
