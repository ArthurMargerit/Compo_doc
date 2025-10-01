An Interface is a list of functions and data provided by a COMPONENT.

CompoMe provides:
 - Functions
 - Data with get/set/access methods
 - Caller, which transforms a string into a function call and returns a string as the result (see example)
 - Fake, which takes a call, converts it to a std::string, and requires a std::string for the answer

# Model
| Field         | Description                    | Form         | Opt | Default                 | Example                                            |
| ------------- | ------------------------------ | ------------ | --- | ----------------------- | -------------------------------------------------- |
| **NAME**      | The name of the Interface      | String       |     |                         | `I1`, `Interface_I1`, `MyInterface`                |
| **NAMESPACE** | The namespace of the Interface | String       | X   | `""`                    | `Package1`, `Package1::SubPackage2`, `compo::base` |
| **PARENT**    | The class to inherit from      | String       | X   | `"CompoMe::Interfaces"` | `Pack1::S1`, `MyInterface`                         |
| **DATA**      | A list of Interface data types | List<String> | X   | `Null`                  | See example section                                |
| **FUNCTION**  | A list of functions to define  | List<String> | X   | `Null`                  | See example section                                |
| **OPTIONS**   | A list of options              | List<...>    | X   | `Null`                  | See [*options*](#) section                         |
| **GEN**       | A list of generators           | List<String> | X   | `Null`                  | See [*gen*](#) section                             |

## Options
| Field                     | Description                                                                            | Example    | Default |
| ------------------------- | -------------------------------------------------------------------------------------- | ---------- | ------- |
| **OPTIONS.FAKE_DBUS**     | Generate the Fake Interface class that translates a call into a *dbusMessage*          | True/False | False   |
| **OPTIONS.CALLER_DBUS**   | Generate the Caller class that translates a *dbusMessage* into a function/get/set call | True/False | False   |
| **OPTIONS.FAKE_STREAM**   | Generate the Fake Interface class that translates a call into a `std::string`          | True/False | True    |
| **OPTIONS.CALLER_STREAM** | Generate the Caller class that translates a `std::string` into a call                  | True/False | True    |
| **OPTIONS.FAKE_JSON**     | Generate the Fake Interface class that translates a call into a JSON object            | True/False | False   |
| **OPTIONS.CALLER_JSON**   | Generate the Caller class that translates a JSON object into a call                    | True/False | False   |


## List of Generators

| Field            | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| **async_call**   | Generate an interface with no return value                    |
| **async_return** | Generate an interface for the return value of an async call   |
| **add_params**   | Add an extra parameter to each function                       |
| **many**         | Each function will take a vector as input and return a vector |
| **many_return**  | Each function will return a vector                            |



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
To introduce the concept of a Caller, we will consider Interface1_impl, which is a concrete implementation of the abstract (pure virtual) class Interface1.

```cpp
// Imagine an interface implementation
Interface1_impl i;

// You request a stream caller
auto i_caller = i.get_caller_stream();

// Prepare the return value
std::pair<bool, std::string> return_value;

// We can now directly call functions with CompoMe::Tools::call.
// The return value is a pair:
//   - First: a boolean (true if the call succeeded)
//   - Second: the result (or return value) of the call, as a string

// Function calls
return_value = CompoMe::Tools::call(i_caller, "f1()");
return_value = CompoMe::Tools::call(i_caller, "f2(1,2)");
return_value = CompoMe::Tools::call(i_caller, "f3(1,2,3)");

// Get/Set data
return_value = CompoMe::Tools::call(i_caller, "set_data1(1)");
return_value = CompoMe::Tools::call(i_caller, "set_data2(2)");
return_value = CompoMe::Tools::call(i_caller, "get_data1()");
return_value = CompoMe::Tools::call(i_caller, "get_data2()");
```

If you want to test an Interface, you can use CompoMe::tools::term.
After calling CompoMe::tools::term, you will see a prompt where you can invoke each function and view the result.
Use ? or help to display an introspection message.

For an example, see:
Test/gen/cpp/0.EMPTY/1.cmd
