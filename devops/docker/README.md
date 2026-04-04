
# Docker on Ubuntu

[Official Docker installation guide for Linux (Ubuntu and others)](https://docs.docker.com/engine/install/)

| Step | Command | Description |
|---|---|---|
| 1 | sudo apt remove docker docker.io containerd runc | Uninstall old versions |
| 2 | curl -fsSL https://download.docker.com/linux/ubuntu/gpg \| sudo apt-key add - | Add Docker’s official GPG key |
| 3 | sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | Add official Docker repository |
| 4 | sudo apt update | Refresh apt cache |
| 5 | apt-cache policy docker-ce | Select Docker repository as default |
| 6 | sudo apt install docker-ce docker-ce-cli containerd.io | Install Docker |
| 7 | sudo systemctl status docker | Check Docker status |
| 8 | sudo usermod -aG docker ${USER} | Add current user to Docker group |
| 9 | docker --version | Check Docker version |

---

## Running a Web Server in a Docker Container

| Command | Description |
|---|---|
| docker container run -d -p 80:80 --name mysite1 nginx | Run nginx, port 80 |
| docker container run -d -p 8080:80 --name mysite2 nginx | Run nginx, port 8080 |
| docker container run -d -p 8081:80 --name mysite3 nginx | Run nginx, port 8081 |

- `-d`: detach and run in the background
- `--name`: container name, must be unique
- `-p X:Y`: publish container's port Y to host port X
- `-P`: publish all exposed ports to random ports

---

## Listing Images and Containers

| Command | Description |
|---|---|
| docker images | List images (old) |
| docker image ls | List images (new) |
| docker ps | List running containers (old) |
| docker container ls | List running containers (new) |
| docker ps -a | List all containers |
| docker container ls -a | List all containers |
| docker container ls -a -f status=exited | Filter by exited status |
| docker container ls -q | Print only container IDs |

---

## Stopping and Removing Containers

| Command | Description |
|---|---|
| docker container stop CONTAINER_ID/NAME | Stop container |
| docker container rm CONTAINER_ID/NAME | Remove stopped container |
| docker container rm -f CONTAINER_ID/NAME | Force remove running container |
| docker container rm $(docker container ls -f status=exited -q) | Remove all stopped containers |

---

## Removing Images and Pruning System

| Command | Description |
|---|---|
| docker rmi IMAGE_NAME | Remove image (old) |
| docker image rm IMAGE_NAME | Remove image (new) |
| docker system prune | Remove dangling images, stopped containers, unused networks |
| docker system prune -a | Remove unused images as well |

---

## Commit Changes in a Container to a New Image

1. Start container, get shell access, make changes, exit:

    ```bash
    docker container run -it --name=container1 centos
    ```

2. Create new image from modified container:

    ```bash
    docker commit -m "What did you do to the image" -a "Author Name" CONTAINER_ID/NAME repository/new_image_name:tag
    # Example:
    docker commit -m "nmap installed" -a "Andrei D." container1 ddandrei/my_centos
    ```

3. Start containers from the image:

    ```bash
    docker image ls
    docker container run -it ddandrei/my_centos
    ```

---

## Tagging and Pushing Images

| Command | Description |
|---|---|
| docker image tag nginx ddandrei/nginx:custom | Tag nginx image |
| docker image tag ddandrei/my_centos:latest ddandrei/my_centos:1.0 | Tag custom image |
| docker image ls | List images |
| docker login | Login to Docker Hub |
| docker image push ddandrei/nginx:custom | Push custom image |

---

## Docker Volumes

| Command | Description |
|---|---|
| docker volume create mysite | Create volume |
| docker volume ls | List volumes |
| docker volume inspect mysite | Inspect volume |
| docker volume rm mysite | Remove volume |
| docker volume prune | Remove all unused volumes |
| docker container run -d --name mywebapp -p 80:80 -v mysite:/usr/share/nginx/html nginx | Start container with volume |
