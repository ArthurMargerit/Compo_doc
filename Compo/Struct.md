A Struct is a simple way to define a C++ class composed of other Structs or CompoMe types.
CompoMe provides:

- Constructor/Destructor
- Functions
- Data with set/get/access methods
- Builder pattern
- Serialization to std::stringstream
- Serialization to Dbus
- UML rendering


# Model

| Field     | Description                          | Format         | Opt | Default             | Example                          |
|-----------|--------------------------------------|----------------|-----|---------------------|----------------------------------|
| NAME      | The name of the Struct               | String         |     |                     | S1, Struct_s1, MyStruct          |
| NAMESPACE | The namespace of the Struct          | String         | X   | ""                  | Package1, Package1::SubPackage2, compo::base |
| PARENT    | The parent class to inherit from     | String         | X   | "CompoMe::Structs"  | Pack1::S1, MyStruct              |
| DATA      | List of Struct or CompoMe data types | List<String>   | X   | Null                | See example section             |
| FUNCTION  | List of functions to define         | List<String>   | X   | Null                | See example section             |
| EXTRA     | Add extra serialization string       | Boolean        | X   | False               | True/False                       |
| HIDE      | List of data fields to hide         | List<String>   | X   | Null                | See example section             |
| GEN       | List of function generators         | List<String>   | X   | Null                | See cpp/2.STRUCT/3.gen          |


# Example

### Basic Empty Struct
```yaml
- STRUCT:
    NAME: S1
```

### Simple Struct with 2 Data Fields
```yaml
- STRUCT:
    NAME: S2
    DATA:
     - i32 a
     - i32 b
```

### Simple Struct with 2 Functions
```yaml
- STRUCT:
    NAME: S3
    FUNCTION:
     - i32 add(i32 p1, i32 p2)
     - i32 sub(i32 p1, i32 p2)
```

### Two Structs, One Using the Other
```yaml
- STRUCT:
    NAME: S4
    DATA:
     - i32 a
- STRUCT:
    NAME: S5
    DATA:
     - S4 sub_struct
     - i32 b
```

### Three Structs with Parent-Child Relationship
```yaml
# Mother class
- STRUCT:
    NAME: S_mother
    DATA:
     - i32 v1_mo

# Child of S_mother
- STRUCT:
    NAME: S_child_1
    PARENT: S_mother
    DATA:
     - i32 v2_ch

# Child of S_mother
- STRUCT:
    NAME: S_child_2
    PARENT: S_mother
    DATA:
     - i32 v3_ch
```


# In C++

# How To Use a CompoMe Struct
To a use a CompoMe::Struct you need to include it.

## Inheritance
All the types extend `CompoMe::Struct`.
You can address them as a `CompoMe::Struct* l_struct_pointer;`.

---

## Files
For a Struct, you can find several files.
Replace `"NS"` with the namespace of the struct (with `"::"` replaced by `"/"`).
For example: `"pkg1"` becomes `"pkg1"`, and `"pkg1::pkg2::pkg3"` becomes `"pkg1/pkg2/pkg3"`.
Replace `"NAME"` with the name of the Struct.

### Include Files
| File Path                                      | Description                                      |
|------------------------------------------------|--------------------------------------------------|
| `inc/Structs/NS/NAME.hpp`                      | Definition of the class `NAME`                  |
| `inc/Structs/NS/NAME_builder.hpp`             | Builder of the class `NAME`                      |
| `inc/Structs/NS/NAME_fac.hpp`                 | System to build from stream the pointer of `NAME`|

### Source Files
| File Path                                      | Description                                      | Frequency of Edit |
|------------------------------------------------|--------------------------------------------------|-------------------|
| `src/Structs/NS/NAME.cpp`                      | Constructor and Destructor of the class          | Frequently        |
| `src/Structs/NS/NAME_function.cpp`             | Function definition                              | Frequently        |
| `src/Structs/NS/NAME_fac.cpp`                  |                                                  | Rarely            |
| `src/Structs/NS/NAME_builder.cpp`              |                                                  | Rarely            |
| `src/Structs/NS/NAME_serialization.cpp`        | Serialization of the stream                      | Rarely            |
| `src/Structs/NS/NAME_get_set.cpp`              | The get/set/accessor of the class                 | Rarely            |

### Include it
All the type will be put in Structs folder. if you define a namespace the type will be fine in the namespace folder. For example to use _S1_ include "Structs/S1.hpp" for _Prj1::myType_ include "Structs/Prj1/myType.hpp".
```c++
#include "Structs/S1.hpp"
#include "Structs/Prj1/MyType.hpp"
```

### Define and use it
Just use it as a standard C++ class.
`S1 l_v = S1();`
`Prj1:::MyType l_v;`

#### get/set/accs
All the data can be acess/ get(copy)/ and set.
```c++
S2 l_v;
// get (copy)
i32 v1 = l_v.get_a();
// set
l_v.set_a(1);

// acessor
l_v.a_a() = 1;
i32 v2 = l_v.a_a();
```

#### function
The function can be call on the object like that.
```c++
S3 l_v;
i32 v1 = l_v.add(1,3);
i32 v2 = l_v.sub(5,3);
```

#### stream
A serialization
```c++
std::stringstream ss;
S3 l_v,l_v2;
ss << l_v;
std::cout << ss.str(); // print it
ss >> l_v2;
```


# USEFUL INFORMATION

## More Example
To get more Information and example of TYPE  you can read all the "code.yaml" file  place in "./Test/gen/cpp/2.STRUCT" and the "main.cpp" file.

## Swig Utilisation
Refer to the swig documentation


# FAQ
## Where can i write the code ?
You have to define the code of the function in "src/Structs/STRUCT_function.cpp" or "src/Structs/NAMESPACE/STRUCT_function.cpp"
All the function are already define, you just need to write the core of the function.
