A Type is the system to define the atomic element that can be exchange over the component

Definition:

# Model
| Field      | Description                                       | Form                   | Opt | Def   | Example                                       |
|------------|---------------------------------------------------|------------------------|-----|-------|-----------------------------------------------|
| NAME       | The name of the Type                              | String                 |     |       | Type1, i32, myType                            |
| NAMESPACE  | The namespace of the type                         | String                 | X   | ""    | Package1, Package1::SubPackage2 , compo::base |
| DEFINITION | a C/C++/ language definition                      | String                 |     |       | int, long, float, unsigned int                |
| NATIF      | Inform the generator if the type is natif         | Boolean                | X   | false | true/false                                    |
| BEFORE     | add Code before the definition                    | String                 | X   | Null  | "class Test;", "#define Nop=1"                |
| AFTER      | add Code after the  definition                    |                        |     |       | "#undef Nop", "warning 'deprecated'"          |
| INCLUDE    | add file to include                               | List<String> or String | X   | Null  | ['<stdint>','"test.hpp"'], "test.hpp"         |
| DYNAMIC    | Inform the generator that this type is a template | Boolean                | X   | False | true/false                                    |
| ARG        | All the class/namespace/... of the template       | List<String>           | X   | Null  | ["Type", "unsigned long int Size"]            |
| TOSTRING   | If you want to generate the operator << and >>    | Boolean                | X   | False | true/false                                    |

# Generator options
.TODO.
SWIG.
DBUS.
STREAM.

# Example

A Basic natif Type:
Natif Can Be used in this situation because the C/C++ compiler doesn't need any include to use "int".
```yaml
- TYPE:
    NAME: int
    NATIF: true
```


A cstdint Type:
This one is not natif due to the redefinition of the name, and the necessity of cstdint
```yaml
- TYPE:
    NAME: i32
    DEFINITION: std::int32_t
    INCLUDE: '<cstdint>'
```

A defined vector of i32:
In this situation VecOfi32 is not a dynamic type because all the template argument is provided in the definition.
```yaml
- TYPE:
    NAME: VecOfi32
    DEFINITION: std::vector<i32>
    INCLUDE:
      - "<vector>"
      - '"Types/i32.hpp"'
```

A Dynamic 2d vector of unknows Type.
In this situation TYPE is a class what will be given to the Vec2dOfT template.
```yaml
- TYPE:
    NAME: Vec2dOfT
    DEFINITION: std::vector<std::vector<TYPE> >
    INCLUDE:
      - "<vector>"
    ARG:
    - TYPE
    DYNAMIC: true
```

# How To Use a CompoMe Type
To a use a CompoMeType you need to include it.

## In C++
### Include it
All the type will be put in Types folder. if you define a namespace the type will be fine in the namespace folder. For example to use _i32_ include "Types/i32.hpp" for _Test::Prj1::myType_ include "Types/Test/Prj1/myType.hpp"

### Define and use it
Just use it as a standard C++ type.
`i32 a = 1;`
`i32 b;`
`VecOfi32 v = {1,2,3,4,5,6};`
`Vec2dOfT<i32> v2;`

# USEFUL INFORMATION
If you want to reuse all the Type define in the C/C++ CompoMe FrameWork The easiest way is to include "CompoMe.yaml".
You will get all the cstdint type (i8,i16,i32,i64,ui8,ui16,ui32,ui64), the natif one ( bool, double, float ...), some optional type (CompoMe::String,...) and Some standard C++ collection (array,map,vector,tuple,pair).

In a Simple project you doesn't  need to define new kind of TYPE.

## More Example
To get more Information and example of TYPE  you can read all the "code.yaml" file  place in "./Test/gen/cpp/1.TYPE" and the "main.cpp" file.

## Swig Utilisation
Refer to the swig documentation
