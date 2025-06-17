# Enumeration

Enum in C/C++ language also known as Enumeration is a user-defined datatype that is used to assign names to the integral constants. 

```cpp
enum enumName {constant1, constant2, constant3, ... so on... };
enum enumName {constant1 = value1, constant2 = value2, ... so on... };
```
- can be travesed using for loop
- can be used in switch case
- cannot extend any class or interface 
- can have methods, fields, and constructors in C++11 and later
- used to improved type safety and code readability
- Two enums cannot have same name in the same scope
- No variable can have a name which is already in some enums.

- C++11 has introduces `enum class` which provides strongly typed enumerations, preventing implicit conversions to integers and allowing for better scoping.

