> **Portfólio:** [Índice geral dos projetos](https://github.com/PauloSilvaSH/Project_finance_pucrs/blob/main/PORTFOLIO.md) · [README de perfil pronto para publicação](https://github.com/PauloSilvaSH/Project_finance_pucrs/blob/main/README_PROFILE.md)

# Análise de Ações da B3 — Pipeline de Dados e Machine Learning

Projeto acadêmico de engenharia e análise de dados que organiza cotações históricas de ações brasileiras, prepara atributos analíticos, explora o comportamento dos ativos e compara modelos de regressão para estimar o fechamento do pregão seguinte.

> **Escopo verificado nos arquivos:** notebooks `Finance_project_Bronze.ipynb`, `Finance_project_Prata.ipynb`, `Finance_project_ML.ipynb` e arquivo `Project_BI.pbix`. O dashboard existe no repositório, mas seu conteúdo visual não foi inspecionado nesta documentação.

## Arquitetura e componentes

```text
Yahoo Finance (yfinance)
        |
        v
Bronze — dados brutos em tabelas Delta
        |
        v
Prata — limpeza, padronização e atributos derivados
        |
        +--> datasets/consultas para análise
        |
        +--> Machine Learning: Linear Regression, Random Forest, XGBoost
        |
        +--> Power BI (.pbix)
```

## Atividades implementadas nos notebooks

### 1. Bronze — ingestão
- Usa Python, `yfinance`, Pandas e Spark.
- Define listas de tickers da B3 organizadas em grupos setoriais.
- Configura uma sessão Spark e cria o schema/database `bronze`.
- Busca histórico de cotações e informações cadastrais e persiste dados brutos em Delta.
- A configuração do notebook prevê dez anos de histórico; a disponibilidade real depende da fonte e do ticker.

### 2. Prata — transformação e exploração
- Lê tabelas brutas e prepara uma tabela curada.
- Padroniza nomes e tipos de campos de cotação e metadados setoriais.
- Cria atributos como variação percentual do dia, amplitude, retorno diário percentual e média móvel de 20 dias.
- Persiste os dados tratados em tabela Delta; o notebook referencia `workspace.prata.table_acoes_b3`.
- Executa análises exploratórias de estatísticas descritivas, volume por empresa/setor, valorização/desvalorização no período e volatilidade.
- Prepara consultas/datasets de análise e exportação para consumo posterior.

### 3. Analytics — Machine Learning
- Carrega os dados tratados e ordena os registros por ticker e data.
- Define como alvo o fechamento do próximo pregão usando deslocamento temporal por empresa (`shift(-1)`).
- Usa atributos numéricos de cotação/tempo e variáveis categóricas de setor e ticker, codificadas com one-hot encoding.
- Compara Regressão Linear, Random Forest e XGBoost.
- Calcula MAE, RMSE e R², apresenta gráficos comparativos e inspeciona importância de atributos nos modelos de árvore.
- O notebook documenta divisão temporal de treino/teste para reduzir vazamento de informação temporal.

### 4. Visualização
- O repositório contém `Project_BI.pbix`, arquivo de Power BI associado ao projeto.
- As páginas, medidas, filtros e origem exata de cada visual precisam ser confirmados abrindo o arquivo no Power BI Desktop.

## Tecnologias identificadas
Python, Pandas, yfinance, PySpark, Spark SQL, Delta Lake, scikit-learn, XGBoost, Matplotlib e Power BI.

## Como reproduzir
1. Configure Python e as bibliotecas utilizadas nos notebooks.
2. Disponibilize um ambiente Spark/Databricks compatível com as referências de catálogo/schema.
3. Execute a ingestão Bronze e confirme a criação das tabelas.
4. Execute a transformação Prata e valide a tabela curada.
5. Execute o notebook de ML após confirmar nomes, tipos e disponibilidade das colunas.
6. Abra `Project_BI.pbix` no Power BI Desktop.

> Os nomes de catálogo/schema são específicos do ambiente. Ajuste-os antes da execução fora do workspace original. A disponibilidade de dados do Yahoo Finance pode variar.

## Pontos de atenção e melhorias
- Centralizar tickers e grupos setoriais em configuração versionada, em vez de listas extensas no notebook.
- Parametrizar período, ambiente e destino das tabelas.
- Criar testes de qualidade para unicidade por ticker/data, valores nulos, preços positivos e continuidade temporal.
- Separar treino e teste por data e avaliar métricas por ticker; revisar cuidadosamente alinhamento de índices após filtros de nulos.
- Comparar com baselines simples (por exemplo, fechamento de hoje como previsão de amanhã). R² alto ou erro baixo não comprova capacidade de superar o mercado.
- Automatizar execução, logs, tratamento de falhas e atualização do dashboard.

## Arquivos principais
- [Bronze](Finance_project_Bronze.ipynb)
- [Prata](Finance_project_Prata.ipynb)
- [Machine Learning](Finance_project_ML.ipynb)
- [Power BI](Project_BI.pbix)

## Limitações
Este é um projeto analítico/educacional, não uma recomendação de investimento. A documentação descreve código e artefatos identificados; não afirma que os notebooks foram executados de ponta a ponta neste ambiente.
