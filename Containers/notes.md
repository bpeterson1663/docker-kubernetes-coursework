Image vs. Container
An image is the application we want to run
A Container is an instance of that image running as a process
You can have many containers running off the same image

docker container run --publish 80:80 --detach nginx
nginx is the image
publish exposes port 80 on the host IP
routes the traffic to the container IP, port 80
