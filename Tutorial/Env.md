Before starting this section, you need to have a working **build environment** and have already compiled the **CompoMe C++ core**.

First, create a project directory:

```bash
mkdir -p projects/Compo_Sound
```

Next, let’s set up the required environment variables.
The two main ones are:

* **COMPOME_PATH**: Path to the CompoMe repository (e.g., `/home/lapin/projects/compo`)
* **COMPOME_MODEL_PATH**: A list of paths (separated by `:`) to all `.yaml` model files from other projects

```bash
export COMPOME_PATH=~/compo
export COMPOME_MODEL_PATH=$(echo ${COMPOME_PATH}/build/CompoMe* | sed "s/ /:/g")
```

