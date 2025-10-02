
# Connector

A **Connector** is similar to a component, but without any *user code*.
All of its code is generated from a template. You can easily add new templates to the system.

This mechanism helps the **Software Architect** save time by avoiding writing obvious or repetitive code.

---

## Definition

| Field         | Description                                        | Form   | Opt | Default | Example                     |
| ------------- | -------------------------------------------------- | ------ | --- | ------- | --------------------------- |
| **NAME**      | The name of the connector                          | String |     |         | `Con_1`                     |
| **NAMESPACE** | Namespace of the connector                         | String | X   | `""`    | `ns`, `ns1::ns2`            |
| **MODEL**     | List of files for code generation                  | Dict   | X   | `None`  |                             |
| MODEL.CPP     | Template for C++ source                            | String | X   |         | `My_template.cpp`           |
| MODEL.HPP     | Template for C++ header                            | String | X   |         | `My_template.hpp`           |
| **PROVIDE**   | Equivalent to *component provide*                  | List   | X   | `[]`    |                             |
| **REQUIRE**   | Equivalent to *component require*                  | List   | X   | `[]`    |                             |
| **DATA**      | Equivalent to *component data*                     | List   | X   | `[]`    |                             |
| **OTHERVAR**  | Any custom variable you want available in template | Any    | X   | `None`  | `"v1"`, `"flop"`, `"Lapin"` |
| **GEN**       | Generate all fields for one kind of connector      | String | X   | `None`  | `logger(I1)`                |

---

# Examples

## Logger

All calls made through this interface will be logged.
The same process applies to return values.
Additionally, the time between a call and its result is measured and reported.

```yaml
- INTERFACE:
    NAME: I1
    ...
    
# This connector is a logger for the interface
- CONNECTOR:
    NAME: I1_logger
    INTERFACE: I1
    PROVIDE:
      - I1 p
    REQUIRE:
      - I1 r
      - CompoMe::Log::Log_I log
    MODEL:
      CPP: Logger.cpp
      HPP: Logger.hpp
      
# This connector is equivalent to I1_logger
- CONNECTOR:
    NAME: I1_logger2
    GEN: logger(I1)
```

---

## Async

Exposes an interface to an **asynchronous system**.
For an interface `I1`, two interfaces are generated:

* One for **calls**
* One for **returns**

The async connector receives a call on `Interface_call`, performs the action, and then calls back with the result on `Interface_return`.

```yaml
- INTERFACE:
    NAME: I1
    ...
    
- INTERFACE:
    NAME: I1_async_call
    GEN: async_call(I1)

- INTERFACE:
    NAME: I1_async_return
    GEN: async_return(I1)

- CONNECTOR:
    NAME: I1_async_simple
    INTERFACE: I1
    INTERFACE_ASYNC_CALL: I1_async_call
    INTERFACE_ASYNC_RETURN: I1_async_return
    MODEL:
      CPP: Async.cpp
      HPP: Async.hpp
    PROVIDE:
      - I1_async_call c
    REQUIRE:
      - I1 r
      - I1_async_return rr
```

---

## Dispatcher

A **Dispatcher** sends calls to a list of interfaces.

```yaml
- INTERFACE:
    NAME: I1

- CONNECTOR:
    NAME: Disp_1
    INTERFACE: I1
    MODEL:
      CPP: Dispatcher.cpp
      HPP: Dispatcher.hpp

- CONNECTOR:
    NAME: Disp_1_eq
    GEN: dispatcher(I1)
```

---

## Distributor

In many cases, you may want to distribute a call and collect results:

* Call to a specific interface `X` → single result
* Call to many interfaces with the same parameters → multiple results
* Call to many interfaces with a vector of parameters → multiple results

```yaml
- INTERFACE:
    NAME: I1

- INTERFACE:
    NAME: I1_many_return
    GEN: many_return(I1,Vec)
    
- INTERFACE:
    NAME: I1_many
    GEN: many(I1,Vec)
    
- INTERFACE:
    NAME: I1_by_id
    GEN: add_params(I1,i32)

- CONNECTOR:
    NAME: Math_many_return_dist
    INTERFACE: I1
    INTERFACE_MANY_RETURN: I1_many_return
    MODEL:
      CPP: Distri_many_return.cpp
      HPP: Distri_many_return.hpp

- CONNECTOR:
    NAME: I1_many_dist
    INTERFACE: I1
    INTERFACE_MANY: I1_many
    MODEL:
      CPP: Distri_many.cpp
      HPP: Distri_many.hpp

- CONNECTOR:
    NAME: I1_by_id_dist
    INTERFACE: I1
    INTERFACE_WITH_ID: I1_by_id
    MODEL:
      CPP: Distri_by_id.cpp
      HPP: Distri_by_id.hpp

- CONNECTOR:
    NAME: I1_all_dist
    INTERFACE: I1
    INTERFACE_WITH_ID: I1_by_id
    INTERFACE_MANY: I1_many
    INTERFACE_MANY_RETURN: I1_many_return
    MODEL:
      CPP: Distri_all.cpp
      HPP: Distri_all.hpp
```
