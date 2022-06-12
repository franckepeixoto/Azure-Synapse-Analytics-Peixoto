## Azure Synapse Analytics
https://play.google.com/books/reader?id=yfQ8LgAAAEAJ&pg=GBS.PA98.w.0.0.0.2_89

O Azure Synapse Analytics é um serviço de análise ilimitado que reúne integração de dados, armazenamento de dados corporativos e análise de big data.
Ele oferece a liberdade de consultgar dados em seus termoms, usando recursos dedicados ou sem sevidor de modo escalar. 

O Azure Synaspe réune esses mundos como uma experiência unificada para ingerir, explorar, preparar, gerenciar e fornecer dados para necessidades imediatas de BI e aprendizado de máquina.

> Em poucas palavras, o AZure Synapse Analytics é i, serviço de anãlise oferecido pela Microsoft no Azure, Ela reúne três componentes:
> * Data integration
> * Enterprise data warehouse (EDW)
> * Big data analytics  

#### A Microsoft renomeou o Azure SQL Data Warehouse (ASDW) como Azure Synapse Analytics (ASA).
Isso singifica que quaisquer resources do data wharehouse corporativo que estavam disponíveis no  ASDW foram transferidos para o ASA. 
No entanto, deve-se notar que o ASA não é apenas um ASDW renomeado - é muito mais do que isso.
Além de trazer a base comprovada do mecanismo SQL do azure, há muitos aprimoramentos que foram adicionados aos recursos EDW no Azure Synapse Analytics.

####  Segurança, privacidade e conformidade.
A segurança e a privacidade de dados da nuvem são sempre um desafio para gerenciar. As violações de dados estão se tornando comuns.
É necessário manter seus dados seguros com recursos avançados de segurança e privacidade de dados para evitar violações de dados, bem como para atender a vários requisitos regulatórios e legais.

O **ASA** fornece alguns recursos avançados de segurança, que incluem detecção automatizada de ameaças e criptografia sempre ativa.

Também é necessário ter controle de acesso refinado, e isso é disponibilizado no **ASA** por meio de resources de segurança em nível de coluna e segurança em nível de linha.

Além disso, é oferecido suporte a nível de coluna e ao mascaramento de dados dinâmicos. Ele ajuda na proteção de informações confidenciais em tempo real.

#### Gerenciamento de Workload.
É fundamental para qualquer sistema de análise de dados em que sempre há demandas concorrentes por recursos disponíveis. 
> Essas demandas concorrentes incluem as seguinte s cargas:
> * Processo de Data ingestion.
> * Processamento/transformação de dados. 
> * Consulta, análise e relatórios.
> * Gerenciamento de dados.
> * Exportação de dados.

#### [Synapse SQL -  Componentes Arquiteturais](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql/overview-architecture)
![](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql/media/overview-architecture/sql-architecture.png)

#### Dedicated Synapse SQL Pool: 
Como sugere o nome, é um recurso de computação dedicada. Ele consiste em um mecanismo de processamento paralelo massivo (MPP).
Para os Pools dedicados, a escala Data Wharehouse Unit (DWU), é uma unidade de abstração para medir o poder de computação para as Pools dedicatas.

Existem dois tipos diferentes de nós em sua arquitetura.
> O primeiro tipo de nó é um nó de controle, que é o único  ponto de entrada para qualquer aplicativo ou usuário.
> Uma vez que um aplicativo ou usuário esteja conectao a um nó de controle no Dedicated Pool, eles podem emitir comandnos T-SQL, que serão enviados ao mecanismo **MPP** para fins de disrtibuição.

Os nós de computação são responsáveis por executar suas consultas ou comandos em paralelo para acelerar o processamento.
> Como é um procesamento distribuido, há a necessidade de mover os dados entre os nós de computação e, para isso, existe um serviço interno no nível de sistema.
> Esse serviço é conhecido como DMS (Data Movement Service) e ajuda a exevutar as consultas e comandos em paralelo e fonrnece reusltados precisos.

