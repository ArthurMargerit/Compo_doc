# Type

A **Type** is the system used to define the atomic elements that can be exchanged between components.

---

## Definition

| Field          | Description                                                    | Form        | Opt | Default | Example                                            |
| -------------- | -------------------------------------------------------------- | ----------- | --- | ------- | -------------------------------------------------- |
| **NAME**       | The name of the Type                                           | String      |     |         | `Type1`, `i32`, `myType`                           |
| **NAMESPACE**  | The namespace of the Type                                      | String      | X   | `""`    | `Package1`, `Package1::SubPackage2`, `compo::base` |
| **DEFINITION** | A C/C++/language definition of the Type                        | String      |     |         | `int`, `long`, `float`, `unsigned int`             |
| **NATIF**      | Indicates whether the type is native (no include required)     | Boolean     | X   | `false` | `true` / `false`                                   |
| **BEFORE**     | Code added before the type definition                          | String      | X   | `Null`  | `"class Test;"`, `"#define Nop=1"`                 |
| **AFTER**      | Code added after the type definition                           | String      | X   | `Null`  | `"#undef Nop"`, `"warning 'deprecated'"`           |
| **INCLUDE**    | Files to include                                               | List/String | X   | `Null`  | `"<cstdint>"`, `["<vector>", "\"Types/i32.hpp\""]` |
| **DYNAMIC**    | Marks the type as a template                                   | Boolean     | X   | `false` | `true` / `false`                                   |
| **ARG**        | Template parameters (classes, namespaces, types, constants, …) | List        | X   | `Null`  | `["Type", "unsigned long int Size"]`               |
| **TOSTRING**   | Generate `<<` and `>>` operators                               | Boolean     | X   | `false` | `true` / `false`                                   |

---

## Examples

### A basic native Type

Can be used directly because the C/C++ compiler does not require any include for `int`.

```yaml
- TYPE:
    NAME: int
    NATIF: true
```

---

### A `cstdint` Type

Not native, since it requires `<cstdint>` and a new alias.

```yaml
- TYPE:
    NAME: i32
    DEFINITION: std::int32_t
    INCLUDE: '<cstdint>'
```

---

### A defined vector of `i32`

Here, `VecOfi32` is **not** dynamic since all template arguments are fixed in the definition.

```yaml
- TYPE:
    NAME: VecOfi32
    DEFINITION: std::vector<i32>
    INCLUDE:
      - "<vector>"
      - '"Types/i32.hpp"'
```

---

### A dynamic 2D vector of unknown Type

Here, `TYPE` is a placeholder template argument.

```yaml
- TYPE:
    NAME: Vec2dOfT
    DEFINITION: std::vector<std::vector<TYPE>>
    INCLUDE:
      - "<vector>"
    ARG:
      - TYPE
    DYNAMIC: true
```

---

## How to Use a CompoMe Type

### Include it

All types are generated in the `Types` folder.

* Without namespace: include `"Types/i32.hpp"`
* With namespace: for `Test::Prj1::myType` include `"Types/Test/Prj1/myType.hpp"`

### Define and use it

Simply use it as a standard C++ type:

```cpp
i32 a = 1; 
i32 b; 
VecOfi32 v = {1,2,3,4,5,6}; 
Vec2dOfT<i32> v2;
```

---

## Useful Information

* If you want to reuse all predefined CompoMe Types, include:

```cpp
#include "CompoMe.yaml"
```

* This provides:

  * All **cstdint types** (`i8`, `i16`, `i32`, `i64`, `ui8`, `ui16`, `ui32`, `ui64`)
  * **Native types** (`bool`, `double`, `float`, …)
  * Some **optional types** (`CompoMe::String`, …)
  * Standard C++ containers (`array`, `map`, `vector`, `tuple`, `pair`)

👉 In a simple project, you usually don’t need to define new Types.

---

## More Examples

For more information, see:

* `./Test/gen/cpp/1.TYPE/code.yaml`
* `main.cpp`

