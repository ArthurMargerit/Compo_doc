
A Struct is a simple way to define a c++ class withch is compose of other Struct or CompoMe TYPE.

CompoMe provide:
- Constructor/Destructor
- Function
- Data with set/get/acc
- Builder 
- Serialisation to std::stringstream
- Serialisation to Dbus

- UML Rendering

# Model

| Field     | Description                      | Form         | Opt | Def                | Example                                       |
|-----------|----------------------------------|--------------|-----|--------------------|-----------------------------------------------|
| NAME      | The name of the Struct           | String       |     |                    | S1, Struct_s1, MyStruct                       |
| NAMESPACE | The namespace of the Struct      | String       | X   | ""                 | Package1, Package1::SubPackage2 , compo::base |
| PARENT    | The class that you inherit       | String       | X   | "CompoMe::Structs" | Pack1::S1, MyStruct                           |
| DATA      | list of Type Struct              | List<String> | X   | Null               | go to example section                         |
| FUNCTION  | a list of function to define     | List<String> | X   | Null               | go to example section                         |
| EXTRA     | add a extra serialisation string | Boolean      | X   | False              | True/False                                    |
| HIDE      | a list of data to hide           | List<String> | X   | Null               | go to example section                         |
| GEN       | a list of function generator     | List<String> | X   | Null               | Go to cpp/2.STRUCT/3.gen                      |


# Generator options
.TODO.
SWIG.
DBUS.
STREAM.

# Example

A Basic empty Struct:
```yaml
- STRUCT:
    NAME: S1
```

This a simple Struct with 2 data a and b
```yaml
- STRUCT:
    NAME: S2
    DATA:
     - i32 a
     - i32 b
```

This a simple Struct with 2 Functions "add" and "sub"
```yaml
- STRUCT:
    NAME: S3
    FUNCTION:
     - i32 add(i32 p1, i32 p2)
     - i32 sub(i32 p1, i32 p2)
```

This 2 Structs with 1 which use the other
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


This 3 structs with a simple parent link
```yaml
# mother class
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

To use i32 TYPE: you need to include CompoMe.yaml as follow `- IMPORT: "CompoMe.yaml"` or to redefined it.
-------



# How To Use a CompoMe Struct
To a use a CompoMe::Struct you need to include it.



## In C++
All the Type extends CompoMe::Struct;
Then you can address them as a `CompoMe::Struct* l_struct_pointer;`

## Files
you can find for a Struct many File:
replace "NS" with the namespace of the struct and  "::" replaced by "/"  eg: "pkg1"=>"pkg1", "pkg1::pkg2::pkg3"=>"pkg1/pkg2/pkg3"
replace "NAME" with the name of the STRUCT

Include files:
-----------------
"inc/Structs/NS/NAME.hpp": Definition of the class NAME
"inc/Structs/NS/NAME\_builder.hpp": Builder of the class NAME
"inc/Structs/NS/NAME\_fac.hpp": System to build from stream the pointer of NAME

Source files:
-----------------

What you will edit frequently this files:
"src/Structs/NS/NAME.cpp": Contructor and Destructor of the class
"src/Structs/NS/NAME\_function.cpp": Function definition

What you will never or rarely edit this file:
"src/Structs/NS/NAME\_fac.cpp": 
"src/Structs/NS/NAME\_builder.cpp": 
"src/Structs/NS/NAME\_serialization.cpp": Serialization of the stream
"src/Structs/NS/NAME\_get\_set.cpp": The get/set/accesor of the class

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
