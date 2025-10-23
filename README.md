# Resumo do Projeto
Desenvolvimento de um MVP de plataforma de Data & Analytics voltada para o setor de comércio de bebidas, com foco em transformar dados em insights acionáveis. O projeto envolveu a implementação de pipelines de engenharia de dados utilizando PySpark, Python e SQL, aplicação de modelagem dimensional (Star Schema) e definição de uma arquitetura em Cloud baseada em Data Lakehouse.
A solução foi projetada para responder aos principais KPIs de negócio, assegurando escalabilidade, desempenho e governança dos dados.

# Arquitetura Medalhão
<img src="https://media.licdn.com/dms/image/v2/D4D12AQHGTqz_KlZAWg/article-inline_image-shrink_1000_1488/B4DZaHqoAXHwAQ-/0/1746032821856?e=1762992000&v=beta&t=6QxtaUUJPY5NbY5UeWM2KsV8PDPAnJFt-HA9pNU2ls8" alt="Preview do projeto" width="900"/>


A arquitetura Medallion estrutura o ciclo de dados em três níveis — Raw/Bronze, Trusted/Silver e Refined/Gold — promovendo uma jornada de evolução que assegura confiabilidade, padronização e valor analítico crescente.

### 🟤 Raw/Bronze — Dados de Origem

- **Propósito:** Preservar as informações exatamente como foram coletadas, sem qualquer tratamento prévio.
- **Fontes/Ações:** Registros de vendas no ponto de venda, logs de transporte e dados de sensores de estoque. Arquivos CSV contendo transações brutas de vendas diárias
- **Usuários:** Engenheiros de dados e responsáveis por ingestão.

### ⚪ Trusted/Silver — Dados Tratados e Harmonizados

- **Propósito:** Padronizar e consolidar os dados da camada Bronze
- **Fontes/Ações:** Limpeza de inconsistências, remoção de duplicidades, enriquecimento com cadastros de produtos e clientes. Dados oriundos de tabelas da camada Raw/Bronze
- **Usuários:** Engenheiros, analistas e cientistas de dados que precisam de informações consistentes.

### 🟡 Refined/Gold — Dados Otimizados para Análise

- **Propósito:** Disponibilizar dados refinados e de alto valor para uso estratégico e operacional.
- **Fontes/Ações:** Criação de métricas de negócio e indicadores como receita, margem, ticket médio e diversos outros KPIs que possam gerar valor ao negócio. Dados oriundos de tabelas da camada Trud
/Silver
- **Usuários:** Analistas de Dados, Stakeholders e Key Users.


### 🔄 Fluxo de Dados

1. Os dados oriundos das origens são ingeridos na camada **Raw**.  
2. Limpeza, Transformações, Deduplicações, transformações e validações na camada **Trusted**.  
3. Dados otimizados para análise, agregados e prontos para utilização de Stakeholders e ferramentas de BI na camada **Refined**.  

### 🛠 Tecnologias Utilizadas

- **Databricks**
- **Unity Catalog**
- **PySpark**
- **Python**
- **SQL**
- **DataLake / DatalakeHouse**


# Pipeline
# Pipeline: job_lakehouse_felipe
O pipeline foi projetado para coletar, processar e organizar dados de forma automatizada em uma arquitetura Lakehouse, evoluindo de dados brutos até informações analíticas nas camadas Raw, Trusted e Refined.
O código Python do pipeline está disponível aqui nesse repositório dentro do diretório 'controller'.

O pipeline está ativo e será executado automaticamente com a chegada de novos arquivos (file_arrival) dentro do Volume `/Volumes/lakehouse/raw/files/`.


## Visão Geral do Projeto
Este projeto apresenta a construção de um Delta Lakehouse com arquitetura dimensional voltado para análises de performance de vendas
A modelagem segue o padrão de tabela fato e dimensões, composta por uma tabela de fatos (fact_order_item) e quatro tabelas dimensionais (dim_product_catalog, dim_region, dim_calendar e dim_trade_channel,).
Essa estrutura possibilita examinar resultados comerciais sob diferentes perspectivas — tempo, canal, localização e produto — permitindo identificar tendências, sazonalidades e padrões de consumo.

## Estrutura do Delta Lakehouse

### Tabelas Dimensão

#### `dim_product_catalog`
- **skproduct_catalog**: Chave primária
- **product_catalog_id**: Identificador do produto
- **ce_brand_flvr, brand_nm**: Marca e sabor
- **pkg_cat, pkg_cat_desc, tsr_pckg_nm**: Categoria e embalagem
- **start_date, end_date**: Validade do registro
- **current**: Indica se é registro atual
- **insertdate**: Timestamp de inserção

####  `dim_region`
- **skregion**: Chave primária
- **place_id**: Identificador do local
- **Btlr_Org_LVL_C_Desc**: Descrição da organização/região
- **start_date, end_date**: Validade do registro
- **current**: Indica se é registro atual
- **insertdate**: Timestamp de inserção

####  `dim_calendar`
- **skcalendar**: Chave primária (BIGINT, auto incremento)
- **date**: Data
- **day, month, year**: Componentes da data
- **insertdate**: Timestamp de inserção

####  `dim_trade_channel`
- **sktrade_channel**: Chave primária
- **trade_channel_id**: Identificador do canal
- **trade_chnl_desc, trade_group_desc, trade_type_desc**: Descrições do canal
- **start_date, end_date**: Validade do registro
- **current**: Indica se é registro atual
- **insertdate**: Timestamp de inserção


---

### Tabela Fato

#### `fact_order_item`
- **skcalendar**: FK para `dim_calendar`
- **order_item_id**: Identificador da venda
- **skproduct_catalog**: FK para `dim_product_catalog`
- **skregion**: FK para `dim_region`
- **sktrade_channel**: FK para `dim_trade_channel`
- **volume**: Quantidade vendida
- **insertdate**: Timestamp de inserção

---


