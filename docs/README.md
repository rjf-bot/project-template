# Project Wiki

Esta pasta funciona como a wiki de contexto do projeto.

## Objetivo

Centralizar a documentação viva que ajuda humanos e agentes a entender:

- o domínio do projeto;
- a arquitetura e os fluxos principais;
- os contratos e integrações mais relevantes;
- os guias operacionais necessários para evoluir o sistema.

## Conteúdo esperado

- visão geral do produto ou serviço;
- fluxos de negócio e fluxos técnicos;
- contratos de integração e dependências externas;
- instruções operacionais para agentes e contribuidores;
- ADRs em `docs/adr/`;
- convenções de agentes em `docs/agents/`.

## Relação com `CONTEXT.md`

- `CONTEXT.md` é o glossário canônico e compacto do domínio.
- `docs/` é a wiki expandida do projeto.

## Quando algo deve ir para `CONTEXT.md`

Registre em `CONTEXT.md` quando o conteúdo for:

- definição oficial de um termo do domínio;
- regra de nomenclatura ou distinção conceitual;
- limite do bounded context;
- restrição de alto nível que precisa ser estável e amplamente reutilizada.

Prefira `CONTEXT.md` quando a informação precisar ser curta, inequívoca e fácil de citar por agentes e contribuidores.

## Quando algo deve ir para `docs/`

Registre em `docs/` quando o conteúdo for:

- explicação expandida de um fluxo;
- documentação funcional ou técnica mais detalhada;
- exemplos, diagramas, walkthroughs e guias operacionais;
- material de onboarding ou referência de arquitetura;
- documentação sujeita a evolução mais frequente do que o glossário canônico.

Prefira `docs/` quando a informação exigir contexto, narrativa, passo a passo ou material de apoio para entendimento.

Quando houver conflito entre os dois, o projeto deve definir explicitamente qual documento é a fonte de verdade para cada tipo de informação.

Regra padrão deste blueprint:

- `CONTEXT.md` vence para vocabulário, definições e limites do domínio.
- `docs/` vence para explicações expandidas, fluxos e guias de uso.
