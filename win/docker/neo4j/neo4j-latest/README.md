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


CREATE (n:Person {name: 'Fishep', age: 33}) RETURN n;
CREATE (a:Person {name: 'Alice', age: 30}), (b:Person {name: 'Bob', age: 25});
CREATE (a:Person {name: 'Alice', age: 31}), (b:Person {name: 'Fishep', age: 34}) RETURN a, b;

MATCH (n) RETURN n LIMIT 10;
MATCH (n:Person {name: 'Alice'}) RETURN n;
MATCH (n:Person) WHERE n.name = 'Alice' RETURN n;
MATCH (n:Person) RETURN n.name, n.age LIMIT 25;

MATCH (n:Person {name: 'Alice'}) SET n.age = 18 RETURN n;

MATCH (n:Person {name: 'Alice'}) DELETE n;
MATCH (n:Person {name: 'Fishep'}) DELETE n RETURN n;
MATCH (n) DELETE n;
MATCH (n) DETACH DELETE n;


MATCH (a)-[]->(b) RETURN a, b;
MATCH (a)-[:FRIEND]->(b) RETURN a, b;
MATCH (a:Person)-[r:FRIEND]->(b:Person) RETURN a, r, b LIMIT 25;
MATCH (a)-[r]->(b) RETURN a, r, b LIMIT 10;

MATCH (a:Person {name:'Alice'}), (b:Person {name:'Bob'}) CREATE (a)-[:FRIEND]->(b);

MATCH (a:Person {name:'Alice'})-[r:FRIEND]->(b:Person {name:'Bob'}) SET r.since = 2023 RETURN r;

MATCH (a)-[r:FRIEND]->(b) DELETE r;
MATCH (a)-[r]->(b) DELETE r;


MATCH (p:Person) RETURN COUNT(p), AVG(p.age);
MATCH (p:Person) RETURN p.name, p.age ORDER BY p.age DESC;
MATCH (p:Person) RETURN p.name, p.age ORDER BY p.age DESC SKIP 1 LIMIT 2;

MATCH p=(a:Person)-[:KNOWS*1..3]->(b:Person) RETURN p;

```

