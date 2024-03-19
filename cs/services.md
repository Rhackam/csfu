# Services Cheatsheet

## :octocat: Git

```git branch d name``` : Delete branch \
```git branch -r``` : List remote branch \
```git branch``` : List local branch

## 📫 POP3

```telnet pop.gmx.net 110``` : Connect to POP3 server on port ```110``` \
```USER user``` : Ask for username \
```PASS user``` : Ask for password \
```LIST``` : List mails \
```RETR 1``` : Read mails 

## ⚓ Kubernetes

```kubeadm init``` : Initialize kubernetes cluster

- Images
  
```kubectl config images pull --image-repository``` : Specify repository where to pull images

- Ressources Query
  
```kubectl get pods -A -o wide``` : List all pods in any namespaces \
```kubectl get services -A``` : List all services in any namespaces 

- Ressources Manipulation
  
```kubectl exec pod_name -it -- /bin/sh```: Pop into a pods and get a shell \
```kubectl delete ressource_type ressource_name``` : Delete cluster ressource (pods, deployment, secret, service, ...) \
```kubectl scale deployment dep_name --replicas=1``` : Scale down or up replicas for given pod \
```kubectl rollout undo deployment/app --to-revision=2``` : Rollout deployment \
```kubectl logs -f pod_name``` : Get stream log for given pod \

```crictl --runtime-endpoint /var/run/containerd/containerd.sock ps```

## 🪟 Samba

- Interact with ``smb`` service

```smbclient -L \\\\10.10.10.10\\ -N``` : List samba shares \
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

## 🐘 Hadoop

```hdfs fsck / | egrep -v '^\.+ | grep -v replica | grep -v Replica``` : Check health of hdfs cluster

## 🛠 Ansible

```ansible-playbook pb.yaml --tags "config"``` : Excute step from config tag

- Vault

```ansible-vault create vars/secure.yaml``` : Create simple ansible vault \
```ansible-vault edit vars/secure.yaml``` : Edit vault without clear decrypt it \
```ansible-vault rekey vars/secure.yaml``` : Change vault password \
```ansible-vault encrypt vars/secure.yaml``` : Encrypt file into a vault \
```ansible-vault decrypt vars/secure.yaml``` : Decrypt ansible vault \
```ansible-vault view vars/secure.yaml``` : Show vault content

- YAML Syntax

```raw: "sudo echo \"wrld\"``` Escaping ``"`` inside modules

## PM2

```pm2 start npm --name tcportfolio.net -- run develop```

## Vim Combo

```.,$d``` : \
```:g/abc/cba``` : Find and replace

## UNIX Keyboards Combo

<kbd>Ctrl</kbd> + <kbd>A</kbd> : Go to start of prompt \
<kbd>Ctrl</kbd> + <kbd>E</kbd> : Go to end of prompt \
<kbd>Ctrl</kbd> + <kbd>R</kbd> : Search into shell history \
<kbd>Ctrl</kbd> + <kbd>O</kbd> : Execute searched command \
<kbd>Ctrl</kbd> + <kbd>U</kbd> : Delete line before cursor \
<kbd>Ctrl</kbd> + <kbd>K</kbd> : Delete line after cursor \
<kbd>Ctrl</kbd> + <kbd>W</kbd> : Delete previous word \
<kbd>Alt</kbd> + <kbd>B</kbd> : Backward one word

## Windows Keyboard Combo
<kbd>Win</kbd> + <kbd>Shift</kbd> + <kbd>&larr;</kbd>/<kbd>&rarr;</kbd> : Move window to left or right screen \
<kbd>Alt</kbd> + <kbd>Tab</kbd> : Switch between windows \
<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>Pause</kbd> : Toggle windows RDP session

## VSCode Keyboads Combo

<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>E</kbd> : Execute hilighted text \
<kbd>Ctrl</kbd> + <kbd>T</kbd> : Toggle terminal
