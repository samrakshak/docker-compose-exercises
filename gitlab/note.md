##### Setup #####
The setting of docker-compose.yml for gitlab that using non web port (80) that must using `external_url` config to set the different prot to gitlab web. That also set the ports arguments too.

if set the `external_url 'http://192.168.1.100:9981'` .that port must set to `9981:9981` cause the external_url will change nginx 80 port to 9981 inside the gitlab container.


##### Backup in docker container ######
[Reference: blog.jscrambler.com](https://blog.jscrambler.com/migrating-your-gitlab-infrastructure-into-docker/)

[Reference: Gitlab Docs](https://docs.gitlab.com/omnibus/settings/backups.html)

###### Backup #####
```sh
docker exec -t <your container name> gitlab-rake gitlab:backup:create
```

Using `cron job` to routine backup.

Using `crontab -e` to write the cron fromat rules to `crontab` for particular user.

If want to customize system cron job that modifies the `/etc/crontab`.

Start backup gitlab cron job at 2 am :
```sh
* 2 * * * docker exec -t <your container name> gitlab-rake gitlab:backup:create
```


###### Restore #####

```sh
docker exec -it <your container name> bash
# stop services
gitlab-ctl stop unicorn  
gitlab-ctl stop sidekiq  
cd /var/opt/gitlab/backups  
chown git:git *_gitlab_backup.tar  
# Overwrite the contents of your GitLab database. It may take some time to complete, depending on how big your database is.
gitlab-rake gitlab:backup:restore BACKUP=<DATE> (e.g. 1460911132)  
gitlab-ctl start  
# Check if everything is ok. Watch out for errors and warnings
gitlab-rake gitlab:check SANITIZE=true  
```
