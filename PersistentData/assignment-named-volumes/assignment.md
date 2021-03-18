docker container run -d --name psql -v psql:/var/lib/postgresql/data postgres:9.6.1

docker container logs -f psql

docker container stop psql

docker container run -d --name psql2 -v psql:/var/lib/postgresql/data postgres:9.6.2

docker container logs psql2 -f