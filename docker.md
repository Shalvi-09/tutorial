
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

**When you run the docker Container, you may imediately see that in `exited` state. The reason is unlike virtual machines, docker are not meant to host os, but it is meant to run/execute  a specific task or process and as soon as that process completes, docker container exits from the running state.**


### Attach and detach commad

Whe u run the `docker run ...` commad by default it runs in attached mode, meaning it will not leave your terminal untill it exits.

So if u want to run the `docker run` in detached mode or in background use `-d` flag. Example: `docker run -d .....`

now if u want to attach to docker container later, you can use the

`docker attach <container Id>` command


### Getting interactive terminal 

By default docker container does not listen to standard input, even if it is attached to terminal. To map your standard input use `-i` option with it, and will start reading input from your terminal.

`docker -i ....`

But still it will have in printing intial standard output things for that use `-t` flag, and combine it with `-i` flag like below.


`docker -it ....`


### Port mapping

use `-p <Host Port>:<docker cotaier port>`

`docker -p 5432:1234 postgres` will map the docker cotainer port 1234 to host port at 5432.

`docker -p 8080:5000 myapp`

you can map multiple ports

`docker -p 5432:1234 -p 3000:4000 postgres`


### Volume mapping

use `-v` flag. So remember docker system has its own isolated file system and any changes happens only there. But what happens when you delete/remove your containers be it postgress or anything ? As soon as you delete your container all the data inside it also goes deleted.

So if you want to persist the data, you would want to map a directory outside the container on the docker host directly inside docker container.

`docker run -v /opt/datadir:/var/lib/mysql mysql` This way when containers runs, it mounts the internal file system to provided volume.
### Detailed Container information
Now `docker ps` command is okay to see the basic docker details but to see the detailed information like mapped volume, port , network etc. use

`docker inspect <docker_name>` 


### See the docker logs

`docker logs <container name>`



### Environment varibables

use `-e <VAEIABLE_NAME>=<VARIABLE VALUE>`

`docker run -e DB_HOST=http://localhost:8080 -p 5432:5432 postgres`


### creating your own image file


```
FROM Ubuntu

RUN apt update
RUN apt install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

```

Here RUN, COPY, ENTRYPOINT, FROM all are called docker command and whatever is there after command are commands arguments.

Now 

`docker build Dockerfile -t <username>/<image name>` if u want to push to  docker hub use

`docker push <username>/<image name>`




So docker uses layred approach to build the image, so Each line in docker file create a new layer which contains changes from just previous steps. So in case a build fails docker instead of building the entire image again it will resume from the last failed steps.


### CMD vs ENTRYPOINT


CMD CommadName param1, example: `CMD sleep 5`

        OR Use JSON Fromat
CMD ["Command Name" , "Parm1"] , example: `CMD["Sleep", "5"]`


So say if you want to override the command written in Dockerfile, for that u can simply anoth command from the terminal and it will override it.

`docker run ubuntu sleep 10` not it will slee for 10 sec insteand of 5, which was provided in dockerfile using CMD Command.



`CMD` and `ENTRYPOINT` are same only difference is when you pass command from your terminal, in case of CMD it will replace the provided command in docker file with terminal one but in case of `ENTRYPOINT` it NOT replace rather it will append to the command provided in docker file.


Say you have below docker file
//image name ubuntu-sleeper
```
FROM Ubuntu

CMD[ "sleep", "5"]

```

and you run from terminal like this

`docker run ubuntu-sleeper` it will sleep for 5 sec.


But, if you run  

`docker run ubuntu-sleeper sleep 10` on run time, it will replace the CMD["sleep", "5"] with CMD["sleep","10"] and ubuntu will now sleep for 10 sec. istead of 5.

BUT, if you are `ENTRYPOINT` in that case it will append.

Say you have below docker file
//image name ubuntu-sleeper
```
FROM Ubuntu

ENTRYPOINT[ "sleep"]

```

and you run from terminal like this

`docker run ubuntu-sleeper 10` on run time it will become ENTRYPOINT[ "sleep","10"] and it will sleep for 10 sec.


But, if you run  

`docker run ubuntu-sleeper` without providing the that parameter(10sec in this case) it will thorw error, as the provided entry point command is wrong.

To manage this scenario and provide a default time

Say you have below docker file
//image name ubuntu-sleeper
```
FROM Ubuntu

ENTRYPOINT[ "sleep"]
CMD["5"]

```
**NOTE: for this to work both must be in JSON format, else it will not work.**


so now if you dont provide the time at terminal it will still work as on nruntime bboth the command will be combined and it will become **ENTRYPOINT[ "sleep","5"]**

But if you provide then it will override the CMD one and ENTRYPOINT will finally become **ENTRYPOINT[ "sleep","10"]**




























































