What is an Image
- App binaries and dependencies
- Metadata about the image data and how to run the image
- Official definition: "An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime."
- No kernel, host provides the kernel

Images are not named but tagged

Image Layers
docker image history nginx:latest // returns the history of the layers
Layers are cached and the same layer can be used in multiple images on the file system to safe time and space

docker image tag nginx bradypeterson/nginx

docker image push bradypeterson/nginx

docker login - to have access to dockerhub

https://docs.docker.com/engine/reference/builder/ 

docker image build -t customimage . // the . references the dockerfile in the current director, -t tagging the image as customimage

docker container run -p 80:8080