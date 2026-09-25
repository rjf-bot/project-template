# Domain Context

Este arquivo é o glossário e contexto canônico do projeto.
Todos os agentes e contribuidores devem usar a terminologia definida aqui.

## Quando usar este arquivo

Use `CONTEXT.md` para registrar conhecimento canônico, curto e estável do domínio.

Este arquivo deve conter:

- termos oficiais e suas definições;
- distinções conceituais importantes;
- entradas, saídas e restrições de alto nível;
- ambiguidades que precisam de uma resposta única para o projeto.

Este arquivo não deve conter:

- tutoriais longos;
- walkthroughs passo a passo;
- descrições detalhadas de fluxos operacionais;
- decisões arquiteturais extensas;
- conteúdo que muda com frequência operacional.

Regra prática: se a pergunta for "qual é o significado oficial deste termo ou limite de domínio?", a resposta deve estar aqui. Se a pergunta for "como isso funciona em detalhe, com exemplos, fluxos ou guias?", a resposta deve ir para `docs/`.

## Project Overview

Descreva em 3 a 5 linhas:

- o problema que o sistema resolve;
- quem são os consumidores principais;
- qual é a saída principal do sistema.

## Bounded Context

Defina o recorte do domínio coberto por este repositório.
Explique o que está dentro e fora do escopo.

## Glossary

Crie uma seção para cada termo crítico do domínio.
Para cada termo, registre:

- definição;
- formato esperado, quando existir;
- ambiguidades comuns;
- termos a evitar.

## Inputs and Outputs

Documente os artefatos de entrada e saída mais importantes do sistema.

## Integrations

Liste integrações externas, contratos e dependências relevantes.

## Constraints

Registre restrições de negócio, técnicas ou regulatórias que não podem ser quebradas.

## Open Questions

Use esta seção para lacunas de entendimento que ainda precisam ser fechadas.
