$ docker network create postgres

$ docker run -d \
    --name postgres \
    --restart always \
    --network postgres \
    -e POSTGRES_PASSWORD=postgres \
    -e PGDATA=/var/lib/postgresql/data/pgdata \
    -v /custom/mount:/var/lib/postgresql/data \
    -p 5432:5432 \
    postgres



$ docker run -p 4000:80 \
    --name pgadmin \
    --restart always \
    --network postgres \
    -e "PGADMIN_DEFAULT_EMAIL=postgres@postgres.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=postgres" \
    -d dpage/pgadmin4
    
-- Crear Backup de base de datos en postgres.
$ docker exec -t -u postgres pg-docker pg_dump -h localhost -p 5432 -U postgres -F c -b -v -f '/var/lib/postgresql/data/comprobantes_{{ansible_date_time.date}}.backup' comprobantes_test
    
-- Levantar Sql Server
$ docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=sa' -e 'MSSQL_PID=Express' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest-ubuntu
    
#cambiar clave postgres
$ psql
    ALTER USER postgres WITH PASSWORD 'postgres';
    CREATE ROLE produccion WITH LOGIN ENCRYPTED PASSWORD 'produccion';
--    sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"


#crear backup de una base de datos
pg_dump -h localhost -p 5432 -U postgres -F c -b -v -f "/var/lib/postgresql/comprobantes_03082017.backup" comprobantes_electronicos

#========archivo sh para respaldar base de datos
## date format ##
NOW=$(date +"%F")
NOWT=$(date +"%T")

PGPASSWORD="postgres" pg_dump -h localhost -p 5432 -U postgres -F c -b -v -f "/home/maint/backups/testlink-$NOW-$NOWT.backup" testlink


#--------tarea cron para respaldar base de datos====== 
crontab -e
30 1 * * * sh /home/maint/postgres_backup.sh



*******Despliegue de contenedores mysql y phpmyadmin
Example stack.yml for mysql:

# Use root/example as user/password credentials
version: '3.1'

services:

  db:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
Try in PWD

Run docker stack deploy -c stack.yml mysql (or docker-compose -f stack.yml up), wait for it to initialize completely, and visit http://swarm-ip:8080, http://localhost:8080, or http://host-ip:8080 (as appropriate).




************or***********test********


version: "3.7"
services:
  mysql-server:
    image: mysql:8.0.19
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - mysql-data:/var/lib/mysql
    ports:
      - "3306:3306"
   
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:5.0.1
    restart: always
    environment:
      PMA_HOST: mysql-server
      PMA_USER: root
      PMA_PASSWORD: secret
    ports:
      - "8080:80"
volumes:
  mysql-data: