# L05-04

Type the following Docker commands:

## Pull and run a Nginx server

    docker run -d -p 8080:80 --name webserver nginx

## List the running containers

    docker ps

## List the images

    docker images

## Attach to the container

    docker container exec -it  webserver bash  
    ## Attaches a terminal to the running container.

Exit by typing

    exit

## Stop the container

    docker stop webserver

## Remove the container from memory

    docker rm webserver

## Remove the image

    docker rmi nginx

## Check Live Resource Usage for All Containers

    docker stats
    ## This displays CPU, memory, network, and I/O usage for all running containers.

    ## Check Resource Usage for a Specific Container
 
    docker stats <container_id_or_name>
    ## Replace <container_id_or_name> with the actual container ID or name.

## Check Detailed Information About a Container

    docker inspect <container_id_or_name>
    ## This provides detailed info, including resource limits.

## Check CPU and Memory Usage (Historical Data)

    docker top <container_id_or_name>
    ## This shows the running processes inside the container.

## Check Disk Space Used by Containers

    docker system df