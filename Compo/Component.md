
#  Model

| Field     | Description                                  | Form         | Opt | Def                | Example                                            |
|-----------|----------------------------------------------|--------------|-----|--------------------|----------------------------------------------------|
| NAME      | The name of the Component                    | String       |     |                    | C1, Struct_s1, MyStruct                            |
| NAMESPACE | The namespace of the Component               | String       | X   | ""                 | Package1, Package1::SubPackage2 , compo::base      |
| PARENT    | The class that you inherit                   | String       | X   | "CompoMe::Structs" | Pack1::S1, MyStruct                                |
| DATA      | list of Type Component                       | List<String> | X   | Null               | go to example section                              |
| FUNCTION  | a list of function to define                 | List<String> | X   | Null               | go to example section                              |
| OPTIONS   | option is a list                             | List ...     | X   | Null               | go to _options_ section                            |
| GEN       | a list of generator                          | List<String> | X   | Null               | go to _gen_ section                                |
| PROVIDE   | list of interface provided by this component | List<String> | x   | Null               | ["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"] |
| REQUIRE   | list of interface required by this component | List<String> | x   | Null               | ["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"] |

# Example

```yaml
- INTERFACE:
    NAME: I1
    ...
    
- INTERFACE:
    NAME: I2
    ...
    
- COMPONENT:
    NAME: Component_A
    DATA:
      - i32 b
    FUNCTION:
      - i32 f1 (i32 p1)
    PROVIDE:
      - I1 p1
    REQUIRE:
      - I2 r1



```
