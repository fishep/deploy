# docker deploy neo4j:latest
> 在docker部署neo4j:latest

#### 部署
```shell

docker-compose -p neo4j up -d

docker-compose -p neo4j down -v

docker exec -it neo4j bash

```

#### 访问
```shell

http://localhost:7474/browser/ -- u/p: [ neo4j/your_password ]
http://neo4j.win.local:7474/browser/ -- u/p: [ neo4j/your_password ]

```

#### 常用命令
```shell

cypher-shell -u neo4j -p your_password

CALL dbms.showCurrentUser();

SHOW DATABASES;
SHOW DEFAULT DATABASE;
SHOW HOME DATABASE;

MATCH (n) RETURN n LIMIT 25;
CREATE (n:Person {name: 'Fishep', age: 33}) RETURN n;
CREATE (n:Person {name: 'Alice', age: 30}) RETURN n;
MATCH (n:Person {name: 'Alice'}) RETURN n;
MATCH (n:Person) RETURN n.name, n.age LIMIT 25;
MATCH (a)-[:KNOWS]->(b) RETURN a, b;
MATCH (a:Person)-[r:KNOWS]->(b:Person) RETURN a, r, b LIMIT 25;

CREATE (a:Person {name: 'Alice', age: 30}), (b:Person {name: 'Bob', age: 25})
CREATE (a)-[:KNOWS]->(b) 
RETURN a, b;

```

