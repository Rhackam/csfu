![image](https://github.com/Rhackam/csfu/assets/80326553/c41e33b9-95cc-443e-8d09-228069416487)# Services Cheatsheet

## :octocat: Git
```git branch d name``` : Delete branch \
```git branch -r``` : List remote branch \
```git branch``` : List local branch

## 🪟 Samba
- Interact with ``smb`` service

```smbclient -L \\\\10.10.10.10\\ -N``` : List samba shares\
```smbclient \\\\10.10.10.10\\share\\ -N``` : Connect to samba share use (``cd``, ``ls`` and ``get`` to interact)

## 🧊 Redis
- Interact with redis keystore

```KEYS *``` : List keys \
```GET``` : Get key value \
```LRANGE key 1 10``` : Get a range of key values (1 to 10)

## :dolphin: MySQL
- Manipulate DB and tables

```SHOW DATABASES;``` : Display all database\
```USE db_name;``` : Enter the desired database\
```SHOW TABLES;``` : Display all the tables of the database

- Manipulate DB through shell

```mysql -u user -p db_name``` : Log in the default root or into the desired database\
```mysqldump -u user -p db_name > file.sql``` : Dump the database content into an output file

## :eye: Nmap

```nping --rate=5 172.16.5.0/16``` : Fast scan entire subnet \
```nc -w1 -i1 $1 22``` : Check if a port is open on remote host with nc 

## :spider_web: Web servers tricks
- DNS

```ls TLD_Extension | awk '{print $0".TLD_Extension"}' | xargs -n 1 host -t A``` : Look for any site in a sub-directory and add the TLD extension. Then perform a ```host``` command and get the server IP address.

## :whale: Docker
- Manipulate containers

```docker run -d -p 5000:5000 --name image_tag tag:version``` : Run container with docker image\
```docker run -it cont_name /bin/bash``` : Runs a command in a new container.\
```docker exec -it cont_name /bin/bash``` : Runs a command in container.

```docker ps -a``` : Show running containers.\
```docker start container_id``` : Starts the stopped containers.\
```docker stop container_id``` : Stops running containers.\
```docker rm container_id``` : Remove container (container must be stopped)

- Manage images

```docker search``` : Searches the Docker Hub for images\
```docker save tag:version | gzip > image.tar.gz``` : Export docker image to ``gz`` archive\
```docker load < image.tar``` : Load image into docker daemon\
```docker tag image_id tag:version``` : Tag builded docker image

```docker images ls``` : Shows all images present in the registry.\
```docker rm image_id``` : Remove image (container must be stopped).

- Build and pull

```docker build -t tag:version - < dockerfile``` : Build and tag image form a **Docker file**.\
```docker build -t tag:version``` : Build and tag image form a **Docker registry**.\
```docker pull``` : Pulls an image or a repository.

- Docker MISC

Docker **daemon** config file located at ``/etc/docker/daemon.log`` (need to restart daemon after editing)\
Docker proxy config file located at ``/etc/systemd/system/docker.service.d/http-proxy.conf``

## 🛠 Ansible

```ansible-playbook pb.yaml --tags "config"

- YAML Syntax

```raw: "sudo echo \"wrld\"``` Escaping ``"`` inside modules

## PM2
```pm2 start npm --name tcportfolio.net -- run develop```
