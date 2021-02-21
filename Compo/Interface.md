A Interface is a list of _function_ and _data_ witch is provide by a COMPONENT.


CompoMe provide:
- Function
- data and get/set/acess
- Caller with transforme a string to a call and return a string as result (see example)
- Fake with take a call a convert it to a std::string and need a std:string for the answer.

# Model
| Field     | Description                    | Form         | Opt | Def                   | Example                                       |
|-----------|--------------------------------|--------------|-----|-----------------------|-----------------------------------------------|
| NAME      | The name of the Interface      | String       |     |                       | I1, Interface_I1, MyInterface                 |
| NAMESPACE | The namespace of the Interface | String       | X   | ""                    | Package1, Package1::SubPackage2 , compo::base |
| PARENT    | The class that you inherit     | String       | X   | "CompoMe::Interfaces" | Pack1::S1, MyInterface                        |
| DATA      | list of Type Interface         | List<String> | X   | Null                  | go to example section                         |
| FUNCTION  | a list of function to define   | List<String> | X   | Null                  | go to example section                         |
| OPTIONS   | option is a list               | List ...     | X   | Null                  | go to _options_ section                       |
| GEN       | a list of generator            | List<String> | X   | Null                  | go to _gen_ section                           |

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

```yaml
- INTERFACE:
    NAME: Interface1
    FUNCTION:
    - void f1()
    - void f2(i32 a, i32 b)
    - i32 f3(i32 a, i32 b, i32 c)
    DATA:
    - i32 data1
    - i32 data2
```

This will declare a interface with 3 functions "f1", "f2" and "f3" and 2 datas "data1" and "data2".

Caller
---
To introduce the concept of calller we will consider "Interface1_impl" with is a class implementation of the abstractClass/PureVirtual class "Interface1".

```cpp
// imagine a interface
Interface1_impl i;

// you ask for a stream
auto i_caller = i.get_caller_stream();

// you prepare the return value
std::pair<bool,std::string> return_value;

// we can now directly call function with CompoMe::Tools::call
// the return value is a pair:
// first value boolean true is call sucess
// second value is the result/return of the call

// function call
return_value = CompoMe::Tools::call(i_caller, "f1()");
return_value = CompoMe::Tools::call(i_caller, "f2(1,2)");
return_value = CompoMe::Tools::call(i_caller, "f3(1,2,3)");

// get/set data
return_value = CompoMe::Tools::call(i_caller, "set_data1(1)");
return_value = CompoMe::Tools::call(i_caller, "set_data2(2)");
return_value = CompoMe::Tools::call(i_caller, "get_data1()");
return_value = CompoMe::Tools::call(i_caller, "get_data2()");
```

If you want to test a Interface you can also use CompoMe::tools::term.
After a Call to CompoMe::tools::term you will see a prompt and you can call each function and see the result.
Use ? or help to get a introspection message.

Go to "Test/gen/cpp/0.EMPTY/1.cmd".
