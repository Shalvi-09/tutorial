






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
1.  CReate a file name "Dockerfile"
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

