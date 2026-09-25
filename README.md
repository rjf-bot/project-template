# SAVIA Project Blueprint

Este diretório contém um blueprint opinativo para repositórios dos projetos SAVIA.

## Objetivo

Oferecer um ponto de partida compartilhado para:

- instruções de agentes;
- contexto de domínio;
- decisões arquiteturais;
- documentação operacional para agentes;
- skills reutilizáveis;
- workflow de especificação, fatiamento em tickets e implementação.

## Workflow opinativo

Este blueprint assume um fluxo operacional completo:

1. transformar conversa ou necessidade em spec;
2. quebrar a spec em tickets verticais e rastreáveis;
3. implementar com validação contínua;
4. revisar a aderência ao padrão e ao escopo.

As skills de `to-spec`, `to-tickets`, `implement`, `code-review`, `python-clean-code`, `pytest-guidelines` e `qa-test-engineering` compõem esse fluxo principal.

## Estrutura

```text
savia-blueprint-mandatory/
├── AGENTS.md
├── CONTEXT.md
├── README.md
├── .agents/
│   └── skills/
│       ├── README.md
│       ├── code-review/
│       ├── fastapi-structure/
│       ├── implement/
│       ├── prompt-engineering/
│       ├── pytest-guidelines/
│       ├── python-clean-code/
│       ├── qa-test-engineering/
│       ├── research/
│       ├── tdd/
│       ├── to-spec/
│       └── to-tickets/
└── docs/
    ├── README.md
    ├── adr/
    │   ├── README.md
    │   └── 0001-template.md
    └── agents/
        ├── README.md
        ├── issue-tracker.md
        └── triage-labels.md
```

## Como usar este blueprint

1. Copie esta estrutura para um novo repositório.
2. Reescreva `CONTEXT.md` com o domínio real do projeto.
3. Ajuste `AGENTS.md` com as regras operacionais do time.
4. Preserve `docs/adr/` para decisões arquiteturais duráveis.
5. Ajuste as skills opcionais conforme a stack do projeto, sem quebrar o workflow central.

## Skills do workflow central

- `to-spec`: consolida a necessidade em uma spec acionável.
- `to-tickets`: transforma a spec em tickets verticais com dependências explícitas.
- `implement`: executa a entrega com foco em validação e revisão.
- `code-review`: revisa padrão e aderência ao escopo.

## Skills base por disciplina

- `python-clean-code`: padrão de código Python.
- `pytest-guidelines`: padrão de testes automatizados.
- `qa-test-engineering`: estratégia de qualidade, cenários e riscos.
- `prompt-engineering`: padrão para prompts e builders de prompt.

## Skills opcionais por stack ou contexto

- `fastapi-structure`: usar quando o projeto expõe API FastAPI.
- `tdd`: usar quando o time trabalha deliberadamente em red-green-refactor.
- `research`: usar quando o projeto exige investigação em fontes primárias.

## Estruturas obrigatórias

- `AGENTS.md`: contrato operacional para agentes e contribuidores.
- `CONTEXT.md`: glossário e contexto canônico do domínio.
- `docs/`: wiki do contexto do projeto, com visão funcional, arquitetura, fluxos e guias operacionais.
- `docs/adr/`: histórico de decisões arquiteturais.
- `docs/agents/`: acordos de trabalho, tracker local e convenções para agentes.
- `.agents/skills/`: skills reutilizáveis e documentadas.

## Regra de uso entre `CONTEXT.md` e `docs/`

- Use `CONTEXT.md` para definições canônicas, limites do domínio e restrições estáveis.
- Use `docs/` para explicações expandidas, fluxos, exemplos, diagramas e guias operacionais.

Em resumo: `CONTEXT.md` responde "o que este termo ou regra significa neste projeto?" e `docs/` responde "como isso funciona em detalhe neste projeto?".

## Requisitos para o tracker local

Este blueprint assume issue tracker local em `.scratch/`, com um diretório por iniciativa.

Convenção mínima:

- `.scratch/<feature-slug>/PRD.md`
- `.scratch/<feature-slug>/issues/<NN>-<slug>.md`
- status registrado na linha `Status:` ou no bloco principal do ticket

Veja `docs/agents/issue-tracker.md` para o contrato operacional completo.
