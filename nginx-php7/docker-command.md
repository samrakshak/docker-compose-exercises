### docker exec ###
Using `docker exec -it mysql-container /bin/bash` to using mysql-container bash inside.

**-i**: Interactive

**-t**: Allocate a pseudo-TTY

### docker volume ###
Using `docker volume ls` to show all the volumes.

Using `docker volume ls -qf dangling=true` to show all dangling volumes.

**-q**: quiet

**-f**: filter that using dangling=true

Using `docker volume prune` to clean all dangling volumes.
When using prune that show confirm option.

### docker-compose stop ###
Using `docker-compose stop` to stop all containers that defined by docker-compose.yml file.

### docker-compose rm -v ###
Using `docker-compose rm -v` to remove all containers that defined by docker-compose.yml file.

**-v**: Remove any anonymous volumes attached to containers

### docker-compose -f docker-compose-some.yml build ###
Using `docker-compose -f docker-compose-some.yml build` to rebuild docker-compose that contains Dockerfile.

### docker-compose -f docker-compose-some.yml up -d ###
Using `docker-compose -f docker-compose-some.yml up` to run specify docker-compose file and make containers up.

**-f**: Specify an alternate compose file (default: docker-compose.yml)

**-d**: Detached mode: Run containers in the background,Print new container names.
