Descrição do Projeto
O DimDim é uma API desenvolvida em .NET 8 que visa o gerenciamento de lançamentos financeiros organizados por setores corporativos. O projeto foi estruturado para demonstrar o domínio de tecnologias de conteinerização, persistência de dados em volumes e comunicação entre serviços através de redes isoladas no Docker. A arquitetura segue o modelo relacional entre Setores e Transações, permitindo uma visão analítica dos gastos por departamento.

Arquitetura da Infraestrutura
A solução é composta por dois serviços principais que operam em harmonia dentro de um ambiente Docker:

API .NET: Responsável pela lógica de negócio e interface Swagger.

MySQL 8.0: Banco de dados relacional para armazenamento persistente.

Requisitos de Sistema
Docker Desktop instalado e configurado.

SDK do .NET 8.0 (para compilação local, se necessário).

Ferramenta de linha de comando (Prompt de Comando ou PowerShell).

Passo a Passo para Configuração do Ambiente
1. Criação da Rede Docker
Para permitir que a aplicação se comunique com o banco de dados pelo nome do serviço, criamos uma rede do tipo bridge:

DOS
docker network create rede-dimdim-563665
2. Criação do Volume de Dados
Para garantir que as informações não sejam perdidas ao reiniciar os containers, utilizamos um volume persistente para o MySQL:

DOS
docker volume create vol-mysql-563665
3. Configuração do Container de Banco de Dados
O container do MySQL deve ser iniciado com as variáveis de ambiente corretas e vinculado à rede e ao volume criados:

DOS
docker run -d --name db-mysql-563665 --network rede-dimdim-563665 -v vol-mysql-563665:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=sua_senha -e MYSQL_DATABASE=dimdim_db mysql:8.0
4. Construção da Imagem da API (Build)
Na pasta raiz do projeto (onde se encontra o arquivo Dockerfile), execute o comando para gerar a imagem da aplicação:

DOS
docker build -t dimdim-app-img .
5. Execução do Container da Aplicação
Inicie o container da API vinculando-o à mesma rede do banco de dados:

DOS
docker run -d --name app-dimdim-563665 --network rede-dimdim-563665 -e ASPNETCORE_ENVIRONMENT=Development -p 8080:8080 dimdim-app-img
Estrutura do Banco de Dados Relacional
O sistema utiliza duas tabelas principais com relacionamento de chave estrangeira:

Tabela: Setores
Armazena os departamentos da empresa, quantidade de colaboradores e orçamento anual.

Campos: Id, Nome, QtdColaboradores, OrcamentoAnual, GestorResponsavel.

Tabela: Transacoes
Armazena os lançamentos financeiros vinculados a um setor específico.

Campos: Id, Descricao, Valor, Data, SetorId (FK).

Utilização da API (Swagger)
Após a inicialização, a documentação interativa da API estará disponível no endereço:
http://localhost:8080/swagger/index.html

Fluxo de Teste Sugerido:
Acesse o endpoint GET /api/Setores para visualizar os setores cadastrados.

Utilize o endpoint POST /api/Transacoes para inserir um novo gasto, informando o ID do setor correspondente no campo setorId.

Validação dos Dados no MySQL
Para verificar se os dados foram persistidos corretamente, acesse o terminal do container do banco:

DOS
docker exec -it db-mysql-563665 mysql -u root -p
Após realizar o login, utilize a seguinte consulta SQL para visualizar os gastos por setor:

SQL
USE dimdim_db;
SELECT s.Nome as Setor, s.GestorResponsavel, t.Descricao, t.Valor 
FROM Setores s 
JOIN Transacoes t ON s.Id = t.SetorId;
Equipe Responsável
João Victor Vendrameto - RM 563665

Gabriel Ambrósio Saraiva - RM 566552
