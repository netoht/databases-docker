# Instância de jenkins

more info: https://registry.hub.docker.com/_/jenkins/

```sh
docker run --name jenkins -p 8080:8080 -v /var/jenkins_home jenkins
```

# Instâncias de RabbitMQ

more info: https://registry.hub.docker.com/u/tutum/rabbitmq/

```sh
docker run --name rabbitmq -d -p 5672:5672 -p 15672:15672 -e RABBITMQ_PASS="pass" tutum/rabbitmq
```

##### Cluster com duas instâncias
```sh
docker run -d --name rabbit1 \
 -p 5672:5672 -p 15672:15672 -p 35197:35197 -p 4369:4369 -p 25672:25672 \
 -e HOSTNAME=node1.host.io \
 -e RABBITMQ_USE_LONGNAME=true \
 -e RABBITMQ_PASS="pass" \
 tutum/rabbitmq

docker run -d --link rabbit1:node1.host.io --name rabbit2 \
 -p 5673:5672 -p 15673:15672 -p 35193:35197 -p 4363:4369 -p 25673:25672 \
 -e HOSTNAME=node2.host.io \
 -e RABBITMQ_USE_LONGNAME=true \
 -e CLUSTER_WITH=node1.host.io \
 -e RABBITMQ_PASS="pass" \
 tutum/rabbitmq

docker run -d --link rabbit1:node1.host.io --link rabbit2:node2.host.io --name rabbit3 \
 -p 5674:5672 -p 15674:15672 -p 35194:35197 -p 4364:4369 -p 25674:25672 \
 -e HOSTNAME=node3.host.io \
 -e RABBITMQ_USE_LONGNAME=true \
 -e CLUSTER_WITH=node1.host.io \
 -e RABBITMQ_PASS="pass" \
 tutum/rabbitmq
```

###### Dados para conexão via api:
```sh
curl --user admin:mypass  http://<host>:<port>/api/vhosts
```


# Instância de Oracle

more info: https://registry.hub.docker.com/u/alexeiled/docker-oracle-xe-11g/

```
docker run --name oracle -d -p 2223:22 -p 1521:1521 -p 8081:8080 alexeiled/docker-oracle-xe-11g
```

###### Dados para conexão com o banco de dados:
```
url:      jdbc:oracle:thin:@192.168.99.100:1521:xe
hostname: 192.168.99.100
port:     1521
sid:      xe
username: system
password: oracle
```

PS: Caso na primeira vez que você se conectar ao banco aparecer uma mensagem dizendo que a senha do usuário irá expirar, execute o comando baixo para alterar a senha para `oracle`

```sql
ALTER USER system IDENTIFIED BY oracle
```

###### Conectando no Oracle Application Express web management console:
```
url:       http://machine_ip_dev:8080/apex
workspace: INTERNAL
user:      ADMIN
password:  oracle
```

###### Login via SSH na máquina
```sh
ssh root@machine_ip_dev -p 2223
password: admin
```

###### Criando novo usuário
```
CREATE USER my_app identified by 123456;
GRANT create session,create table TO my_app;
GRANT UNLIMITED TABLESPACE TO my_app;
SELECT * FROM dba_users WHERE username = 'MY_APP';
```


# Instância de MySQL

more info: https://registry.hub.docker.com/_/mysql/

```
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=mysql -d mysql:latest
```

###### Dados para conexão com o banco de dados:
```
url:      jdbc:mysql://192.168.99.100:3306
hostname: 192.168.99.100
port:     3306
username: root
password: mysql
```


# Instância de PostgreSQL

more info: https://registry.hub.docker.com/_/postgres/

```
docker run --name postgresql -p 5432:5432 -e POSTGRES_PASSWORD=postgresql -d postgres:latest
```

###### Dados para conexão com o banco de dados:
```
url:      jdbc:postgresql://192.168.99.100:5432/postgres
hostname: 192.168.99.100
port:     5432
username: postgres
password: postgresql
```


# Instância de SQL Server (com Vagrant)

more info: https://github.com/fgrehm/vagrant-mssql-express

```
git clone https://github.com/fgrehm/vagrant-mssql-express.git
cd vagrant-mssql-express
# Download SQL Server with Tools installer
curl -O http://download.microsoft.com/download/0/4/B/04BE03CD-EAF3-4797-9D8D-2E08E316C998/SQLEXPRWT_x64_ENU.exe
vagrant up
# Get a coffee as it will take a while for it to finish provisioning
# vagrant ssh
# Enabling feature Telnet client
# c:\> pkgmgr /iu:"TelnetClient"
```

###### Dados para conexão com o banco de dados:
```
url:      jdbc:sqlserver://192.168.50.4:1433;databaseName=sqlserver
hostname: 192.168.50.4
port:     1433
username: sa
password: #SAPassword!
```
