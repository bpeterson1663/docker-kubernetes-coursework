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

Overlay Multi-Host Networking
    --driver overlay // used when creating a network
    - For container-to-container traffic inside a single Swarm
    - Optional IPSec (AES) encyption on network creation
    - Each service can be connected to multiple networks

// Create network
docker network create --driver overlay MyDrupal
// Create service
docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgress // created on node 1
docker service create --name drupal --network mydrupal -p 80:80 drupal //created on node 2

Routing Mesh
- Routes ingress (incoming) packets for a Service to proper Task
- Spans all nodes in Swarm
- Uses IPVS from Linux Kernel
- Load balances Swarm Services across their Tasks
Two ways this works
    - Container-to-container in a Overlay network (use VIP)
    - External Traffic incoming to published ports (all nodes listen)
    - creates a virtual IP which load balances the incoming to the nodes
- this is statelss load balancing
- LB is at OSI Layer 3 (TCP), not Layer 4 (DNS)
- Both limitation can be overcome with:
    - Nginx or HAProxy LB proxy,
    - Docker Enterprise Edition, which comes with built-in L4 web proxy
docker service create --name search --replicas 3 -p 9200:9200 elasticsearch:2

Stacks
- Compose files as their declarative definition for services, networks, and volumes
- We use docker stack deploy rather then docker service create
- Stacks manages all those objects for us, including overlay network per stack. Adds stack name to start of their name
- New deploy: key in Compose file. Can't do build. Shouldnt build in production
- Compose now ignores deploy:, Swarm ignores build: 
- docker-compose cli not needed on Swarm server

docker stack deploy -c example-voting-app-stack.yml voteapp

docker stack services voteapp

docker stack ps voteapp
