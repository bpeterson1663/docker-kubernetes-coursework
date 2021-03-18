docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD= yes mysql

docker container logs db // GENERATED ROOT PASSWORD: if we need the password to log i n

docker container run -d --name webserver -p 8080:80 httpd

docker container run -d --name proxy -p 80:80 nginx

docker container ls

docker container rm IDS