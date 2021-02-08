CompoMe is a Component Framework.


First Step:
======

Clone repository
----
```bash
git clone --recurse-submodules https://gitlab.marger.it:10443/ruhtra/compo
cd compo
```

Install dependencies
-----

## Linux Systems:
```bash
# DEBIAN TODO
sudo apt intall $(cat tool/dep_deb.txt) 

# ARCH TODO
sudo pacman -S $(cat tool/dep_deb.txt)

# guix environment -m tool/guix_manifest.scm
```
Python dep:
```bash
pip install -r tool/env.txt
```

Build CompoMe C++ core:
```bash
cd test/gen/
./core_build.sh
```

Run Test Check (Optional):
```bash
./run_test.sh cpp
./run_test.sh graph
./run_test.sh uml
```
## Docker Solution (Windows/Linux/MacOS)


Download Image: (image)[https://marger.it/CompoMe_Docker_img.tar.gz].
How to ReBuild this image (__TODO__).

```bash
# load it
docker load -i ....
# run the docker image 
# replace IMG by the IMAGE_ID 
# replace PORT by a port wherre you want to bind ssh port of container
# replace COMPO_DIR by the compo clone directory
docker run -d --name Compo --hostname CompoMeImg --privileged -v COMPO_DIR:/home/compozer/compo  -p PORT:22 -it IMG
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

```bash
docker exec -it Compo bash
```

In the docker terminal
```bash
# add your ssh-key key.pub
cat >> /etc/ssh/authorized_keys.d/compozer
<paste-it><ctrl-d>
# exit the docker terminal
```

```bash
# login with ssh
ssh compozer@127.0.0.1 -p PORT -i key
```

TUTORIAL
----------
...TODO...

EXAMPLE
---------
Simple:
- [Example/HelloWord]()
- [Example/Serialization]()
- [Example/Interface]()
- [Example/Car]()

Link:
- [Example/Http_Server]()
- [Example/Https_Server]()
- [Example/Dbus_Client]()
- [Example/Dbus_Server]()

Extra:
- [Example/SwigWithMe]()

MODEL
----------
CompoMe manage this elements:
- [Type](Compo/Type)
- [Struct](Compo/Struct)
- [Error](Compo/Error)
- [Event](Compo/Event)
- [Bus](Compo/Bus)
- [Interface](Compo/Interface)
- [Component](Compo/Component)
- [Connector](Compo/Connector)
- [Link](Compo/Link)
- [Deployment](Compo/Deployment)

GENERATOR
----------
CompoMe Have 4 kind of generator:
- [C++](generator/Cpp)
- [Uml](generator/Uml)
- [Graph](generator/Graph)
- [Python](generator/Python)

FAQ
---

Can i add my own Generator or modified the existing one ?
...TODO...

I find a bug/troubleshooting, Where can i find a solution or submit it?
...TODO...

... ?
...TODO...

Troubleshooting
----------

...TODO...

