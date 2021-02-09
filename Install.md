Install dependencies
-----------------------

# On Standard Linux System

Take a look at tool/dep_<YourDistro>.txt and run the install process.

- Debian
```bash
sudo apt intall $(cat tool/dep_deb.txt) 
```

- Arch
```bash
sudo pacman -S $(cat tool/dep_arch.txt)
```

# On System With Docker (Windows,Linux, ...)
Download Image: [Here](https://marger.it/CompoMe_Docker_img.tar.gz).
How to ReBuild this image: [Guix Docker Build](Guix_Docker_build).

In the next Command you need to:
- replace IMG by the IMAGE_ID (use `docker images` after loading )
- replace PORT by a port wherre you want to bind ssh port of container (map it to a unused port ex: 8022)

```bash
# load it
docker load -i ....

# run it
docker run -d --name Compo --hostname CompoMeImg --privileged -p PORT:22 -it IMG
```

### Login
Two Solutions
-------------

#### Solution 1: docker exec acess
```bash
docker exec -it -u compozer Compo bash
```

#### Solution 2: ssh acess (no way to log with PasswordAuthentication only with PubkeyAuthentication)
Generate a ssh key.
```bash
ssh-keygen -N "" -f key
```
You can notice that 2 file "key" and "key.pub" have been created.

```bash
docker exec -it Compo bash
```

In the docker terminal
```bash
# add your ssh-key key.pub
cat >> /etc/ssh/authorized_keys.d/compozer
<paste-key.pub-it><ctrl-d>
# exit the docker terminal
```

Now, you can use ssh to connect to the docker image. 

```bash
# login with ssh
ssh compozer@127.0.0.1 -p PORT -i key
```

