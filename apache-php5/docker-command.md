### The final docker command line ###
docker run -d -p 80:80
-v ~/Documents/Github/docker-compose-exercises/apache-php5/src:/var/www/html --name php-container --link mysql-container:mysql php-image

**-v**: Using the volume to mapping host folder. ~/host/local/folder:/to/container/folder

**--link**: link the mysql container to php container to knows each other.
