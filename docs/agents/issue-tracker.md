# Issue tracker

As issues deste repo vivem como **issues do GitHub** em `panlabs-tech/ethitorial`. Use a CLI `gh` para todas as operações; não há board, ROADMAP faseado nem planilha a sincronizar.

O estado de execução mora na própria issue: assignee, labels, comentário e o PR que a fecha (`Closes #N`). Ver [workflow.md](workflow.md) para o fluxo que consome esse estado.

## O que entra

Trabalho sobre o **produto** `ethitorial`: catálogo, identidade visual, boundaries de domínio (`catalog`, `identity`, `engagement`, `narration`, `shared`, `platform`), API, web, infra própria e conteúdo de `content/`.

Vertical slices geradas no fatiamento entram aqui uma a uma, cada uma atravessando schema, API, UI e testes.

## O que não entra

**Trabalho sobre o padrão panlabs.** Configuração de org, anatomia de repo, CI compartilhada, ambiente da máquina e conformidade têm tracker próprio, em [`panlabs-tech/.github`](https://github.com/panlabs-tech/.github/issues). O retrofit deste repo contra a anatomia é issue de lá, não daqui: o alvo é do meta por natureza.

**PRs externos.** Esta é uma org de um mantenedor; PR de fora não faz parte do fluxo de triagem.

## Convenção de fechamento

O corpo do PR fecha a issue com **`Closes #N`**, em inglês. O repo escreve em pt-BR, mas a keyword de fechamento é interpretada pelo GitHub e só existe em inglês: `Fecha #N` cria referência e não fecha nada.
