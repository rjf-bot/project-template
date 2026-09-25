# Blueprint Agent Instructions

## Language

Sempre responda ao usuário em português do Brasil (pt-BR), a menos que o projeto defina explicitamente outro idioma padrão.

## Domain Context

Leia o `CONTEXT.md` e os arquivos relevantes em `docs/adr/` antes de alterar a lógica de domínio, prompts ou workflows.
Não invente sinônimos para termos canônicos de domínio que já estejam definidos no `CONTEXT.md`.

## Development Standards

Todo projeto criado a partir deste blueprint deve declarar as skills que são obrigatórias para a sua stack principal.
Exemplos:

* Projetos em Python devem exigir uma skill de Python clean-code.
* Suítes de teste devem exigir uma skill de test-guidelines.
* Projetos de API devem exigir uma skill de framework-structure quando aplicável.

Este blueprint assume um workflow opinativo com spec, tickets, implementação e review.

## Project Structure

| Directory | Purpose |
| --- | --- |
| `docs/` | Wiki de contexto do projeto: contexto funcional, notas de arquitetura, fluxos e guias operacionais |
| `docs/adr/` | Decisões arquiteturais |
| `docs/agents/` | Documentação operacional voltada para agentes |
| `.agents/skills/` | Skills reutilizáveis carregadas sob demanda |
| `app/` ou `src/` | Código-fonte de produção |
| `tests/` | Testes automatizados |
| `.scratch/` | Rastreador de issues local e artefatos de planejamento |

## Workflow

O workflow padrão em repositórios criados a partir deste blueprint é:

1. Converter a conversa atual ou solicitação em uma spec.
2. Quebrar a spec aprovada em tickets com dependências de bloqueio explícitas.
3. Implementar um ticket por vez com validação focada.
4. Fazer o review do resultado para garantir a conformidade com os padrões e o escopo.

Não trate spec, tickets e implementação como caminhos secundários opcionais, a menos que o projeto redefina explicitamente o workflow.

## Running and Testing

Documente os comandos canônicos no `README.md` do projeto e dê preferência a um único ponto de entrada como `make`, `just` ou equivalente.

## Git and PR Rules

* Não faça commit, push, merge ou rebase a menos que o usuário solicite explicitamente.
* Dê preferência a alterações pequenas e verificáveis.
* Atualize a documentação ao alterar regras, arquitetura ou vocabulário do domínio.

## Agent Skills

As seguintes skills fazem parte do workflow padrão do blueprint:

* `to-spec`: converter uma solicitação em uma spec ou PRD.
* `to-tickets`: quebrar um plano ou spec em tickets do tipo tracer-bullet.
* `implement`: entregar um ticket com validação incremental.
* `code-review`: revisar o trabalho em relação aos padrões e à spec.

As seguintes skills constituem a linha de base de qualidade padrão:

* `python-clean-code` para arquivos de código-fonte Python.
* `pytest-guidelines` para arquivos em `tests/`.
* `qa-test-engineering` para estratégia de testes, análise de risco e design de cenários.
* `prompt-engineering` para artefatos de prompt, builders de prompt e alterações de few-shot.

As seguintes skills são opcionais e dependem do formato do projeto:

* `fastapi-structure` para aplicações FastAPI.
* `research` para investigações fundamentadas em fontes primárias.
* `tdd` quando a equipe trabalha explicitamente com foco em testes primeiro (test-first).

Se o projeto alterar essa linha de base, documente a mudança explicitamente neste arquivo.