#### Serverless Synapse SQL Pool: 
Como o próprio nome sugere, não possui poder de computação dedicado. 

Com base em suas consultas ou comandos, ele fornecerá os recursos de computação para você nos bastidores.~

Portando, você não precisa se preocupar em provisionar seus recursos de computação antecipadamente, o que é necessário para os Pools dedicados.
> Os Serverless Pools usam um mecanismo de processamento de consulta distribuido **(DQP)**, que é um pouco direrente do **MPP** usados por Dedicated Pools.

> Ele  também não usa conceito de **DWU**, mas depende do tamanho dos dados que estão sendo processados por sua consulta de estorno.

> Os pools sem servidor (serverless) também usam arquitetura baseada em nó. 

> O primeiro tipo de nó, é um nó de controle para qualquer aplicativo ou usuário. Depois que um aplicativo ou usuário estiver conectado ao nó de controle, ele poderá emitir comandos T-SQL, que serão enviados ao mecanismo DQP para fins de distribuição.

#### DQP - Distributed Query Processing Engine
O Mécanismo é responsável por otimizar e orquestras a execução distribuida dos comandos ou consultas enviadas pelo usuário/aplicativo.

O DQP divide as consultas em consultas menores para vários nós de computação, (outro tipo de nõ).
> Cada pequena consulta enviada a um nó de computação é conhecida como tarefa, que pode representar independentemente uma unidade de execução disrtibuída.


#### Storage Layer
É a segunda camada após a camada de computação na arquitetura do Synapse SQL.
>Você possui duas opção principais para a camda de armazenamento. Com base em seus requisitos, você pode decidir usar o **Azure Blob Storage** ou  o **Azure Data Lake Storage GEn2**;
ambas as opções estão disponíveis para pools dedicados ou não.


