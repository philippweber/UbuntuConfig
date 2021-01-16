# Postgre-SQL on Docker

```bash
# Running postgres
docker run --name postgresql -u "$(id -u postgresql):$(id -g postgresql)" -p 5432:5432 -v /var/lib/postgresql/data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=admin123 -d postgres

docker run --name postgresql -u "$(id -u postgresql):$(id -g postgresql)" -p 5432:5432 -v /var/lib/postgresql/data:/var/lib/postgresql/data -e PGDATA=/var/lib/postgresql/data -e DB_PORT=5432 -e POSTGRES_PASSWORD=admin123 -d postgres

docker run --name postgresql -u postgresql:postgresql -p 5432:5432 -v /var/lib/postgresql/data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=admin123 -d postgres

# Running postgres with a system user
# Creating user & group postgresql
# Todo: /var/log/pgadmin4/ for logs
sudo groupadd -r postgresql
sudo useradd -md /var/lib/postgresql -r -g postgresql -s /bin/false postgresql
id postgresql
sudo chown -R postgresql:postgresql /var/lib/postgresql/

# Base for pgadmin4
# https://hub.docker.com/r/fenglc/pgadmin4/
# https://github.com/fenglc/dockercloud-pgadmin4.git
# dockercloud-pgadmin4/Dockerfile for changes 

git clone 'https://github.com/fenglc/dockercloud-pgadmin4.git'
cd dockercloud-pgadmin4/
vim Dockerfile 
sudo docker build .
sudo docker images
sudo docker tag 2e58782924b1 fenglc/pgadmin4:selfbuilt
sudo docker images

sudo groupadd -r pgadmin4
sudo useradd -md /var/lib/pgadmin4 -r -g pgadmin4 -s /bin/false pgadmin4
id pgadmin4
sudo chown -R pgadmin4:pgadmin4 /var/lib/pgadmin4/

# Todo: /var/log/pgadmin4/ for logs

# Run selfbuilt pgadmin4
docker run --name pgadmin4 -u "$(id -u pgadmin4):$(id -g pgadmin4)" -v /var/lib/pgadmin4:/var/lib/pgadmin4 -v /var/lib/pgadmin4/pgadmin:/var/lib/pgadmin --link postgresql:postgres -p 5050:5050 -e DEFAULT_USER=$HOME -e DEFAULT_PASSWORD=admin123 -d fenglc/pgadmin4:selfbuilt

docker run --name pgadmin4 --link postgresql:postgres -p 5050:5050 -d fenglc/pgadmin4:selfbuilt

# DEFAULT_USER
# default 'pgadmin4@pgadmin.org'
# DEFAULT_PASSWORD
# default 'admin'

docker run --name pgadmin4 --link postgresql:postgres -p 5050:5050 -e DEFAULT_USER=$HOME -e DEFAULT_PASSWORD=admin123 -d fenglc/pgadmin4:selfbuilt

# Notes on databases
psql -h 127.0.0.1 -p 5432 -U postgres
#admin123
psql -h 127.0.0.1 -p 5432 -U JavaTestOwner JavaTestDB
#java123
```

