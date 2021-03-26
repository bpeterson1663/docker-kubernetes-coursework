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

Secrets Storage
What is a secret
-   usernames and passwords
- TLS certificates and keys
- SSH Keys
- any data you would prefer not be on front page news
Only stored on disk on Manager nodes
Default is Managers and Workers "control plane" is TLS + Mutual Auth
Secrets are first stored in Swarm then assigned to a Service
Only containers in assigned Service can see them
They look like files in container but are actually in-memory fs
    /run/secrets/secret_name //key value store, key is the file name, value is what is in the file
docker-compose cli should never be used in production, not secure as far as secrets go. 
Secrets are a swarm only thing. docker-compose as a workaround for local development

vim psql_user.txt

docker secret create psql_user psql_user.txt
echo "myDBpassWORD" | docker secret create psql_pass - ABOVE_ID

docker service update --secret-rm
When removing a secret the container will be redeployed. Secret is part of the immutable design of services. Not ideal for databases

Using secrets in local development with docker compose
- use a bind mount with the secret file (only file based secrets)
- this is not secure but works great for local development

Full App Lifecycle With Compose
Override file - docker-compose.override.yml
    - This file will override any settings in the docker-compose.yml file when runnnig docker compose up. docker.compose.yml will get picked up first then will run the docker-compose.override.yml file
Use -f to specify a file, base file needs to be first then the override file

Service Updates:
    Provides rolling replacement of tasks/containers in a service
    Limits downtime (be careful with "prevents" downtime)
    Will replace containers for most changes
    Has many, many cli options to control the update
    Create options will usually change, adding -add or -rm to them
    Includes rollback and healthchoptions
    Also has scale and rollback subcommand for quicker access
    A stack deploy, when pre-existing, will issue service updates

docker service create -p 8080:80 --name web nginx:1.13.7

docker service scale web=5

docker service update --image nginx:1.13.6 web //updating to an older version

docker service update --publish-rm 8088 --publish-add 9090:80 web

docker service update --force web