#### Synapse Pipelines
O Synapse Pipelines é uma integ  de daods baseada em nuvem e um serviço ETL;ELT que é uma parte central da arquitetura do ASA.
> Ele devira quase tudo do Azure Data Factory. [Integrate Data with Azure Data Factory or Azure Synapse Pipelines.](https://docs.microsoft.com/en-us/learn/modules/data-integration-azure-data-factory/5-understand-components)

![](https://media.springernature.com/lw685/springer-static/image/chp%3A10.1007%2F978-1-4842-7061-5_4/MediaObjects/506586_1_En_4_Fig6_HTML.jpg)

#### Distribution
A distribuição  é um importante componente de arquitetura que nos ajuda a entender como o mecanismo de MPP do Dedicated Pool distribui internamente os dados para procesamento nos nós de coputa~ção disponíveis.

Uma vez que a consulta é enviada para o nó de controle, que é o ponto de entrada para qulaquer consulta enviada por qualquer aplicativo/usuario  ou conexão, o nó de controle
**divide em 60 consultas menores** para que possam ser executadas em paralelos.
> Quando você ingere dados no Azure storage usando o Dedicated PooL,ele fragmenta os dados em distribuições para obter o melhor desempenho do sistema. 
**Você tem três padrões de framentação disponíveis**

* [**HASH Distribution**](https://docs.microsoft.com/pt-br/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-distribute): 
> Quando dedicated Pool usa uma função de HAS para atribuir deterministicamente cada linha a uma distribuição, ela é conhecidacomo  distribuição hash.
> Nesse tipo de tablea, uma coluna será designada como coluna de distribuição e a distribuição de gash aplicará a função de hash aos valores na coluna de distribuição designada para garantir que uam linha seja atribuida apenas a uma distribuição.
> ![](https://docs.microsoft.com/pt-br/azure/synapse-analytics/sql-data-warehouse/media/sql-data-warehouse-tables-distribute/hash-distributed-table.png)

**!** As tabelas disitribuidas por hash fazem mais sentido quando você tem um grande volume de **fatos** que deseja distribuir adequadamente para obter um desempenho ideal.
> Esse tipo de distribuição, é indicado para tabelas com um volumen acima de 2GB.

---

* [**Round-Robin Distribution:**](https://docs.microsoft.com/pt-br/azure/synapse-analytics/sql-data-warehouse/design-guidance-for-replicated-tables)
> Essa é uma das distribuições mais fáceis de aplicar, pois não requer nenhum conhecimento prévio de valor nas colunas para fins de distribuição. 

> No entanto, não é possível obter nenhuma otimização adicional ao consultar os dados.

> Você deve evitar esse tipo de distribuição se sua tabela for usada em JOINs, pois não dará um bom desempenho.

A distribuição round-robin pe geralmente usada quando você não consegue encontrar uma boa coluna candidata para **aplicar funções de hash**.

Da mesma forma, quando você sabe que suas consultas nas tabelas vão varia a cada vez, é uma boa ideia usar essa distribuição.
> Como regra geral, você pode lembrar que a distribuição round-robin é boa para carregamento de dados, mas não é a solução ideal ao consultar os dados.
>![](https://csharpcorner-mindcrackerinc.netdna-ssl.com/article/distributions-in-azure-synapse-analytics/Images/image-20211221102614-2.png)

---

* [**Replication-based Distribution:**](https://docs.microsoft.com/pt-br/azure/synapse-analytics/sql-data-warehouse/design-guidance-for-replicated-tables)
> Esse tipo de distribuição é realmente útil quando o tamanho da sua tabela é menor. A regra geral é replicar tabelmas com menos de **2GB**.

> No entanto, em determinadas situações, você pode decidir replicar tabelas  ainda maiores que  2GB em cada um dos nós computaveis.

Geralmente, as tabelas de dimensão são menbores em tamanho e são boas candidatas para esse tipo de distribuição.
> Se em um cenário você replicou suas tabelas de dimensões menores em cada um de seus nós, ao unir os dados distribuídos da tabela de fatos com as tabelas de dimensões, cada nó terá ps dadps reçacopmadps às tabelas de si.

> Isso rresulta em **melhor desempenho** para seus JOINs. Há uma pequena sobrecarga de copiar todos os dados para todos os nós para oter os benefícios das tabelas replicateds.
![](https://docs.microsoft.com/pt-br/azure/synapse-analytics/sql-data-warehouse/media/design-guidance-for-replicated-tables/replicated-table.png)

---

### Dedicate (Provisioned) Synapse SQL Pool
Para reunir dois mundos diferentes - ou  seja, Big Data Analytics (BDA) e Data Wherehouse Enterprise (EDW), a Microsoft introduziu o Azure Synapse Analytics.
O [**Dedicated Synapse Pool**](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) é uma representação de uma coleção de recursos específicos de análise sendo usados pela arquitetura Dedicate Synapse SQL.
> Depois que os dados forem carregados, você poderá executar uma carga de trablaho de análise de dados  usando o mecanismo de procesamento paralelo em massa (MPP).

Como resultado do mecanismo MPP que o dedicated pool usa internamente, há uma melhoria de desempenho significativa e notável em relação a qualquer sistema de gerenciamento de banco de dados tradicional.

![](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/media/sql-data-warehouse-overview-what-is/dedicated-sql-pool.png)

---

### Serverless (On-Demand) Synapse SQL Pool
Como você não precisa provisionar ou dedicar nenhuma infraestrutura especifica antecipadamente para começar a usar esa opção, ela é conhecida como pool SQL sob demanda.

Como os  nós de computação são provisionados para você pelo Azure de forma automatica com base em seu workload, as cobrança não se baseiam no número de nós de computação usados.
Portanto, haverá combrança por terabyte de dados procesasdos para a execução de sua consulta. Isso torna o pagamento conforme o uso.

---
#### Segurança
Dedicated Pool possui segurança em nível de linha (RLS), criptografia de dados transparente (TDE), descoberta e classificação de dados,
avaliação  de vunerabilidade, proteção avançada contra ameaças (ATP) e auditoria são alguns dos recursos importantes que só estão disponiveis no Dedicated Pool.


