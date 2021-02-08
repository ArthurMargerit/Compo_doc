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
Linux Systems:
```bash
# DEBIAN TODO
sudo apt intall $(cat tool/dep_deb.txt) 

# ARCH TODO
sudo pacman -S $(cat tool/dep_deb.txt)
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

