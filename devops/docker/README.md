### Installing Docker on Ubuntu ###

#### uninstalling any old versions
sudo apt remove docker docker.io containerd runc


#### adding Docker’s official GPG signing key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


#### adding the official docker repository 
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

#### refreshing the apt cache
sudo apt update

#### selecting the docker repository as the default one
apt-cache policy docker-ce

#### installing docker
sudo apt install docker-ce docker-ce-cli containerd.io

#### checking its status
sudo systemctl status docker

#### adding the current user to the docker group to be able to run the docker command
sudo usermod -aG docker ${USER}

docker --version

---------------------------------------------------------------------------------------------------------------------

##### Running a Web Server in a Docker Container ##
docker container run -d --p 80:80 --name mysite1 nginx  
docker container run -d --p 8080:80 --name mysite2 nginx  
docker container run -d --p 8081:80 --name mysite3 nginx  

#### -d => detach and run in the background
#### --name => container name, must be unique, randomly chosen from a list if it's not given
#### -p X:Y => publish a container's port to the host 
#### process listens on port Y in the container but is accessed on port X from the outside (LAN/Internet)
#### -P => publish all exposed ports to random ports

#### listing local images
docker images # old command
docker image ls # new command

#### listing all running containers
docker ps # old command
docker container ls # new command
#### -q => printing only the containers' ids

#### listing all containers (created, running, stopped)
docker ps -a
docker container ls -a

#### filtering by status
docker container ls -a -f status=exited

#### stopping a container
docker container stop CONTAINER_ID|CONTAINER_NAME
Example: docker container stop mysite1

#### removing a container (must be stopped)
docker container rm CONTAINER_ID|CONTAINER_NAME
#### -f => force remove the container (can be running)
Example: docker container rm mysite1

#### removing all stopped containers
docker container rm $(docker container ls -f status=exited -q)

#### removing an image
docker rmi IMAGE_NAME # => old command
docker image rm IMAGE_NAME # => new command
Example: docker image rm nginx

#### removing dangling images, stopped containers, dangling build cache and networks not used
docker system prune
#### -a => remove unused images, as well

---------------------------------------------------------------------------------------------------------------------

##### Commiting changes in a container to a new image

#### STEP 1

#### start the container, get shell access, make any changes and exit

docker container run -it --name=container1 centos

#### STEP 2

#### create a new image from the modified container

docker commit -m "What did you do to the image" -a "Author Name" CONTAINER_ID|CONTAINER_NAME repository/new_image_name:tag

Example: docker commit -m "nmap installed" -a "Andrei D." container1 ddandrei/my_centos

#### STEP 3

#### start containers from the image

docker image ls

docker container run -it ddandrei/my_centos

#### adding a new tag to an existing image

docker image tag nginx ddandrei/nginx:custom

docker image tag ddandrei/my_centos:latest ddandrei/my_centos:1.0

docker image ls

#### pushing custom images to docker Hub

#### STEP 1

#### create an account on hub.docker.com

#### STEP 2

docker login => enter username and password

#### STEP 3

docker image push ddandrei/nginx:custom # => the username on docker hub must match the image's repository (ddandrei)

---------------------------------------------------------------------------------------------------------------------

#### creating a new volume
docker volume create mysite

#### listing all volumes
docker volume ls

#### inspecting a volume
docker volume inspect mysite

#### removing a volume
docker volume rm mysite
docker volume prune # => remove all unused volumes

#### starting a container with the volume
docker container run -d --name mywebapp -p 80:80 -v mysite:/usr/share/nginx/html nginx

---------------------------------------------------------------------------------------------------------------------





---------------------------------------------------------------------------------------------------------------------
