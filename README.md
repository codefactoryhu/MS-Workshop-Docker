# <span style="color:red">Lifecycle</span>

#### Creates a container but does not start it
```
docker create

# example
docker create -t -i fedora bash
docker start -a -i 6d8af538ec5
```

#### Allows the container to be renamed
```
docker rename

# example
docker rename my_container my_new_container
```

#### Creates and starts a container in one operation
```
docker run 

# example
docker run --name test -it debian
```

####  Deletes a container
```
docker rm

# Remove all stopped containers
docker rm $(docker ps -a -q)

# Remove a container and its volumes
docker rm -v redis
```

#### Updates a container's resource limits
```
docker update 

# example
docker run -dit --name test --kernel-memory 50M ubuntu bash
docker update --kernel-memory 80M test
```

#### Logging
```
There's also a logging driver available for individual containers in docker 1.10. To run docker with a custom log driver (i.e., to syslog), use docker run --log-driver=syslog.
```

# <span style="color:red">Starting and Stopping</span>
#### Starts a container so it is running
```
docker start 

# example
docker start my_container
```

#### Stops a running container
```
docker stop 

# example
docker stop my_container
```

#### Stops and starts a container
```
docker restart 

# example
docker restart my_container
```

#### Pauses a running container, "freezing" it in place
```
docker pause 

# example
docker pause my_container
```

#### Will unpause a running container
```
docker unpause 

# example
docker unpause my_container
```

#### Blocks until running container stops
```
docker wait 
```

#### sends a SIGKILL to a running container
```
docker kill 

# example
docker kill my_container
docker kill --signal=SIGHUP my_container
docker kill --signal=HUP my_container
docker kill --signal=1 my_containe
```

#### will connect to a running container
```
docker attach 
```

# <span style="color:red">CPU Constraints</span>
You can limit CPU, either using a percentage of all CPUs, or by using specific cores.

For example, you can tell the cpu-shares setting. The setting is a bit strange -- 1024 means 100% of the CPU, so if you want the container to take 50% of all CPU cores, you should specify 512. See https://goldmann.pl/blog/2014/09/11/resource-management-in-docker/#_cpu for more:

```
docker run -ti --c 512 agileek/cpuset-test
```

You can also only use some CPU cores using cpuset-cpus. See https://agileek.github.io/docker/2014/08/06/docker-cpuset/ for details and some nice videos:
```
docker run -ti --cpuset-cpus=0,4,6 agileek/cpuset-test
```

# <span style="color:red">Memory Constraints</span>
#### You can also set memory constraints on Docker:
```
docker run -it -m 300M ubuntu:14.04 /bin/bash
```

# <span style="color:red">Capabilities</span>
#### Give access to a single device:
```
docker run -it --device=/dev/ttyUSB0 debian bash
```

#### Give access to all devices:
```
docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb debian bash
```

# <span style="color:red">Info</span>
#### Shows running containers
```
docker ps 

# Show both running and stopped containers
docker ps -a

docker ps --filter status=running

```

#### Gets logs from container. (You can use a custom log driver, but logs is only available for json-file and journald in 1.10).
```
docker logs 
```

#### Looks at all the info on a container (including IP address)
```
docker inspect 

# Get an instance’s IP address
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_ID

# Get an instance’s MAC address
docker inspect --format='{{range .NetworkSettings.Networks}}{{.MacAddress}}{{end}}' $INSTANCE_ID

# Get an instance’s log path
docker inspect --format='{{.LogPath}}' $INSTANCE_ID

# Get an instance’s image name
docker inspect --format='{{.Config.Image}}' $INSTANCE_ID

# List all port bindings
docker inspect --format='{{range $p, $conf := .NetworkSettings.Ports}} {{$p}} -> {{(index $conf 0).HostPort}} {{end}}' $INSTANCE_ID
```

#### gets events from container
```
docker events 
```

#### shows public facing port of container
```
docker port 
docker port my_container
```

####  shows running processes in container
```
docker top 
```

#### shows containers' resource usage statistics
```
$ docker stats

CONTAINER ID        NAME                                    CPU %               MEM USAGE / LIMIT 
    MEM %               NET I/O             BLOCK I/O           PIDS
b95a83497c91        awesome_brattain                        0.28%               5.629MiB / 1.952GiB   0.28%               916B / 0B           147kB / 0B          9
67b2525d8ad1        foobar                                  0.00%               1.727MiB / 1.952GiB   0.09%               2.48kB / 0B         4.11MB / 0B         2
e5c383697914        test-1951.1.kay7x1lh1twk9c0oig50sd5tr   0.00%               196KiB / 1.952GiB     0.01%               71.2kB / 0B         770kB / 0B          1
4bda148efbc0        random.1.vnc8on831idyr42slu578u3cr      0.00%               1.672MiB / 1.952GiB   0.08%               110kB / 0B   
```

#### shows changed files in the container's FS
```
docker diff 
```

# <span style="color:red">Import / Export</span>
#### Copies files or folders between a container and the local filesystem
```
docker cp 
```

#### Turns container filesystem into tarball archive stream to STDOUT
```
docker export 
```

# <span style="color:red">Executing Commands</span>
#### To execute a command in container
```
docker exec 

# To enter a running container, attach a new shell process to a running container called foo, use: 
docker exec -it foo /bin/bash.
```

# <span style="color:red">Images</span>
#### Shows all images
```
docker images 
```

#### Creates an image from a tarball
```
docker import 

# Import from a remote location
docker import http://example.com/exampleimage.tgz

# Import from a local file
cat exampleimage.tgz | docker import - exampleimagelocal:new

# Import from a local directory
sudo tar -c . | docker import - exampleimagedir

# Import from a local directory with new configurations
sudo tar -c . | docker import --change "ENV DEBUG true" - exampleimagedir
```

#### Creates image from Dockerfile
```
docker build 
```

#### Creates image from a container, pausing it temporarily if it is running.
```
docker commit 
```

#### Removes an image
```
docker rmi 
```

#### Loads an image from a tar archive as STDIN, including images and tags (as of 0.7).
```
docker load 
```

#### Saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions (as of 0.7).
```
docker save 
```

#### Shows history of image.
```
docker history 
```

#### Tags an image to a name (local or registry).
```
docker tag 
```

# <span style="color:red">Example</span>

#### install docker
```
sudo yum install docker -y
```

#### enable docker
```
sudo systemctl enable docker
sudo systemctl start docker

alias docker="sudo /usr/bin/docker"
```

#### docker hub
```
docker login
```

#### docker command
```
docker ps
docker images
```

#### run first Docker (docker hub on megtekinthetoek) https://hub.docker.com/
```
docker run -d tutum/hello-world
```

#### run docker with port expose
```
docker run -d -p 8080:80 tutum/hello-world
```

#### how can we create own docker image
```
mkdir -p first-docker/src
cd first-docker
nano src/index.php
```

#### index.php
```php
<?php

phpinfo();
```
#### https://hub.docker.com/_/php/

```
nano Dockerfile
### Dockerfile
FROM php:apache
COPY src/ /var/www/html
EXPOSE 80
```

#### build image
```
docker build -t first-docker .
```

#### tag
```
docker tag first-docker zvigh/first-docker
```

#### docker hub push
```
docker push zvigh/first-docker
```

#### start first-docker container
```
docker run -d -p 8080:80 zvigh/first-docker
```
