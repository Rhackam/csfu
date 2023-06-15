## :shell: Pure Shell
### 📚 Compress file

```tar -czf archive_name.tar.gz files``` : Compress file with tar \
```tar -xzf archive_name.tar.gz files``` : Decompress file with tar \
```du -sh /var/log/* | grep 'G' | xargs gzip``` : Piping file to compress into xargs

### 🔤 Char

```sed -i '/^$/d'``` : Delete blank line\
```sed -i 's/, [A-Za-z0-9:., ]*//'``` : Delete string after ``,``\
```sed -i 's/[A-Z]/\L&/g'``` : Lowercase strings with ``sed`` \
```sed -i 's/[a-z]/\U&/g'``` : Uppercase strings with ``sed`` \
```sed -i 's/_.*$//'``` : Delete any strings passed between ``\`` and ``.`` \
```grep 'golang-1.15\|yarn'``` : search for one or other patern \
```(\b25[0-5]|\b2[0-4][09]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}``` : IP Address regex

### 🗓 Date

```date``` : Display Timezone date\
```date -u``` : Display GMT date \
```date +"%Y-%m-%H"``` : Display formated date

```unlink /etc/locatime``` : Remove preset system Timezone\
```ln -s /usr/share/zoneinfo/Etc/GMT-2 /etc/localtime``` : Change Timezone to Paris GMT+2

### 🗃 File systems

```pvcreate /dev/sda1``` : Create a partition on a physical disk \
```pvs``` : Display physical volumes

```fdisk /dev/sda``` : Enter the partition editor and create new partition \
```partx -a -v /dev/sdb``` : Update the partition table of the disk

```vgcreate vg_name /dev/sda1 /dev/sda2``` : Create a volume group by adding two different disk partitions \
```vgextend vg_name /dev/sda2``` : Extend volume groupe (require new disk partition) \
```vgremove vg_name``` : Delete volume groupe \
```vgreduce vg_name /dev/sda1``` : Reduce volume groupe disk use \
```vgs``` : Display volumes group

```lvcreate -L 40G -n lv_name vg_name``` : Create logical voulme on a volume group \
```lvextend -L +1G -r /dev/mapper/appvg_lvlogs``` : Extend logical volume \
```lvs``` : Display Logical volumes

```mkfs.ext4 /dev/mapper/lv_name``` : create a file system on a logical volume (replace ```ext4``` by your choosen FS type)

```df -h``` : Display file system (human readable) \
```lsblk``` : List storage devices tree \
```du -sh /folder/*``` : Display folder size (human readable)

```echo 1 > /sys/class/block/sdd/device/rescan``` : Rescan volumes for existing disk \
```echo "- - -" > /sys/class/scsi_host/host0/scan``` : Rescan devices list to update new disk

### 📡 Network

```ip -s -s neigh flush all``` : Flush arp table (sudo)\
```for i in $(cat host); do ping -W 1 -c 1 $i 1>/dev/null 2>&1; echo $i $?; done``` : Online script to test if host is up \
```for i in $(cat host); do ping -c 1 $i | grep -Eo -m 1 '(\b25[0-5]|\b2[0-4][09]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}'; done```

### 🗺 Search and Path

```ln -s /path/to/link/ /link/name/```

```dpkg -l | grep -c '^ii'``` : Count packages listed by ``dpkg``

```cat file | wc -l``` : Count line in file

```find -type [fcdl]``` : Search for file type ``f`` regular file, ``c`` charcter, ``d`` directory, ``l`` symbolic link \
```find -name tute``` : Search for anything matching tute \
```find -size +20k -size -50k``` : Search for anything matching a size between 20KiB and 50BiB \
```find -newer[am at cm ct mm mt] 2022-15-05``` \
```find -atime +1``` : Search for files last accessed (read) for 2 day \
```find -name tute -exec ls -lt {} +``` : Use exec and ls to list files
 
### 📦 Packet Manager (RHEL/CentOS)

```yum list installed``` : List installed packets \
```yum list available``` : List packet ready to be installed \
```yum repolist``` : List remote packet repositories \
```yum -y install``` : Install packet \
```subscription-manager repos --enable=repos_name``` : Activation d'un repository yum \
```subscription-manager repos --disable=repos_name``` : Désactivation d'un repository yum

### ⏳ Processus

```ps -ef``` : Display all system process

```ipcs -s``` : Display semaphores ipc V \
```ipcs -m``` : Display shared memory ipc V \
```ipcs -l``` : Display all ipc V informations

```ipcrm -m shmid``` : Remove shared memory ipc V \
```ipcrm -s semid``` : Remove semaphore ipc V \
```ipcs -s | grep procname | awk '{printf( "-s %s ", $2 )}' | xargs ipcrm``` : Clean semaphore for a process (``-m`` for shared memory)

### 🔒 Security

```firewall-cmd --get-zones``` : Display configured zones\
```firewall-cmd --list-all --zone=public``` : Display `public` zone paramerters\
```firewall-cmd --zone=public --permanent --add-service=https``` : Whitelist `https` service for `public` zone\
```pam_tally2 --reset --user tute``` : Unlock ssh UNIX account

### 📥 Send or Download files

```rsync -avzP files user@hostname:/path``` : Send file to remote host with rsync\
```rsync -avzP user@hostname:/files /path``` : Download file from remote host with rsync

### 🚛 Services

```systemctl start|stop|status|restart|reload procname``` : Manage processus run with ``systemctl`` \
```service procname start|stop|status|restart|reload``` : Manage processus run with ``service`` \
```/etc/init.d/rocname.service start|stop|status|restart|reload``` : Manage processus run with ``init.d`` \
```systemctl list-units --type=service``` : List all services in systemd

```chkconfig --add oracle``` : Add the /etc/init.d oracle service to the list of /etc/rc.d/ for status managment \
```chkconfig oracle on``` : Enable start on boot for oracle service \
```chkconfig oracle off``` : Turning off start on boot for oracle service \
```chkconfig --level 5 oracle``` : Set /etc/rc.d/* level to enable service managment

### 🚇 SSH

```ssh-keygen``` : Generate a pair of ssh private and public keys\
```ssh -i /path user@hostname``` : Use private ssh key to authenticate\
```echo -n hello; ssh -q -o "ConnectTimeout=1" user@host``` : Execute remote command trought ssh\
```scp -r files user@hostname:/path``` : Send file to remote host with scp\
```scp -r user@hostname:/file /path``` : Download file form remote host with scp\
```ssh -L 8081:host.name:80 admin@host.name``` : Create SSH tunnel and binding local ``8081`` port to remote ``80`` port

### 🥥 System Informations

```cat /sys/class/dmi/id/product_name``` : Get the information about whether a host is virtual or physical\

### 📟 Terminal

```echo "$SHELL"``` : Check wich shell you're using \
```history -d 152``` : Delete history entry

### 👱‍♂️ Users

```chage -d 0 user``` : set password expiration to next loggin
```chage -M 30 user``` : set password expiration to 30 days
