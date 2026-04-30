Projeto DimDim - Gestão Financeira Corporativa

- *Descrição do Projeto* - 
O DimDim é uma API desenvolvida em .NET 8 que visa o gerenciamento de lançamentos financeiros organizados por setores corporativos. O projeto foi estruturado para demonstrar o domínio de tecnologias de conteinerização, persistência de dados em volumes e comunicação entre serviços através de redes isoladas no Docker. A arquitetura segue o modelo relacional entre Setores e Transações, permitindo uma visão analítica dos gastos por departamento.

- *Arquitetura da Infraestrutura* -
A solução é composta por dois serviços principais que operam em harmonia dentro de um ambiente Docker:

API .NET: Responsável pela lógica de negócio e interface Swagger.

MySQL 8.0: Banco de dados relacional para armazenamento persistente.
