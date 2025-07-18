### Strings

| Feature                        | C++ `std::string`                                  | C Character Array (`char[]`)                |
|---------------------------------|---------------------------------------------------|---------------------------------------------|
| Type                            | Class/Object                                      | Array of characters                         |
| Null Terminator                 | Managed internally                                | Must end with `'\0'`                        |
| Array Decay                     | No array decay                                    | Array decay occurs                          |
| Memory Allocation               | Dynamic                                           | Static (fixed size)                         |
| Modifiability                   | Contents can be modified                          | Contents can be modified                    |
| Built-in Functions              | Rich set of member functions (e.g., `.length()`)  | No built-in functions, use `<string.h>`     |
| Safety                          | Bounds checking, safer                            | No bounds checking, risk of overflow        |
| Assignment                      | Direct assignment supported (`str1 = str2`)       | Not supported, must use `strcpy()`          |
| Concatenation                   | Use `+` operator or `.append()`                   | Use `strcat()`                              |
| Comparison                      | Use `==`, `.compare()`                            | Use `strcmp()`                              |
| Usage                           | Preferred for modern C++                          | Used in C and legacy C++ code               |


- array decay occurs when passing a character array to a function, treating it as a pointer.

## C++ String Functions

- length() : returns the number of characters in the string.
- size() : same as length, returns the number of characters.
- empty() : checks if the string is empty.
- clear(): clears the string, making it empty.
- append(): appends a string or character to the end.
- swap(str1, str2): swaps the contents with another string.
- resize(new_size): changes the size of the string.
- find(substring): finds the first occurrence of a substring.
- push_back(char): adds a character to the end.
- pop_back(): removes the last character.
- substr(start, length): returns a substring from the string.
- erase(start, length): removes a portion of the string.

