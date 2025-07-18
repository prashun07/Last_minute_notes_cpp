## References in C++
References in C++ are aliases for existing variables. They are declared using the `&` symbol.

```cpp
int a = 10;
int& ref = a; // ref is a reference to a
```

### Key Points
- References must be initialized when declared.
- They cannot be changed to refer to another variable.
- Useful for function arguments to avoid copying.
- Pointer can be null, but references must always refer to a valid object.


### Example: Function with Reference Parameter
```cpp
void increment(int& num) {
    num++;
}

int main() {
    int value = 5;
    increment(value); // value is now 6
}
```

## Templates in C++
Templates allow writing generic and reusable code. They enable functions and classes to operate with any data type.


```cpp
#include <iostream>

template <typename T>
T add(T a, T b) {
    return a + b;
}
template <typename T>           
class Box {
    T value;
public:
    Box(T val) : value(val) {}
    T getValue() const { return value; }
};
int main() {
    Box<int> intBox(10);
    Box<double> doubleBox(5.5);
    std::cout << intBox.getValue() << std::endl; // Outputs: 10
    std::cout << doubleBox.getValue() << std::endl; // Outputs: 5.5
    add(5, 10); // Works with integers
    add(5.5, 2.5); // Works with doubles
    return 0;

}
```
