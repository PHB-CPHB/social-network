# Social-network

## Requirements

Maven

Java 8

Docker

Postgres on port 5432 with the role 'appdev'. (Important to import data follow setup)

Neo4j on port 7474. (Important to import data follow setup)

## Setup

### Neo4j
The following Docker commands are used to fill your database with data

First you need to get the files which can be done with following commands

The file can be found [here](https://github.com/datsoftlyngby/soft2018spring-databases-teaching-material/blob/master/data/archive_graph.tar.gz)

After that you need to unpack it and change some headers. That can be done with the follwing commands
```
sed -i -E '1s/.*/:ID,name,job,birthday/' social_network_nodes.csv
sed -i -E '1s/.*/:START_ID,:END_ID/' social_network_edges.csv
```

After that you can create your neo4j container in docker and import the data with the following command
```
docker run \
    -d --name neo4j \
    --rm \
    --publish=7474:7474 \
    --publish=7687:7687 \
    --volume=$(pwd)/import:/import \
    --volume=$(pwd)/plugins:/plugins \
    --env NEO4J_AUTH=neo4j/class \
    --env=NEO4J_dbms_security_procedures_unrestricted=apoc.\\\*,algo.\\\* \
    --env=NEO4J_dbms_memory_pagecache_size=6G \
    --env=NEO4J_dbms_memory_heap_max__size=10G \
    neo4j
```
```
docker exec neo4j sh -c 'neo4j stop'
docker exec neo4j sh -c 'rm -rf data/databases/graph.db'
```
```
docker exec neo4j sh -c 'neo4j-admin import \
    --nodes:Person /import/social_network_nodes.csv \
    --relationships:ENDORSES /import/social_network_edges.csv \
    --ignore-missing-nodes=true \
    --ignore-duplicate-nodes=true \
    --id-type=INTEGER'
```

### Postgres

```
docker run -p 5432:5432 --name data jegp/soft2018-data
```

## Running the Example

To run the code, open the project in an IDEA and navagiate to the Main class and execute the code

## Results
| Depth | Postgres Median | Postgres Average  | Neo4j Median | Neo4j Average |
| ----- |:---------------:| :----------------:|:------------:|:-------------:|
|   1   |                 |                   |              |               |
|   2   |                 |                   |              |               |
|   3   |                 |                   |              |               |
|   4   |                 |                   |              |               |
|   5   |                 |                   |              |               |
