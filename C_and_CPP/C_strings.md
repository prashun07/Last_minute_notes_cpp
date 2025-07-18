### Strings

Strings are a specific example of a one-dimension array. Where each element in the data collection is a single character. 
- the last element in a string is always the literal \0. 


> char message_1 [] - Array of chars.

> char * message_2 - Pointer to chars.

> uint8_t * message_3 - Pointer to uint8_t, equivalent to char.

> char str[] = {'H', 'e', 'l', 'l', 'o', '\0'};
The following shortcut can be used to achieve the same declaration:

> char str[] = "Hello";
Note that in the second option there is no need to explicitly add the trailing '\0' literal. Strings can be transversed and handled like any other array. The compiler automatically determines the size of the array to fit the string plus the null terminator.
- if size given is smaller than the string, the compiler will throw an error.(size= no of chars + 1 for the null terminator)

// Attribution of a string to an char array. ILLEGAL
> message_1 = "ERROR\n";

---
### Pointer to character vs. Character Arrays
String literals and character arrays are two different ways to represent strings in C. Here’s a breakdown of the differences:

> char str[] = "Hello";
Type: Array of characters
Memory Allocation: Allocated on the stack (or in static memory if declared globally)
Modifiability: ✅ You can modify the contents:
> str[0] = 'h';  // Valid
Storage:
// Equivalent to:
char str[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
- A copy of the string literal is made and stored in the array.

> char *str = "Hello";
Type: Pointer to character
Memory Allocation: Points to a string literal stored in read-only memory
Modifiability: ❌ Undefined behavior if you try to modify it:
> str[0] = 'h';  // ❌ Undefined behavior (usually causes crash)
Storage:
- The pointer str just stores the address of the string literal "Hello".
-  No copy is made. The pointer points to a constant string.

---

### String Functions
#### <stdio.h>

- sprintf() - The same as printf but to a string.
- sscanf()  - The same as scanf but from a string.

#### <stdlib.h>

- atoi() - Converts a string to a int.
- atof() - Converts a string to a double.
- atol() - Converts a string to a long int.
- getenv() - Get environment variables or proprieties from the
           system.

#### <string.h>

- memcmp()   - Compares the first n chars of the sequence s1 and s2.
- memcpy()   - Copies the first n chars from s2 to s1.
- memset()   - Initializes a array or a memory block with a value.

- strcat()   - Concatenates a copy of s2 to s1.
- strchr()   - Returns the first occurrence of a char in a string.
- strcmp()   - Compares the chars of the sequence s1 with s2.

- strcpy()   - Copies the string pointed by s2 into a array s1.
- strlen()   - Returns the length of a string (in bytes). The '\0'
             is not included.
- strncat()  - Appends up to n characters of string s2 to the end of
             string s1.
- strncmp()  - Equal to strcmp() but only for n characters.
- strncpy()  - Equal to strcpy() but only for n characters.

```c
#include<stdio.h>
#include<string.h>

int main()
{
    char src[]="Prashun";
    char dest[20];
    
    printf("String src: %s\n",src);
    printf("Length of src: %ld\n",strlen(src));
    
    strcpy(dest,src);
    printf("Length of dest: %d\n",(int)strlen(dest));
    printf("String dest: %s\n",dest);
    int result=strcmp(dest , src);
    if(result<0)
    {
        printf("String dest is smaller than src\n");
    }
    else if(result>0)
    {
        printf("String src is larger than src\n");
    }
    else{
        printf("Both string are equal\n");
    }
    
    char last[]="Jha";
    printf("Length of last: %ld\n",strlen(last));
    printf("String last: %s\n",last);
    strcat(src," "); //Add space at the last of src
    strcat(src,last); //Concate both string last into src
    
    printf("Length of src: %ld\n",strlen(src));
    printf("String src: %s\n", src);
    
    
    //first occurrence of a character
    char str[10]="RadaRdaRR";
    char * index=strchr(str,'R');
    printf("First occurrence of R at %ld\n",index-str);
    
    //last occurrence of a character
    char *lastind=strrchr(str,'a');
    printf("Last occurrence of a at %ld\n",lastind-str);
    
    //Find a substring in a large string
    char large_str[100]="Hello Geeks,I am prashun,and a Geeks.";
    
    char *sstr= strstr(large_str, "Geeks");
    
    if(sstr)
    {
        printf("String Geeks found\n");
    }
    else{
        printf("String Geeks not found\n");
    }
    
    //Spilt string corresponding to a char
    printf("\nSpilt string.....\n");
    char * delimiter=strtok(large_str,",");
    
    while(delimiter!=NULL)
    {
        printf("%s\n",delimiter);
        delimiter=strtok(NULL,",");
    }
    
    return 0;
}
```

**Output** 
> String src: Prashun
> Length of src: 7
> Length of dest: 7
> String dest: Prashun
> Both string are equal
> Length of last: 3
> String last: Jha
> Length of src: 11
> String src: Prashun Jha
> First occurrence of R at 0
> Last occurrence of a at 6
> String Geeks found

> Split string.....
> Hello Geeks
> I am prashun
> and a Geeks.



### Important interview questions
**🔹 Reverse a String**
```c
#include <stdio.h>
#include <string.h>

void reverse(char *str) {
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++) {
        char temp = str[i];
        str[i] = str[len - 1 - i];
        str[len - 1 - i] = temp;
    }
}

int main() {
    char str[] = "hello";
    reverse(str);
    printf("Reversed: %s\n", str);
    return 0;
}
```

**🔹 Check Palindrome**
```c
#include <stdio.h>
#include <string.h>

int isPalindrome(char *str) {
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++) {
        if (str[i] != str[len - 1 - i])
            return 0;
    }
    return 1;
}

int main() {
    char str[] = "madam";
    if (isPalindrome(str))
        printf("Palindrome\n");
    else
        printf("Not a Palindrome\n");
    return 0;
}
```
**🔹 String Length (without strlen)**
```c
#include <stdio.h>
#include <string.h>

int my_strlen(char *str) {
    int len = 0;
    while (str[len] != '\0') len++;
    return len;
}
```

**🔹 String Copy (without strcpy)**
```c
void my_strcpy(char *dest, const char *src) {
    while (*src) {
        *dest++ = *src++;
    }
    *dest = '\0';
}
```

**🔹 Concatenate Two Strings**
```c
void my_strcat(char *dest, const char *src) {
    while (*dest) dest++;         // move to end of dest
    while (*src) *dest++ = *src++; // copy src
    *dest = '\0';
}
``` 

**🔹 Count Vowels and Consonants**
```c
#include <string.h>
#include <stdio.h>

void countVowelsConsonants(const char *str, int *vowels, int *consonants) {
    *vowels = *consonants = 0;
    while (*str) {
        char c = *str;
        if ((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z')) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' ||
                c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U')
                (*vowels)++;
            else
                (*consonants)++;
        }
        str++;
    }
}
```

**🔹 Find a Substring (like strstr)**
```c
char *my_strstr(const char *haystack, const char *needle) {
    int len_h = strlen(haystack);
    int len_n = strlen(needle);

    for (int i = 0; i <= len_h - len_n; i++) {
        int j = 0;
        while (j < len_n && haystack[i + j] == needle[j]) j++;
        if (j == len_n) return (char *)(haystack + i);
    }
    return NULL;
}
``` 

**🔹 Compare Two Strings (like strcmp)**
```c
int my_strcmp(const char *s1, const char *s2) {
    while (*s1 && (*s1 == *s2)) {
        s1++;
        s2++;
    }
    return s1 - s2;
}
```

### Strings in cpp

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
-