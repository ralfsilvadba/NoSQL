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

