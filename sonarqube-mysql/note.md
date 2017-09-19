### Dump data base from mysql ###

#### Backup ####
```sh
docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql
```

#### Restore ####
``` sh
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE
```
---

### Backup sonarqube ###
The sonarqube data stored inside `/opt/sonarqube/data`. If using container that use bind mount or volume to link this directory in compose file.

Bind mound:`/my/sonar/data:/opt/sonarqube/data`

Volumes: `my-sonar-volume:/opt/sonarqube/data`

#### Restore ####
1. Wait for container ready.
2. Restore db.
3. Restart sonarqube container.
