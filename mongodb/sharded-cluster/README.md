# MongoDB - sharded cluster

### O que é MongoDB?
MongoDB é um banco de dados de documentos com caracterísitcas de escalabilidade e flexibilidade, classificado como programa de banco de dados NoSQL.

### Objetivo
Implementar uma arquitetura de sharded cluster que atende a carateristicas do teorema CAP como consitência e tolerância à falha através do particionamento de dados e escalabilidade horizontal utilizando containers orquestrados pelo Docker.

### Docker
Para criação deste laboratório é necessário utilizar o Docker que pode ser encontrado neste [link](https://www.docker.com/)

### Arquitetura
![image](https://github.com/ralfsilvadba/NoSQL/blob/main/img/Arquitetura_ShardedCluster_MongoDB.png)


Com base na imagem iremos utilizar na prática deste passo a passo 01 servidor de mongos, 03 servidores de Config server em modo replicaSet, 01 shard com dois servidores em replicaSet, 01 shard com dois servidores em replicaSet.

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

Configurando os servidores para trabalhar em modo replicaSet do MongoDB

`rs.initiate({ _id: "configserver", configsvr: true, version: 1, members: [ { _id: 0, host : "mongo-config01:27017" }, { _id: 1, host : "mongo-config02:27017" }, { _id: 2, host : "mongo-config03:27017" } ] } )
`
### Shard
Criando e iniciando os containers do componente Shard, neste passo iremos utilizar 02 grupos de shard cada um com dois servidores.

`docker run --name mongo-shard1a --net mongo-cluster -d mongo mongod --port 27018 --shardsvr --replSet shard01`

`docker run --name mongo-shard1b --net mongo-cluster -d mongo mongod --port 27018 --shardsvr --replSet shard01`

Conectando no servidor mongo-shard1a para criar a configuração de replicatSet

`docker exec -it mongo-shard1a mongo --port 27018`

Configurando os servidores de shard para trabalhar em modo replicaSet

`rs.initiate( { _id: "shard01", version: 1, members: [ { _id: 0, host : "mongo-shard1a:27018" }, { _id: 1, host : "mongo-shard1b:27018" }, ] } )`

Primeiro grupo de shard configurado, vamos realizar o mesmo procedimento para o segundo grupo.

`docker run --name mongo-shard2a --net mongo-cluster -d mongo mongod --port 27019 --shardsvr --replSet shard02`

`docker run --name mongo-shard2b --net mongo-cluster -d mongo mongod --port 27019 --shardsvr --replSet shard02`

Conectando no servidor mongo-shard1a para criar a configuração de replicatSet

`docker exec -it mongo-shard2a mongo --port 27019`

Configurando os servidores de shard para trabalhar em modo replicaSet

`rs.initiate( { _id: "shard02", version: 1, members: [ { _id: 0, host : "mongo-shard2a:27019" }, { _id: 1, host : "mongo-shard2b:27019" }, ] } )`

### Mongos
Criando e iniciando o container do componente Mongos

`docker run -p 27017:27017 --name mongo-router --net mongo-cluster -d mongo mongos --port 27017 --configdb configserver/mongo-config01:27017,mongo-config02:27017,mongo-config03:27017 --bind_ip_all`

Conectando no servidor mongo-router para associar a configuração do cluster

`docker exec -it mongo-router mongo`

Adicionando os componentes de shard

`sh.addShard("shard01/mongo-shard1a:27018")`

`sh.addShard("shard01/mongo-shard1b:27018")`

`sh.addShard("shard02/mongo-shard2a:27019")`

`sh.addShard("shard02/mongo-shard2b:27019")`

Seguindo todos os passos anteriores chegamos a criação do Sharded Cluster na prática, com isso vamos aumentar a capacidade do cluster utilizando mais um grupo de shard.

### Escalando o cluster
Criando e iniciando os novos servidores de Shard

`docker run --name mongo-shard3a --net mongo-cluster -d mongo mongod --port 27020 --shardsvr --replSet shard03`

`docker run --name mongo-shard3b --net mongo-cluster -d mongo mongod --port 27020 --shardsvr --replSet shard03`

Conectando no servidor mongo-shard3a para criar a configuração de replicatSet

`docker exec -it mongo-shard3a mongo --port 27020`

Configurando os servidores de shard para trabalhar em modo replicaSet

`rs.initiate( { _id: "shard03", version: 1, members: [ { _id: 0, host : "mongo-shard3a:27020" }, { _id: 1, host : "mongo-shard3b:27020" }, ] } )`

Conectando ao mongo-router para adicionar o novo shard

`docker exec -it mongo-router mongo`

Adicionando o novo shard ao cluster

`sh.addShard("shard03/mongo-shard3a:27020")`

`sh.addShard("shard03/mongo-shard3b:27020")`

## Conclusão

Através deste passo a passo realizamos a criação de um Sharded cluster utilizando MongoDB como banco de dados NoSQL e podemos notar a simplicidade para efetuar a configuração deste modelo de cluster assim como o seu caminho objetivo para escalar conforme sua necessidade.
