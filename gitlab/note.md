#### Setup ####

##### Http setting #####
The setting of docker-compose.yml for gitlab that using non web port (80) that must using `external_url` config to set the different prot to gitlab web. That also set the ports arguments too.

if set the `external_url 'http://192.168.1.100:9981'` .that port must set to `9981:9981` cause the external_url will change nginx 80 port to 9981 inside the gitlab container.

##### SSH setting #####
Use different ssh port that must set the `gitlab_rails['gitlab_shell_ssh_port']` setction at the docker-compose file. And expose ssh port using customize ssh port if you don't use host defautl ssh port (22).

Usually, The host ssh port will taking by sshd services that will making the docker container alert ssh port was in used error.

If using {host ssh port}:{conatiner ssh port} like `2222:22` only that will making ssh link error on the project page. After added `gitlab_rails['gitlab_shell_ssh_port']` section the project page will show the ssh link correctly.

Error link: `git@192.168.1.100:bob/awesome-project.git`

Correct link: `ssh://git@192.168.1.100:2222/bob/awesome-project.git`

```yaml
environment:
    GITLAB_OMNIBUS_CONFIG: |
        gitlab_rails['gitlab_shell_ssh_port'] = 2222

ports:
    - '2222:22'
```


#### Backup in docker container ####
[Reference: blog.jscrambler.com](https://blog.jscrambler.com/migrating-your-gitlab-infrastructure-into-docker/)

[Reference: Gitlab Docs](https://docs.gitlab.com/omnibus/settings/backups.html)

##### Backup #####
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

##### Backup to aws s3 storage #####

Backup to remote storage like `s3`. and store backups into the folder of your bucket that will need add additional argument - `DIRECTORY=your-folder` statement end of the rake backup task. (instead of a global config setting) 

```sh
gitlab-rake gitlab:backup:create DIRECTORY=backups
```

[Gitlab issue reference:#23221](https://gitlab.com/gitlab-org/gitlab-ce/issues/23221)




##### Restore #####

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

##### Troubleshooting #####

After runing `gitlab-rake gitlab:backup:create` the output showing below:

``` sh
Dumping repositories ...
 * someone/awesome-app ... [DONE]
 * someone/awesome-app.wiki ...  [SKIPPED]
```

That meaing the `awesome-app.wiki` repository doesn't backup successfully.
    - case 1. The `awesome-app.wiki` repository was empty that doesn't backup into file
    - case 2. The `awesome-app.wiki` repository was **NOT** empty but doesn't backup into file.

In the case 2. You need use `sudo gitlab-rails console` to fix this problem. The follow step:

```rb
p = Project.find_by_full_path 'someone/awesome-app'
wiki = ProjectWiki.new p
wiki.repository.empty?
wiki.repository.expire_all_method_caches
wiki.repository.empty?
```
The `wiki.repository.empty?` will return true or false to present empty or not.

The output:
```rb
irb(main):003:0> wiki.repository.empty?
=> true
irb(main):004:0> wiki.repository.expire_all_method_caches
=> [:size, :commit_count, :readme, :version, :contribution_guide, :changelog, :license_blob, :license_key, :gitignore, :koding_yml, :gitlab_ci_yml, :branch_names, :tag_names, :branch_count, :tag_count, :avatar, :exists?, :empty?, :root_ref]
irb(main):005:0> wiki.repository.empty?
=> false

```

I think the key point was `wiki.repository.expire_all_method_caches` to flush all caches. After above steps I backup the skipped repository successfully.:tada:

[Gitlab issue Reference](https://gitlab.com/gitlab-org/gitlab-ce/issues/28854)
