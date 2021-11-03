# MongoDB - sharded cluster

### O que é MongoDB?
MongoDB é um banco de dados de documentos com caracterísitcas de escalabilidade e flexibilidade, classificado como programa de banco de dados NoSQL.

### Objetivo
Implementar uma arquitetura de sharded cluster que atende a carateristicas do teorema CAP como consitência e tolerância à falha através do particionamento de dados e escalabilidade horizontal utilizando containers orquestrados pelo Docker.

### Docker
Para criação deste laboratório é necessário utilizar o Docker que pode ser encontrado neste [link](https://www.docker.com/)

### Arquitetura
Imagem

Com base na imagem iremos utilizar na prática deste post 01 servidor de mongos, 03 servidores de Config server em modo replicatSet, 01 shard com dois servidores em replicatSet, 01 shard com dois servidores em replicatSet.

### Componentes da arquitetura
- Mongos: Funciona como um roteador de consultas, uma interface entre a aplicação e o sharded cluster;
- Config server: Armazena parâmetros de configuração do cluster e metadados
- Shard: Responsável por armazenar um subconjunto de dados


### Etapas
- Criar e configurar os servidores de Config server
- Criar e configurar os servidores de shard
- Criar e configurar o servidor de mongos
- Escalar o cluster adicionando um novo shard


## Inicio

### Docker 
Para realizar a criação da estrutura do cluster é necessário criar uma nova rede dentro do Docker utilizando o comando abaixo

`docker network create mongo-cluster`

Validar criação da rede
`docker network ls`

### Config server
Criando e iniciando os containers do componente config server

`docker run --name mongo-config01 --net mongo-cluster -d mongo mongod --configsvr --replSet configserver -port 27017`
`docker run --name mongo-config02 --net mongo-cluster -d mongo mongod --configsvr --replSet configserver -port 27017`
`docker run --name mongo-config03 --net mongo-cluster -d mongo mongod --configsvr --replSet configserver -port 27017`

Conectando no servidor mongo-config01 para criar a configuração de replicatSet

`docker exec -it mongo-config01 mongo`

Configurando os servidores para trabalhar em modo replicatSet do MongoDB

`rs.initiate({ _id: "configserver", configsvr: true, version: 1, members: [ { _id: 0, host : "mongo-config01:27017" }, { _id: 1, host : "mongo-config02:27017" }, { _id: 2, host : "mongo-config03:27017" } ] } )
`


