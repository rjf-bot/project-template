# Checklists — QA Test Engineering

Referência sob demanda para [`SKILL.md`](SKILL.md). Consulte a seção pertinente ao tipo de análise em andamento.

## Cobertura mínima

Avalie, conforme aplicável ao caso:

- Caminho feliz e fluxo principal.
- Campos obrigatórios, opcionais e condicionais.
- Valores válidos, inválidos, mínimos, máximos e de fronteira.
- Partições de equivalência e combinações relevantes de dados.
- Valores nulos, vazios, ausentes, duplicados ou inconsistentes.
- Formatos, tipos e tamanhos incorretos.
- Regras condicionais e dependências entre campos.
- Estados anteriores e posteriores da informação.
- Perfis, permissões e acessos indevidos.
- Paginação, ordenação, filtros, busca e totalização.
- Persistência, integridade e consistência dos dados.
- Idempotência, concorrência e repetição de operações.
- Falhas de integração, indisponibilidade, timeout e respostas inesperadas.
- Regressão e impacto sobre funcionalidades relacionadas.
- Segurança, desempenho, acessibilidade, usabilidade e observabilidade.

## API

Para cada endpoint, analise e valide:

- Método HTTP e rota.
- Parâmetros de caminho, consulta, cabeçalhos e autenticação.
- Corpo da requisição e contrato de dados.
- Campos obrigatórios, opcionais, tipos, formatos e limites.
- Códigos de status e mensagens de erro.
- Estrutura, conteúdo e consistência da resposta.
- Tempo de resposta e comportamento em timeout.
- Efeitos no banco de dados, filas, arquivos ou serviços relacionados.
- Comportamento diante de valores inexistentes, duplicados ou não autorizados.
- Compatibilidade com documentação, interface e integrações consumidoras.

## Interface gráfica

- Estados visuais, habilitação, desabilitação e carregamento de componentes.
- Mensagens de sucesso, alerta, validação e erro.
- Navegação, retorno, cancelamento e preservação de dados.
- Aderência às regras de negócio e consistência com a API.
- Responsividade, acessibilidade, usabilidade e comportamento em diferentes resoluções, quando aplicável.
- Atualização, persistência e apresentação dos dados após as ações do usuário.

## Priorização por risco

Combine estes fatores para decidir profundidade e ordem de execução/automação:

- Impacto para o usuário e para o negócio.
- Probabilidade de falha.
- Complexidade técnica e quantidade de integrações.
- Criticidade e sensibilidade dos dados.
- Frequência de uso da funcionalidade.
- Histórico de defeitos e mudanças recentes.
- Dificuldade de detecção e de recuperação após a falha.
