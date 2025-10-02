# Component

A **Component** is the main unit of composition in CompoMe.
It can:

* Store **data**
* Expose **functions**
* Provide and require **interfaces**
* Emit and receive **events** through **buses**
* Contain **sub-components** and define **connections** between them

All code scaffolding is generated, leaving only the business logic to implement.

---

## Model

| Field                  | Description                                                  | Form   | Opt | Default            | Example                                              |
| ---------------------- | ------------------------------------------------------------ | ------ | --- | ------------------ | ---------------------------------------------------- |
| **NAME**               | The name of the Component                                    | String |     |                    | `C1`, `Struct_s1`, `MyStruct`                        |
| **NAMESPACE**          | The namespace of the Component                               | String | X   | `""`               | `Package1`, `Package1::SubPackage2`, `compo::base`   |
| **PARENT**             | The base class to inherit from                               | String | X   | `CompoMe::Structs` | `Pack1::S1`, `MyStruct`                              |
| **DATA**               | List of data fields (Types)                                  | List   | X   | `Null`             | See examples                                         |
| **FUNCTION**           | List of functions to define                                  | List   | X   | `Null`             | See examples                                         |
| **OPTIONS**            | Options for generation                                       | List   | X   | `Null`             | See [Options](#options)                              |
| **GEN**                | List of generators                                           | List   | X   | `Null`             | See [Generators](#generators)                        |
| **PROVIDE**            | List of interfaces provided by this component                | List   | X   | `Null`             | `["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"]` |
| **REQUIRE**            | List of interfaces required by this component                | List   | X   | `Null`             | `["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"]` |
| **EMITTER**            | List of buses this component can emit                        | List   | X   | `Null`             | `Bus_A`                                              |
| **RECEIVER**           | List of buses this component can receive                     | List   | X   | `Null`             | `Bus_B`                                              |
| **CONNECTION**         | Connections between sub-components and provide/require ports | List   | X   | `Null`             | See [Connections](#connections)                      |
| **COMPONENT_INSTANCE** | List of sub-components                                       | List   | X   | `Null`             | `["C2 cs", "C3 ci"]`                                 |
| **REQUIRE_LIST**       | Special grouped require list                                 | List   | X   | `Null`             |                                                      |

---

## Options

| Field              | Description                 |
| ------------------ | --------------------------- |
| **OPTIONS.SWIG**   | Generate bindings for SWIG  |
| **OPTIONS.STREAM** | Enable stream serialization |

---

## Connections

Connections describe how **provides**, **requires**, **emitter**, and **receiver** ports are linked between sub-components.

| Description           | LEFT              | MID   | RIGHT         |               |            |
| --------------------- | ----------------- | ----- | ------------- | ------------- | ---------- |
| Provide link          | `PROVIDE`         | `     | ->`           | `SC.PROVIDE`  |            |
| Internal link (p → r) | `SC.PROVIDE`      | `-->` | `SC.REQUIRE`  |               |            |
| Require link          | `SC.REQUIRE`      | `>-   | `             | `REQUIRE`     |            |
| Require list link     | `SC.REQUIRE_LIST` | `>+   | `             | `REQUIRE`     |            |
| Back link to provide  | `SC.REQUIRE_LIST` | `+->` | `SC.PROVIDE`  |               |            |
| Event link            | `EMITTER`         | `     | =             | `             | `RECEIVER` |
| Emit to bus           | `SC.EMITTER`      | `>=   | `             | `EMITTER`     |            |
| Bus to receive        | `EMITTER`         | `     | =>`           | `SC.RECEIVER` |            |
| Internal event link   | `SC.EMITTER`      | `=>`  | `SC.RECEIVER` |               |            |

---

# Examples

## Provide/Require Interfaces

```yaml
- INTERFACE:
    NAME: I1
    FUNCTION:
    - void f2()
    - i32 f3(i32 a, i32 b)
    
- INTERFACE:
    NAME: I2
    DATA:
    - i32 d1
    
- COMPONENT:
    NAME: Component_A
    DATA:
      - i32 b
    FUNCTION:
      - i32 f1(i32 p1)
    PROVIDE:
      - I1 p1
    REQUIRE:
      - I2 r1
```

**Usage in C++**

```cpp
Component_A c;

// attributes
c.set_b(1);
c.get_b();

// function call
c.f1(2);

// provide access
c.get_p1().f2();
c.get_p1().f3(1, 2);
```

---

## Bus Emitter/Receiver

```yaml
- EVENT:
    NAME: ev1

- EVENT:
    NAME: ev2

- BUS:
    NAME: Bus_A
    EVENTS:
      - ev1
      
- BUS:
    NAME: Bus_B
    EVENTS:
      - ev2

- COMPONENT:
    NAME: Comp_1
    RECEIVER:
      - Bus_A b_recv
    EMITTER:
      - Bus_B b_emit
```

**Usage in C++**

```cpp
Comp_1 b;

b.configuration();
b.connection();
b.start();

// Create and push the event
ev1 e1;
b.get_b_recv().push(&e1);

// The event will be processed
b.step();

b.stop();
```

---

## SubComponent / Connector Connections

```yaml
- IMPORT: CompoMe.yaml

- INTERFACE:
    NAME: I1
- INTERFACE:
    NAME: I2

- COMPONENT:
    NAME: C2
    PROVIDE:
      - I1 b
    REQUIRE:
      - I2 c
      - I2 c2
    
- COMPONENT:
    NAME: C3
    REQUIRE:
      - I1 e

- COMPONENT:
    NAME: C4
    PROVIDE:
      - I2 e

- COMPONENT:
    NAME: C1
    COMPONENT_INSTANCE:
      - C2 cs
      - C3 ci
      - C4 co
    PROVIDE:
      - I1 a
    REQUIRE:
      - I2 d
    CONNECTION:
      - a |-> cs.b
      - cs.c >-| d
      - ci.e --> cs.b
      - cs.c2 --> co.e
```

<p align="center">
  <img src="https://gitlab.marger.it:10443/ruhtra/compo/-/wikis/Compo/graph/component_ex.png" alt="Component graph example"/>
</p>


👉 Do you want me to also make a **“Cheat Sheet” section** at the end that summarizes *all possible Component constructs* (data, function, provide, require, emitter, receiver, sub-component, connection) in a compact table for quick reference?
