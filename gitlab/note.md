The setting of docker-compose.yml for gitlab that using non web port (80) that must using external_url config to set the different prot to gitlab web. That also set the ports arguments too.

if set the `external_url 'http://192.168.1.100:9981'` .that port must set to `9981:9981` cause the external_url will change nginx 80 port to 9981 inside the gitlab container.
