# Issue Tracker: Local Markdown

Este blueprint assume um issue tracker local baseado em arquivos Markdown dentro de `.scratch/`.

## Objetivo

Dar suporte ao workflow opinativo completo:

- converter uma necessidade em spec;
- quebrar a spec em tickets rastreáveis;
- implementar por fatias verticais;
- manter contexto e histórico no próprio repositório.

## Convenções

- Uma iniciativa por diretório: `.scratch/<feature-slug>/`
- A spec principal fica em `.scratch/<feature-slug>/PRD.md`
- Os tickets de implementação ficam em `.scratch/<feature-slug>/issues/<NN>-<slug>.md`
- A numeração dos tickets começa em `01` e segue a ordem de dependência
- O estado do ticket deve aparecer perto do topo do arquivo

## Estrutura mínima

```text
.scratch/
└── <feature-slug>/
    ├── PRD.md
    └── issues/
        ├── 01-<slug>.md
        ├── 02-<slug>.md
        └── 03-<slug>.md
```

## Estados canônicos

Os estados e papéis canônicos devem seguir `docs/agents/triage-labels.md`.

Vocabulário padrão:

- `needs-triage`
- `needs-info`
- `ready-for-agent`
- `ready-for-human`
- `wontfix`

## Publicação de uma spec

Quando uma skill mandar publicar uma spec no issue tracker:

1. criar `.scratch/<feature-slug>/` se ele ainda não existir;
2. gravar o documento principal em `.scratch/<feature-slug>/PRD.md`;
3. registrar no corpo do documento o problema, a solução, o escopo e as decisões principais;
4. marcar o artefato como pronto para execução por agente quando aplicável.

## Publicação de tickets

Quando uma skill mandar publicar tickets:

1. criar `.scratch/<feature-slug>/issues/`;
2. criar um arquivo por ticket;
3. ordenar os tickets por dependência, com bloqueadores primeiro;
4. registrar `Blocked by:` e `Status:` em cada ticket;
5. manter critérios de aceite simples e verificáveis.

## Modelo mínimo de ticket

```markdown
# 01 - Título do ticket

**What to build:** descrição do comportamento fim a fim.

**Blocked by:** None - can start immediately.

**Status:** ready-for-agent

- [ ] Critério de aceite 1
- [ ] Critério de aceite 2
```

## Regras operacionais

- Não misture múltiplas iniciativas no mesmo diretório.
- Não publique um único arquivo contendo todos os tickets.
- Não use caminhos de arquivo como contrato funcional do ticket.
- Prefira linguagem de comportamento observável ao descrever escopo.
