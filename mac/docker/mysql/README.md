# mysql

### 常用指令
```mysql
CREATE DATABASE IF NOT EXISTS demo;
USE demo;
CREATE USER 'demo' IDENTIFIED BY 'demo';
GRANT ALL ON demo.* TO demo;
GRANT REPLICATION SLAVE ON *.* TO 'demo'@'%';
GRANT REPLICATION CLIENT ON *.* TO 'demo'@'%';
FLUSH PRIVILEGES;

# exit;
# mysql -udemo -pdemo

CREATE TABLE `demo`
(
    `id`           bigint AUTO_INCREMENT     not null comment 'id',
    `comment`     varchar(255)         default null comment '备注',
    primary key (`id`)
);

insert into `demo` (`comment`) values ('this is a test');
select * from demo;
delete from demo;

drop table demo;

drop database demo;

# ALTER USER 'demo'@'%' IDENTIFIED WITH 'mysql_native_password' BY 'demo';

```
