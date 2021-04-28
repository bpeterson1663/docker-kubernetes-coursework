https://github.com/BretFisher/ama/issues/17 //Security recommendations for Docker on Linux servers, in order of priority.

https://docs.docker.com/engine/security/

https://sysdig.com/blog/20-docker-security-tools/


https://github.com/docker/docker-bench-security // Scans for best practice configurations 

RUN groupadd --gid 1000 node \
    && useradd --uid 1000 --gid node --shel /bin/bash -create-home node

Preventing users from breaking out of the container and onto the host
- Enable user namespace
- https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/
- https://docs.docker.com/engine/security/userns-remap/