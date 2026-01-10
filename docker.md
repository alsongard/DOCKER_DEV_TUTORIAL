
# DEVOPS
DevOps is a development methodology aimed at bridging the gap between Development and Operations, emphasizing communication and collaboration, continuous integration, quality assurance and delivery with automated deployment utilizing a set of development practices".



Docker is a set of tools to deliver software in containers.
Containers are packages of software.



**Docker CLI Basics**
- command line interface (CLI) client
- a REST API
- Docker daemon

    
More information on command use:
[link](https://docs.docker.com/engine/reference/commandline/cli/)  


To run an image we use:
```bash
docker container run <image_name>
```

To automatically run and remove a container we use:
```bash
docker container run --rm imageName
```

To remove an image we use:
```bash
docker image rm <image_name>
```

However before removing an image we need to stop and remove any containers using that image.

so first we check to see if any containers are running:
```bash
docker container ls
docker ps
docker container ls -a
```

using the id from the above command we can stop and remove the container:
```bash
docker container stop <container_id>
docker container rm <container_id>
``` 

If you have hundreds of stopped containers and you wish to delete them all, you should use docker container prune. Prune can also be used to remove "dangling" images with docker image prune. Dangling images are images that don't have a name and are not used. They can be created manually and are automatically generated during build. Removing them just saves some space.

And finally you can use docker system prune to clear almost everything. We aren't yet familiar with the exceptions that docker system prune doesn't remove.


```bash
docker run imagename
dokce stop container cointainerId
docker system prune: remove images
docker container prune: remove containers not running
docerk image prune: remove images not in uuse
```



To get an imge  we use the command:
```bash
docker image pull <image_name>
```



command	explain	shorthand
| command | Description |
| --- | --- |
| docker image ls |	Lists  all images	docker images
| docker image rm <image> | 	Removes an image	docker rmi
| docker image pull <image> | 	Pulls image from a docker registry	docker pull
| docker container ls -a	 | Lists all containers	docker ps -a
| docker container run <image> | 	Runs a container from an image	docker run
| docker container rm <container> | 	Removes a container	docker rm
| docker container stop <container> | 	Stops a container	docker stop
| docker container exec <container> | 	Executes a command inside the container 	docker exec



# Starting to use Docker
1. Download Ubuntu image
```bash
docker image pull ubuntu:latest
```

To interact with the ubuntu image we: 
- run a container from the image
check if image is downloaded:
```bash
docker image 
```
```bash
docker container run -it ubuntu:latest
``` 

The flags used: 
-i : interactive
-t : terminal


```bash
docker attach <name> 
docker attach <container_id>
```
This brings the container into the foreground.

To pause a container we use the command:
```bash
docker puase <container_id>
docker puase <container_name>
```

To unpuase the container we use:
```bash
docker unpause <container_id>
docker unpause <container_name>
``` 

Now to check the logs of the container we use:
```bash
docker logs -f <container_id>
```
The -f flag is used to follow the logs in real time.

At any time we want to attach a container without stopping it when entering commands in the terminal we use: ``--no_stdin`` flag
```bash
docker attack  --no_stdin looper
```

At any time we want to execute commands inside a running container we use:
```bash
docker exec  container_name command
```
Example:
```bash
docker exec looper ls -l
```
The above command list the files in the container named looper.
We can also execute a bash shell and then run commands inside the container:
```bash
docker exec -it container_name bash
```

Example:
```bash
docker exec -it looper bash
```

Now if you want to remove the container after exiting we use the flag: ``--rm``
```bash
docker run --rm -it ubuntu bash
```

**Scenario Problems**
At some point you may want to run a command for a given image we use
```bash
docker run <image_name> sh -c "command"
```
Example:
```bash
docker run ubuntu sh -c "echo Hello World"
```

Now let's say if your are missing some dependencies you can install them using apt we do the following:
- Get into the container
```bash
docker exec -it container_id bash
```
- Update apt
```bash
apt update
apt install application_name
```


## In depth to Images
Images are the basic building blocks for containers and other images. When you "containerize" an application you work towards creating the image.  
By learning what images are and how to create them, you'll be ready to start utilizing containers in your own projects.

[Link](https://hub.docker.com/)

Better yet we could search for images using the command:
```bash
docker search <image_name>
``` 


To search for images in other registries we can use:
we use the following:
Example: [quay.io](https://quay.io/)

```bash
docker search quay.io/<image_name>
```

Example:
```bash
docker search quay.io/hello-world
```

To pull the image we use:
```bash
docker pull quay.io/hello-world
```


**Tags**
When downloading images one should be aware of image tags. Tags are used to identify different versions of the same image. If no tag is specified the latest tag is used by default.

Take for example the ubuntu image. There are different versions of ubuntu images available. To see the different tags we can visit the docker hub page for ubuntu: [link](https://hub.docker.com/_/ubuntu)
Reading the documentation reveals that the latest tags points to LTS(long-term support) versions of ubuntu.

**Docs**
The ubuntu:latest tag points to the "latest LTS", since that's the version recommended for general use. The ubuntu:rolling tag points to the latest release (regardless of LTS status).


When downloading images from docker hub it consist of layers. This layers are downloaded in parallel to speed up the download process. 

We can also tag images locally using:
```bash

docker tag image:tag image:new_tag
docker tag ubuntu:latest new_image_name:new_tag
```

To summarize, an image name may consist of 3 parts plus a tag. Usually like the following: registry/organisation/image:tag. But may be as short as ubuntu, then the registry will default to Docker hub, organisation to library and tag to latest. The organisation may also be a user, but calling it an organisation may be more clear.


**Scenario Practise**
Even after running your container and you may need to get a password fro the container itself you can use the ``exec`` command to run commands inside the container.

Example:  
Image Name: devopsdockeruh/pull_exercise
```bash
docker image pull devopsdockeruh/pull_exercise
docker run -it --name exercise devopsdockeruh/pull_exercise
# after which you wil be requested some input
docker exec -it  exercise cat README.md
```


## Building Images
To build images we use a file called Dockerfile.

now to build our image we use the command ``docker build``

By default docker build will look for a file named Dockerfile. Now we can run docker build with instructions where to build (.) and give it a name (-t <name>):


Example of a Dockerfile
```Dockerfile
# Alpine Linux is a security-oriented, lightweight Linux distribution based on musl libc and busybox.
FROM alpine:lates


# Install Bash
RUN apk update && apk add bash

# Use /usr/src/app as your working directory: All command will be executed in this directory
WORKDIR /usr/src/app


# Copy file hello.sh to working directory
COPY hello.sh .

# modify file permission
# chmod u+x hello.sh

# Command to execute when running docker
CMD ["./hello.sh"]
```
What is the use of the -t flag?
The -t flag allows us to tag our image with a name so that we can easily find it later.


When building we can see 3 steps namely:
1/3
2/3
3/3
```bash
=> [1/3] FROM docker.io/library/alpine:latest@sha256:865b95f46d98cf867a156fe4a135ad3fe50d2056aa3f25ed31662dff6  0.0s
=> [internal] load build context                                                                                0.0s
=> => transferring context: 66B                                                                                 0.0s
=> CACHED j[2/3] WORKDIR /usr/src/app                                                                            0.0s
=> [3/3] COPY hello.sh .     
```
The above is the output of the command 
```bash
docker build -t myfirstDocker .
```

Syntax: ``docker build -t name2Image pathtoDockerFile``
Layers have multiple functions. We often try to limit the number of layers to save on storage space but layers can work as a cache during build time. Optimization:Chapter 4


one cad add files to a system thru the cfollowing:
```
docker cp additional.txt 3c9209d5e238:/usr/src/app 
```


**docker diff**  
List the changed files and directories in a container᾿s filesystem since the container was created. Three different types of change are tracked:  

| Symbol |	Description |
| --- | --- |
| A | A file or directory was added |
| D | A file or directory was deleted |
| C | A file or directory was changed |


```bash
C /usr
C /usr/src
C /usr/src/app
A /usr/src/app/additional.txt
C /root
A /root/.ash_history
```

After making changes to your container you may want to save the container we do the following:
```bash
docker commit 3c9209d5e238 changed-docker
```

Save changes of the above container to an image


**RUN cmd**
In your Dockerfile, `EOF` is a **delimiter** that marks the beginning and end of a heredoc (here document) block. It allows you to write multi-line commands inside a `RUN` instruction without chaining them with `&&`.

Example:
```dockerfile
RUN <<EOF
apt-get update
apt-get install -y curl
EOF
```

- `<<EOF` starts the block.
- All lines after are treated as part of the script.
- The block ends when Docker encounters a line with just `EOF`.

You can use any identifier (e.g., `END`, `SCRIPT`), but `EOF` (End Of File) is common by convention.



**CMD**
CMD is executed when we call docker run: ``docker contianer run --name containerName imageName:tag``


One can have multiple Dockerfile within the same directory the only difference is that you add a tag or a word: ``<Dockerfile.something>``
Example:
Dockerfile.test   
```bash
docker buildj -t imageName -f fileName directory/pathtoDockerfile.something
docker build -t testing -f Dockerfile.test .
docker container run --name testerv1 hello-tester
```



**Entry Point**
Check the history of a downloaded image:
```bash
docker history imageName
```

The Docker documentation CMD (opens in a new tab) (opens in a new tab) says a bit indirectly that if a image has ENTRYPOINT defined, CMD is used to define it the default arguments.(if an image has an entrypoint one can specify arguments when running the container)

simple-web-service image
```Dockerfile
# GET YOUR MINIMAL IMAGE: WHAT IS NECESSARY TO RUN SCRIPT

FROM devopsdockeruh/simple-web-service

CMD ["server"]
```

When writting our Dockerfile we must always put lines that are prone to changes(likely to change) at the bottom inorder to enhance/reduce building time of images(efficiency)


## Download videos exercise
When using curl the ``-o`` flag is used to specify the path and filename of the save file:
e.g
```bash
curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o ~/.local/bin/yt-dlp
```

the ``-o`` in the current directory create local/bin and save as ``yt-dlp``


**sh**
The ``sh`` shell when set at the end of the following opens up a shell:
```bash
docker exec -it containerName sh
```
To run a specific command in non-interactively we use:
```bash
docker exec -it containerName  sh -c "your_command_here"
```

By default the entrypoint for any docker is set to: ``/bin/sh -c`` if no ENTRYPOINT is set.

If no argument is given we can set: 
```Dockerfile
CMD ["https://url"]
```

There are two ways to set the ENTRYPOINT and CMD: exec form and shell form. We have been using the exec form, in which the command itself is executed. The exec form is generally preferred over the shell form, because in the shell form the command that is executed is wrapped with /bin/sh -c, which can result in unexpected behaviour. However, the shell form can be useful in certain situations, for example, when you need to evaluate environment variables in the command like $MYSQL_PASSWORD or similar.

In the shell form, the command is provided as a string without brackets. In the exec form the command and its arguments are provided as a list (with brackets), see the table below:



|Dockerfile |	Resulting command | 
| ---- | ---- |
| ``ENTRYPOINT /bin/ping -c 3 CMD localhost`` | /bin/sh -c '/bin/ping -c 3' /bin/sh -c localhost |
| ``ENTRYPOINT ["/bin/ping","-c","3"] CMD localhost`` | /bin/ping -c 3 /bin/sh -c localhost |
| ``ENTRYPOINT /bin/ping -c 3 CMD ["localhost"]`` | /bin/sh -c '/bin/ping -c 3' localhost |
| ``ENTRYPOINT ["/bin/ping","-c","3"] CMD ["localhost"]`` | /bin/ping -c 3 localhost |



# Interacting with the container via volumes and ports

using volumes:
In the command: docker container run -v "$(pwd):/mydir" imageName
    -v is the flag for volumes.
    "$(pwd)" is a command substitution that gets the current working directory on the host (where you are running the command).
    :/mydir specifies the directory inside the container where the host directory will be mounted.


```bash
docker run -v "$(pwd):\mydir" --name trial-vidoes download-videos:v1
```
This downloads a video
A Docker volume is essentially a shared directory or file between the host machine and the container. When a program running inside the container modifies a file within this volume, the changes are preserved even after the container is shut down and removed, as the file resides on the host machine.


The flag: ``-v "$(pwd)"`` makes this directory accessible to my container and any changes that either occur in the container or my device is seen in either (container and my machine)


The -v "$(pwd):/mydir" bind mount creates a two-way (bidirectional) synchronization between your current host directory and /mydir in the container:
    Files from your current directory are immediately available inside the container.
    Any changes made inside the container (e.g., new files, edits, deletions) are reflected on the host.
    Any changes made on the host are immediately visible inside the container.
This behavior is the default for Docker bind mounts on most platforms, though performance and edge cases (e.g
., file deletion, inotify events) may vary on macOS and Windows due to filesystem translation layers.


Solution:
```bash
docker container run -v "$(pwd)/text.log:/usr/src/app/text.log" --name simple-web devopsdockerruh:simple-web-service:ubuntu
```


## Allowing external connections into containers​
Sending messages: Programs can send messages to URL (opens in a new tab) (opens in a new tab) addresses such as this: http://127.0.0.1:3000 (opens in a new tab) (opens in a new tab) where HTTP is the protocol (opens in a new tab) (opens in a new tab), 127.0.0.1 is an IP address, and 3000 is a port (opens in a new tab) (opens in a new tab). Note the IP part could also be a hostname (opens in a new tab) (opens in a new tab): 127.0.0.1 is also called localhost (opens in a new tab) (opens in a new tab)
 so instead you could use http://localhost:3000.
Receiving messages: Programs can be assigned to listen to any available port. If a program is listening for traffic on port 3000, and a message is sent to that port, the program will receive and possibly process it.


It is possible to map a port on your host machine to a container port. For example, if you map port 1000 on your host machine to port 2000 in the container, and then send a message to http://localhost:1000 on your computer, the container will receive that message if it's listening to its port 2000.


Opening a connection from the outside world to a Docker container happens in two steps:
    Exposing port
    Publishing port
Exposing a container port means informing Docker that the container listens to a certain port. This doesn't do much, except it helps humans with the configuration.
Publishing a port means that Docker will map host ports to the container ports.
To expose a port, add the line EXPOSE <port> in your Dockerfile
To publish a port, run the container with -p <host-port>:<container-port>
If you leave out the host port and only specify the container port, Docker will automatically choose a free port as the host port:



```bash
docker run -p 4567:contianer_port imageName
```
We could also limit connections to a certain protocol only, e.g. UDP by adding the protocol at the end: EXPOSE <port>/udp and -p <host-port>:<container-port>/udp.


# Utilizing tools from the Registry
```bash
docker container run -p 5000:3000 gemtrial
```