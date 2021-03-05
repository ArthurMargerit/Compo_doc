
#  Model

| Field              | Description                                                  | Form   | Opt | Def                | Example                                            |   |
|--------------------|--------------------------------------------------------------|--------|-----|--------------------|----------------------------------------------------|---|
| NAME               | The name of the Component                                    | String |     |                    | C1, Struct_s1, MyStruct                            |   |
| NAMESPACE          | The namespace of the Component                               | String | X   | ""                 | Package1, Package1::SubPackage2 , compo::base      |   |
| PARENT             | The class that you inherit                                   | String | X   | "CompoMe::Structs" | Pack1::S1, MyStruct                                |   |
| DATA               | list of Type Component                                       | List   | X   | Null               | go to example section                              |   |
| FUNCTION           | a list of function to define                                 | List   | X   | Null               | go to example section                              |   |
| OPTIONS            | option is a list                                             | List   | X   | Null               | go to _options_ section                            |   |
| GEN                | a list of generator                                          | List   | X   | Null               | go to _gen_ section                                |   |
| PROVIDE            | list of interface provided by this component                 | List   | x   | Null               | ["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"] |   |
| REQUIRE            | list of interface required by this component                 | List   | x   | Null               | ["INTERFACE_NAME name_1", "INTERFACE_NAME name_2"] |   |
| EMITER             | list of bus that can be emit                                 | List   | x   | Null               | ...                                                |   |
| RECIEVER           | list of bus that can be recieve                              | List   | x   | Null               | ...                                                |   |
| CONNECTION         | list of connection between sub component and provide/require | List   | x   | Null               | ...                                                |   |
| COMPONENT_INSTANCE | List of sub component                                        | List   | x   | Null               | ...                                                |   |

# options

| Field          | Description |
|----------------|-------------|
| OPTIONS.SWIG   |             |
| OPTIONS.STREAM |             |


# Example

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
      - i32 f1 (i32 p1)
    PROVIDE:
      - I1 p1
    REQUIRE:
      - I2 r1
```

```cpp
Component_A c;

// atribute
c.set_b(1);
c.get_b();

// function call
c.f1(2);

// provide
c.get_p1().f2();
c.get_p1().f3(1,2);

```

