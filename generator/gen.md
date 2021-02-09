

### GLOBAL
| GÉNÉRATEUR | TYPE | STRUCT | EVENT | ERROR | BUS | INTERFACE | DEPLOYMENT | COMPONENT | LINK |
|------------|------|--------|-------|-------|-----|-----------|------------|-----------|------|
| C++        | OK   | OK     | OK    | OK    | OK  | OK        | OK         | OK        | OK   |


### STRUCTURE
| GÉNÉRATEUR | GET/SET/A | CONSTUCTEUR | SERIALIZATION | PARENT | SHELL | SWIG | DBUS |
|------------|-----------|-------------|---------------|--------|-------|------|------|
| C++        | OK        | OK          | OK            | OK     | OK    | OK   | OK   |



### ERROR
| GÉNÉRATEUR | GET/SET | CONSTUCTEUR | SERIALIZATION | DEFAULT | PARENT |
|------------|---------|-------------|---------------|---------|--------|
| C++        | OK      | OK          | OK            | OK      | OK     |


### INTERFACE
| GÉNÉRATEUR | FUNCTION | GET/SET | DEFAULT | CALLER | FAKE | PARENT |
|------------|----------|---------|---------|--------|------|--------|
| C++        | OK       | OK      | OK      | OK     | OK   | OK     |


### SERIALIZATION
| SERIALIZATION | TYPE | STRUCT | ERROR | CALLER | FAKE | POINTER | CONTEXT |
|---------------|------|--------|-------|--------|------|---------|---------|
| C++/STREAM    | OK   | OK     | OK    | OK     | OK   | OK      | PARTIAL |
| C++/DBUS      | OK   | OK     | -     | OK     | OK   | -       | -       |


### COMPONENT
| GÉNÉRATEUR | ACCES INTERFACE | FUNCTION | DATA | INIT | COPY | GET/SET | DEFAULT | PARENT | PROVIDE/REQUIRED |
|------------|-----------------|----------|------|------|------|---------|---------|--------|------------------|
| C++        | OK              | OK       | OK   | OK   | OK   | OK      | OK      | OK     | OK               |

### SUBCOMPONENT
| GÉNÉRATEUR | SUBCOMPONENT | CONNECTION C2SC | CONNECTION SC2C | CONNECTION SC2SC | STEP |
|------------|--------------|-----------------|-----------------|------------------|------|
| C++        | OK           | OK              | OK              | OK               | OK   |


### DEPLOIMENT
| GÉNÉRATEUR | DEPLOIMENT | Instance | INSTALLATION LINK | default | CONNECTION |
|------------|------------|----------|-------------------|---------|------------|
| C++        | OK         | OK       | OK                | OK      | OK         |


### LINK
| GÉNÉRATEUR | LINK |
|------------|------|
| C++        | OK   |


### COMPILATION
| GÉNÉRATEUR     | COMPILATION | CMAKE FULL | CMAKE COMPOSANT | CMAKE RUN |
|----------------|-------------|------------|-----------------|-----------|
| C++ CMAKE MAKE | OK          | OK         | OK              | OK        |

### RUN
| GÉNÉRATEUR | RUN | MEMORY | DEBUG | UNIT TEST | SHELL |
|------------|-----|--------|-------|-----------|-------|
| C++        | OK  | OK     | OK    | ...       | ...   |

### LINK
| GÉNÉRATEUR | IN | OUT |
|------------|----|-----|
| C++ UDP    | OK | OK  |
| C++ TCP    | OK | OK  |
| C++ DIRECT | OK | OK  |
| C++ ZMQ    | OK | OK  |
| C++ DBUS   | OK | OK  |
