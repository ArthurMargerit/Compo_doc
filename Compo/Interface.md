A Interface is a list of _function_ and _data_ witch is provide by a COMPONENT.


CompoMe provide:
- Function
- data and get/set/acess
- Caller with transforme a string to a call and return a string as result (see example)
- Fake with take a call a convert it to a std::string and need a std:string for the answer.

# Model
| Field     | Description                  | Form         | Opt | Def                | Example                                       |
|-----------|------------------------------|--------------|-----|--------------------|-----------------------------------------------|
| NAME      | The name of the Struct       | String       |     |                    | S1, Struct_s1, MyStruct                       |
| NAMESPACE | The namespace of the Struct  | String       | X   | ""                 | Package1, Package1::SubPackage2 , compo::base |
| PARENT    | The class that you inherit   | String       | X   | "CompoMe::Structs" | Pack1::S1, MyStruct                           |
| DATA      | list of Type Struct          | List<String> | X   | Null               | go to example section                         |
| FUNCTION  | a list of function to define | List<String> | X   | Null               | go to example section                         |
| OPTIONS   | option is a list             | List ...     | X   | Null               | go to _options_ section                        |
| GEN       | a list of generator          | List<String> | X   | Null               | go to _gen_ section                            |

## Options
| Field                 | Description                                                                   | Example    | Default |
|-----------------------|-------------------------------------------------------------------------------|------------|---------|
| OPTIONS.FAKE_DBUS     | Generate the Fake Interface class witch translate the call to a _dbusMessage_ | True/False | False   |
| OPTIONS.CALLER_DBUS   | Generate the Call Class witch translate a _dbusMessage_ to a function/get/set | True/False | False   |
| OPTIONS.FAKE_STREAM   | Generate the Fake Interface class witch translate the call to a std::string   | True/False | True    |
| OPTIONS.CALLER_STREAM | Generate the Call Class witch translate a std::string to a call               | True/False | True    |

## Gen
List of generator:
| Field        | Description                                               |
|--------------|-----------------------------------------------------------|
| async_call   | Generate a interface with no return                       |
| async_return | Generate a interface for the return value of a async call |
| add_params   | Rajoute  un parametre a chaque function                   |
| many         | Each function will have a vector as input and as return   |
| many_return  | Each function will have a vector as return                |


# Example
