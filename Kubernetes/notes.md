Kubernetes
- Makes many servers act like on
- Runs on top of Docker as a set of API's in containers
- Provides API/CLI to manage containers across servers
- Many clouds provide it for you
- Many vendors make a "distribution" of it

Why Kubernetes
- First undersant why you may need orchestration
- Not everys olution needs orchestration
- Servers + Change Rate = benefit of orchestaration
- Decide which orchestrator
- If Kubernets, decide which distribution
    Cloud or self-managed (Docker Enterprise, Rancher, Openshift VMware)

Kubernetes vs Swarm
- Swarm: Easier to deploy/manage
- Swarm: Comes with docker, single vendor container platform
- Swarm: Swarm solves a majority of use cases
- Swarm: Secure by default
- Swarm: Easier to troubleshoot
- Kubernetes: More features and flexibility
- Kubernetes: Widest adoption and community
- Kubernetes: Flexible - covers widest set of use cases
- Kubernetes: vendor support

Terms
- Kubectl: CLI to configure Kubernetes and manage apps
    - Using "cube control" official pronunciation
- Node: Single server in the Kubernetes cluster
- Kublet: Kubernetes agent running on nodes
- Control Plane: Set of containers that manage the cluster
    - Includes API server, scheduler, controller manager, etcd and more
    - Sometimes calle the maseter
- Pod: one or more containers running together on one Node
    - Basic unit of deployment. COntainers are always in pods
- Controller: For creating/updating pods and other objects
    - Many types of Controllers inc. Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob, etc. 
- Service: network endpoint to connect to a pod
- Namespace: Filtered group of objects in cluster

kubectl run (changing to be only for pod creation)
kubectl create (create some resources via CLI or YAML)
kubectl apply (create/update anyting via YAML)

kubectl run my-nginx --image nginx

kubectl get pods

kubectl logs deployment/my-apache

Exposing Containers
- "kubectl expose" creates a service for existing pods
- A service is a stable address for pod
- CoreDNS allows us to resolve services by name
- There are different types of services
    - ClusterIP
        - default
        - Single, internal virtual IP allocated
        - only reachable from within cluster
        - Pods can reach service on apps port number
    - NodePort
        - Outside the cluster talks to the service
        - High port allocated on each node
        - Port is open on every node's IP
        - Anyone can connect (if they can reach node)
        - Other pods need to be updated to this port
    - LoadBalancer
        - Mostly used in the cloud
        - Controls a LB endpoint external to the cluster
        - Only available when infra provider gives you a LB (AWS ELB, etc)
        - Creates NodePort+ClusterIP services, tells LB to send to NodePort
    - External Name
        - Adds CNAME DNS record to CoreDNS only
        - Not used for Pods, but for giving pods a DNS name to use for something outside Kubernetes

kubectl create deployment httpenv --image=bretfisher/httpenv

kubectl scale deployment/httpenv --replicas=5

kubectl expose deployment/htpenv --port 8888

kubectl run --generator run-pod/v1 tmp-shell --rm -it --image bretfisher/netshoot -- bash

kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort

kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer

--dry-run -o yaml //outputs what the command would do in a yamls file. useful for testing.  

Imperative vs Declarative
- Imperative: Focus on how a program operates
- Declarative: Focus on what a program should accomplish

Imperative
- Examples: kubectl run, kubectl create deployment, kubectl update
    - we start with a state we know
    - we ask kubectl run to create a deployment
- Different commands are required to change that deployment
- Different commands are required to change that deployment
- Different commands are required per object
Imperative is easier when you know the state
- Imperative is easier to get started
- Imperative is easier for humans at the CLI
- Imperative is NOT easy to automate

Declarative
- Examples: kubectl apply -f my-resources.yaml
    - We don't know the current state
    - We only know what we want the end result to be (yaml contents)
- Same command each time (exception for delete)
- Resources can be all in a file,k or many files (apply a whole dir)
- Requires understanding the YAML keys and values
- More work than kubectl run for just starting a pod
- The easiest way to automate
- The eventual path to Git happiness

Storage in Kubernetes
- Storage and stateful workloads are harder in all systems
- Containers make it both harder nad easier than before
- StatefulSets is a new resource type, making Pods more sticky
- Use db-as-a-service whenever you can

Volumes in Kubernetes
- Volumes
    - Tied to lifecycle of Pod
    - All containers in a single Pod can share them
- Persistent Volumes
    - Created at the cluster level, outlives a Pod
    - Separates storage config from Pod using it
    - Multiple Pods can share them
- CSI plugins are the new way to connect to storage (Container Storage Interface)

- Ingress
    - None of our Services types work at OSI Layer 7 (HTTP)
    - How do we route outside connections based on hostname or URL?
    - Ingress Controllers (optional) do this with 3rd party proxies
    - Nginx is popular, but Traefik, HAProxy, F5, Envoy, Istio, etc

CRD's and The Operator Pattern
- You can add 3rd party resources and controllers
- This extends Kubernetes API and CLI
- A pattern is starting to emerge of using these together
- Operator: automate deployment and management of complex apps
- Examples: Databases, monitoring tools, backups, and cusotm ingresses

Higher Deployment Abstractions
- All our kubectl commands just talk to the Kubernetes API
- Kubernetes has limited built-in templating, versioning, tracking and management of your apps
- There are now over 60 3rd party tools to do that, but many are defunct
- Helm is the most popular
- Compose on Kubernetes comes with Docker Desktop