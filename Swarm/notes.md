Swarm
- A clustering solution built inside Docker
- Manager and worker. Managers are workers with special set of permissions

docker info to determine if swarm is running

docker swarm init

docker swarm join --token TOKEN_FROM_INIT

docker node ls

docker service //docker run for swarm

docker service create alpine ping 8.8.8.8

docker service ls

docker service update qw8h6lk9n649 --replicas 3 // id is from the alpine service. updating the service to have 3 replicas. Id is from running docker service ls

If removing the container, the warm will bring up a new one in its place
In order to remove the swarm run, docker service rm SERVICE_NAME

play-with-docker.com
docker-machine + VirtualBox
Digital Ocean + Docker Install
get.docker.com