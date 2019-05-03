Docker cheat sheet
==================

Display Docker version and info:

`docker --version` or `docker version`
`docker info`

Image
-----

Execute Docker image:

`docker run *image-name*`

`-p *exposed-port*:*app-port*` Port parameter
`-d` Run in dettached mode

List Docker images:

`docker image ls`

List Docker containers (running, all, all in quiet mode):

`docker container ls`
`docker container ls --all`
`docker container ls -aq`

Services
--------

Initialize and take down the swarm manager:

`docker swarm init`
`docker swarm leave --force`

Deploy and remove a stack:

`docker stack deploy -c *compose-file* *stack-name*`
`docker stack rm *stack-name*`

List services:

`docker service ls`

List a service's tasks:

`docker service ps *stack-name*`

Miscellaneous
-------------

Push an image to the Docker repository:

`docker push *username*/*repository*:*tag*`