### Docker exec ###
Using `docker exec -it mysql-container /bin/bash` to using mysql-container bash inside.

**-i**: Interactive

**-t**: Allocate a pseudo-TTY

### Docker volume ###
Using `docker volume ls` to show all the volumes.

Using `docker volume ls -qf dangling=true` to show all dangling volumes.

**-q**: quiet

**-f**: filter that using dangling=true

Using `docker volume prune` to clean all dangling volumes.
When using prune that show confirm option.
