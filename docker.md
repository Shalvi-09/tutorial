
## Dockerfile

Docker File is used to create docker image, you can use images created by other as well and you can also create your own image

To create a docker file you need to create a "Dockerfile"( File name need not be "Dockerfile" it can be anything but thats convention)

So Docker File:

  A text file with instructions to build a docker Image.

Major Primary Componenent is 

```
FROM 
RUN
CMD


```

Whole process is 
1. Create a file name "Dockerfile"
2. Add Instruction in Dockerfile
3. Build dockerfile to create image
4. Run image to create Container

Commands:

`$ docker build .`
`$ docker build -t ImageName:Tag <directory of Dockerfile>`

`$ docker run <Imageid>`

### References:
[https://github.com/wsargent/docker-cheat-sheet#dockerfile](https://github.com/wsargent/docker-cheat-sheet#dockerfile)
[https://docs.docker.com/engine/reference/builder/#environment-replacement](https://docs.docker.com/engine/reference/builder/#environment-replacement)

## Docker tutorial

### Run

`Docker run` commands runs the docker image, if docker image are not vialable locally it will pull from docker hub or metioned location.


### list started comntainers

`docker ps` commands lists the running docker containers with certian details like `container Id`, `Name, Port, Status` etc.

**NOTE: In new docker versions `docker ps` is replaced with `docker container ls` command.**


`docker ps -a` to list all container runing or exited ones.

### Stop conntainer

`docker stop <container Name | docker Id>` it will stop the running container.

To show/list the stopped/running conatiner use `docker ps -a`

### REmove a docker container

`docker rm <docker name | Docker Id>` and it will print back your docker name.

to check it has been deleted or not run `docker ps -a`

### List docker image

`docker images` to list all the docker images

### Remove a docker image

Before you remove an image you need to make sure no container is running using that image. if its running u need to stop them and remove them. only then you can remove the docker image.

`docker rmi <image Name>`


### Pull - Download an image

Say you just want to download the docker image but do not want to run it, for that time rather later.  For that 

`docker pull <image name>`

now later when you run this image, docker will make use of this image, instead of pulling from remote.


























































































