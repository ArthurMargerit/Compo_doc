# You can find many generator but all of them  have the same architecture/organisation.

A generator is define in a yaml file, you specified this file in CONFIG with the field "generation_model".

Two elements are availables in the .yaml file of generator.

* A target is a link between the model and a list of generated file
* A import is an other .yaml file with target or impport

The target have many field
| FIELD       | DESCRIPTION                                                                                  | EXAMPLE                                                                                                     |
|-------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| NAME        | The Name                                                                                     | "Struct"                                                                                                    |
| TARGET_NAME | This field can be customize with the CompoMe Element Data                                    | "Struct.{{D_NAME}}.Core" for a element name S1 you will get "Struct.S1.Core"                                |
| FOR         | A Regex witch select the element of CompoMe Element                                          | "STRUCTS.*", "STRUCTS.*(THIS_STRUCT).DATA.*(THIS_DATA)", ""                                                 |
| FILES       | A list of Files to generate                                                                  | [{"IN": "Struct.cpp","OUT": "Struct_{{F_NAME}}.cpp" },{"IN": "Struct.cpp","OUT": "Struct_{{F_NAME}}.cpp" }] |
| FILES.*.IN  | The Path to the Jinja Template Model file                                                    | "my_Struct_template.cpp"                                                                                    |
| FILES.*.OUT | The output Path of the model after generation                                                |                                                                                                             |
| IF          | If you want to select a elment in function of data                                           | {"OPTIONS.SWIG":True}                                                                                       |
| IF.*.AND.*  | To write complexe selection witch require that all child element need to be true             | ...                                                                                                         |
| IF.*.OR.*   | To write complexe selection witch require that one or more element to be true                | ...                                                                                                         |
| IF.*.NOT.*  | To write complexe selection witch require the négation                                       | ...                                                                                                         |
| COMMANDS    | A list of shell command to call after generation ("format", check, clean, optimize, etc ...) | ["clang-format Struct_{{NAME}}.cpp"]                                                                        |
| DEFAULT     | Some vars and their values definitions                                                       | ["MyVar":"MyStringvalue","MyStruct":"MODEL:STRUCTS"]                                                        |


EXAMPLE:
----

Imagine a project wherre the developer want to get information of the structure like std::type\_index, std::type\_info.

In the generation.yaml you add a target. This target as for objectif to generate a "introspection class" with more functionality than the standart solution.

To explain a target we will consider the model.yaml
```yaml
- STRUCT: 
    NAME: One_Struct
    NAMESPACE: ns::ns2
    
- STRUCT: 
    NAME: A_other_struct
    NAMESPACE: ns
    DATA: 
    - i32 a
    - i32 b
    FUNCTION:
    - void f1()
    - i32 f2(i32 a)
    - i32 f3(i32 a,i32 b)
```

Add a target to generation.yaml
```yaml
- NAME: "Structs_Info" # choose a name
  FOR: "STRUCTS.*"     # you want to generat it for each target this one will match 2 time one for ns::ns2::One_Struct and ns::A_other_struct
  DEFAULT:
    MAIN: "MODEL:."     # we want a access to the parsing model
    AUTHOR: "ME"        # we define a variable with the value "ME"
  TARGET_NAME: "STRUCT.{{D_NAME}}.INFO" # for eg the target name is "STRUCT.ns::struct_of_my_project.INFO"
  FILES:
    - IN: "Structs/Struct_Info_template.hpp" # the jinja template file to generate the .hpp
      OUT: "inc/Structs/{{F_NAME}}_Info.hpp" # the output for eg is "inc/Structs/ns/One_Struct_Info.hpp" "inc/Structs/ns/ns2/A_other_struct_Info.hpp"
    - IN: "Structs/Struct_Info_template.cpp" # the jinja template file to generate the .cpp
      OUT: "src/Structs/{{F_NAME}}_Info.cpp" # the output for eg is "src/Structs/ns/One_Struct_Info.cpp" "src/Structs/ns/ns2/A_other_struct_Info.cpp"
  COMMANDS:
    - "clang-format -i inc/Structs/{{F_NAME}}_Info.hpp" # format the .hpp file
    - "clang-format -i src/Structs/{{F_NAME}}_Info.cpp" # format the .cpp file
```

And 2 files in template dir.

The HPP files is "Structs/Struct_Info_template.hpp"
```yaml
#pragma once
// File: {{F_NAME}}_info.hpp
// Author: {{AUTHOR}}

// Do what you want ... 
{%with NAMESPACE=d.NAMESPACE%} {% include "helper/namespace_open.hpp" with context %}{%endwith%}

class {{NAME}}_Info {
    void get_info();
    void get_function_list();
    void get_data_list();
};

{%with NAMESPACE=d.NAMESPACE%} {% include "helper/namespace_close.hpp" with context %}{%endwith%}
```

The CPP files is "Structs/Struct_Info_template.cpp"
```yaml
#include "Structs/{{F_NAME}}_Info.hpp"

{%with NAMESPACE=d.NAMESPACE%} {% include "helper/namespace_open.hpp" with context %}{%endwith%}

void {{NAME}}_Info::get_info() {
    // ...
}

void {{NAME}}_Info::get_function_list() {
    // ...
}

void {{NAME}}_Info::get_data_list() {
    // ...
}
{%with NAMESPACE=d.NAMESPACE%} {% include "helper/namespace_close.hpp" with context %}{%endwith%}
```






