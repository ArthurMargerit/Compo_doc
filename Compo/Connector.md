A Connector is like a component but without any "user code". 
All of the code is generated by the template. You can easily add new template to this system.

This system help the SoftwareArchitect to avoid loosing time in obvious code.

| Field          | Description                                      | Form   | Opt | def  | example             |
|----------------|--------------------------------------------------|--------|-----|------|---------------------|
| NAME           | name of the connector                            | String |     |      | Con_1               |
| NAMESPACE      | namespace of the connector                       | String | x   | ""   | ns, ns1::ns2        |
| MODEL          | list of file for generation                      | Dict   | x   | None |                     |
| MODEL.CPP      | cpp                                              | String | x   |      | My_template.cpp     |
| MODEL.HPP      | hpp                                              | String | x   |      | My_template.hpp     |
| PROVIDE        | like component                                   | List   | x   | []   |                     |
| REQUIRE        | like require                                     | List   | x   | []   |                     |
| DATA           | like data                                        | List   | x   | []   |                     |
| ** OTHERVAR ** | Any variable that you want in template           | wyw    | x   | None | "v1","flop","Lapin" |
| GEN            | generate all the field for one kind of connector | String | x   | None | logger(I1),...      |

# Example
## Logger
All the call done by this interface will be log and the call will be transfert to the real interface.
The same step is done for the return value.
The Time between the call and the result is also measure and reported.

```cpp
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
      
# This connector is I1_logger equivalent
- CONNECTOR:
    NAME: I1_logger2
    GEN: logger(I1)
```

## Async
Expose a Interface to a async system.
For a Interface I1, you will get 2 interface , one Interface for the call and a return Interface.
The Async Connector recieve a call by the Interface_call do the call and will call you back with the result.

```
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

## Dispatcher
A Dispatcher is a system to send the call to a list of interface.

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

## Distributeur
In many situation you want to distribute a call and get 1 result.
A Call to the interface number X and them 1 result.
A Call to many Interface With same params, them you will get many result.
A Call to many interface with a vector of params, them  many result.

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
