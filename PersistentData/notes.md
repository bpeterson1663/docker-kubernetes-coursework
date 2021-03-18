Persistent Data (unique data)
Containers are usually immutable and ephemeral (unchangeable as far as binaries and dependencies)

Volumes need manually deletion 

docker container run -d --name mysql -e MYSQL_ALLOW_PASSWORD=True msyql

docker container inspect mysql //can see the volume and mount

Name Volumes
docker container run -d --name mysql -e MYSQL_ALLOW_PASSWORD=True -v mysql-db:/var/lib/mysql mysql // -v NAME:LOCATION

Bind Mounting
- Maps a host file or directory to a container file or directory
- Basically just two locations pointing to the same files

docker container run -d --name nginx -p 80:80 -v $(pwd):/user/share/nginx/html nginx

