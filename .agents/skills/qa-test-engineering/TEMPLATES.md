# Templates — QA Test Engineering

Referência sob demanda para [`SKILL.md`](SKILL.md).

## Caso de teste

Cada caso deve ser claro, independente e reproduzível. Use os campos aplicáveis:

- **Identificação/título** — indica ação, condição analisada e resultado esperado (nunca genérico como "validar se funciona").
- **Objetivo**
- **Referência** ao requisito ou regra de negócio.
- **Pré-condições**
- **Massa de dados**
- **Passos de execução**
- **Resultado esperado** por passo ou ao final.
- **Pós-condições** e necessidade de restauração dos dados.
- **Prioridade, tipo de teste e risco relacionado**
- **Evidências esperadas**

## Registro de defeito

Investigue além do sintoma: diferencie comportamento observado, impacto, área possivelmente afetada e evidência disponível. Não afirme causa-raiz sem evidência técnica suficiente — registre suspeita como hipótese de investigação. O registro deve conter, no mínimo:

- **Título objetivo** — ação, condição e comportamento incorreto.
- **Ambiente e versão**, quando disponíveis.
- **Pré-condições e dados utilizados**
- **Passos para reprodução**
- **Resultado obtido**
- **Resultado esperado**
- **Evidências**, logs, payloads ou consultas relevantes.
- **Frequência de ocorrência**
- **Severidade, impacto e possíveis componentes afetados**
