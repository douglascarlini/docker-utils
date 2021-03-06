# Docker Utils
Commands for Docker environment

### \# FTP

```bash
$ sudo docker run --name <container> --restart=always -v <path>:/home/vsftpd -p 20:20 -p 21:21 -p 47400-47470:47400-47470 -e FTP_USER=admin -e FTP_PASS=123456 -e PASV_ADDRESS=<ip> -d bogem/ftp
```

### \# MySQL

##### Create
```bash
$ sudo docker run -e MYSQL_ROOT_PASSWORD=<password> -e MYSQL_ROOT_HOST=% -p <port>:3306 -d mysql/mysql-server --default-authentication-plugin=mysql_native_password --character-set-server=utf8 --collation-server=utf8_unicode_ci
```

##### Create database

```bash
$ sudo docker exec -i <container> mysql -u root -p<password> -e "CREATE DATABASE <database>;"
```

##### Restore database
```bash
$ zcat <filename>.sql.gz | sudo docker exec -i <container> mysql -u root -p<password> <database>
```

##### Create database backup
```bash
$ sudo docker exec <container> mysqldump -u root -p<password> <database> | gzip > <filename>.sql.gz
```

##### Misc
```bash
# Fix error about 'ONLY_FULL_GROUP_BY'
$ sudo docker exec -it <container> mysql -u root -p<password> -e "SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));"

# Alter database global time zone
$ sudo docker exec -i <container> mysql -u root -p<password> -e "SET @@global.time_zone = '-03:00'";
```

### \# Misc

##### Clear Container Logs
```bash
echo "" > $(docker inspect --format='{{.LogPath}}' <container>)
```

##### Backup Container to File
```bash
$ sudo docker commit -p <container> <backup-name>
$ sudo docker save -o <backup-name>.tar <backup-name>
```

##### Restore Container Backup from File
```bash
$ sudo docker load −i <backup-name>.tar
$ sudo docker run −ti <backup−name>
```
