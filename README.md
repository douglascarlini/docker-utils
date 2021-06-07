# Docker Utils
Commands for Docker environment

### \# FTP

```bash
docker run --name <container> --restart=always -v <path>:/home/vsftpd -p 20:20 -p 21:21 -p 47400-47470:47400-47470 -e FTP_USER=admin -e FTP_PASS=123456 -e PASV_ADDRESS=<ip> -d bogem/ftp
```

### \# MySQL

##### Create
```bash
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_ROOT_HOST=% -p 3306:3306 -d mysql/mysql-server --default-authentication-plugin=mysql_native_password --character-set-server=utf8 --collation-server=utf8_unicode_ci
```

##### Create (v5.7)
```bash
docker run --name mysql57 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_ROOT_HOST=% -p 3306:3306 -d mysql/mysql-server:5.7 --character-set-server=utf8 --collation-server=utf8_unicode_ci
```

##### Create (mariaDB)
```bash
docker run --name mariadb -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_ROOT_HOST=% -p 3306:3306 -d ghcr.io/linuxserver/mariadb
```

##### Create database

```bash
docker exec -i mysql57 mysql -u root -p123456 -e "CREATE DATABASE dbname;"
```

##### Restore database
```bash
zcat dbname.sql.gz | docker exec -i mysql57 mysql -u root -p123456 dbname
```

##### Create database backup
```bash
docker exec mysql57 mysqldump -u root -p123456 dbname | gzip > dbname.sql.gz
```

##### Misc
```bash
# Fix error about 'ONLY_FULL_GROUP_BY'
docker exec -it <container> mysql -u root -p<password> -e "SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));"

# Alter database global time zone
docker exec -i <container> mysql -u root -p<password> -e "SET @@global.time_zone = '-03:00'";
```

### \# Misc

##### Clear Container Logs
```bash
echo "" > $(docker inspect --format='{{.LogPath}}' <container>)
```

##### Backup Container to File
```bash
docker commit -p <container> <backup-name>
docker save -o <backup-name>.tar <backup-name>
```

##### Restore Container Backup from File
```bash
docker load −i <backup-name>.tar
docker run −ti <backup−name>
```
