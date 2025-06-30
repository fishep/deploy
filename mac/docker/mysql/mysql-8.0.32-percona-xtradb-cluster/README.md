# 基于docker部署mysql
> 部署mysql8.0.32的集群模式, 多主同步, 所有节点都是主节点, 实时同步, 应用端不用区分读写节点

#### 部署 percona-xtradb-cluster
```shell

# 先执行一遍下面的 为集群创建证书

docker compose -p dbs up -d 

# 打印节点ip
docker exec -it alpine bash
for i in {1..3}; do ping -c 1 mysql-node$i; done | grep PING | sed -r "s/PING mysql-node[0-9] \((.*)\): 56 data bytes/\1/"  
```

#### 为集群创建证书
```shell
docker run --name pxc --rm --user=root -it -v pxc-cert:/cert -v pxc-config:/etc/percona-xtradb-cluster.conf.d percona/percona-xtradb-cluster:8.0.32 bash
mysql_ssl_rsa_setup -d /cert
chown -R mysql:mysql /cert

cat << EOF > /etc/percona-xtradb-cluster.conf.d/cert.cnf
[mysqld]
skip-name-resolve
ssl-ca=/cert/ca.pem
ssl-cert=/cert/server-cert.pem
ssl-key=/cert/server-key.pem

[client]
ssl-ca=/cert/ca.pem
ssl-cert=/cert/client-cert.pem
ssl-key=/cert/client-key.pem

[sst]
encrypt=4
ssl-ca=/cert/ca.pem
ssl-cert=/cert/server-cert.pem
ssl-key=/cert/server-key.pem
EOF

chown -R mysql:mysql /etc/percona-xtradb-cluster.conf.d/cert.cnf

```

#### 集群调试
```shell

docker exec -it mysql-node1 bash -ic "mysql -uroot -proot -hmysql-node1 -e 'SELECT @@server_id,NOW()';"
docker exec -it mysql-node2 bash -ic "mysql -uroot -proot -hmysql-node2 -e 'SELECT @@server_id,NOW()';"
docker exec -it mysql-node3 bash -ic "mysql -uroot -proot -hmysql-node3 -e 'SELECT @@server_id,NOW()';"
docker exec -it mysql-node1 bash -ic "while sleep 1; do mysql -uroot -proot -hmysql-node1 -e 'SELECT @@server_id,NOW()'; done"
docker exec -it mysql-node1 bash -ic "mysql -uroot -proot -hmysql-node1 -e 'SELECT * FROM demo.demo';"
docker exec -it mysql-node2 bash -ic "mysql -uroot -proot -hmysql-node2 -e 'SELECT * FROM demo.demo';"
docker exec -it mysql-node3 bash -ic "mysql -uroot -proot -hmysql-node3 -e 'SELECT * FROM demo.demo';"

show status like 'wsrep%';
show variables like 'pxc_encrypt_cluster_traffic';

select * from performance_schema.pxc_cluster_view;

```