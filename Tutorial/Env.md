
Avant de commencé cette section il est n'écesaire d'avoir un Environement de compilation, et d'avoir compilé le core de CompoMe C++.


Crée un dosier.
```bash
mkdir -p projects/Compo_Sound
```


Nous allons commencé par definir les variables d'environement.
Les deux principales sont:
- COMPOME_PATH: Cette variable indique la position du repository eg /home/lapin/projects/compo
- COMPOME_MODEL_PATH: list of path separate by ":" to all .yaml file of other projects

```bash
export COMPOME_PATH=~/compo
export COMPOME_MODEL_PATH=$(echo ${COMPOME_PATH}/build/CompoMe* | sed "s/ /:/g")
```

