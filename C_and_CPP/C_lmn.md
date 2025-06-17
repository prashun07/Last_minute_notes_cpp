# Simple notes on the C Programming Language
Simple notes to revise the C programming language.

## Table of contents

- [Simple notes on the C Programming Language](#simple-notes-on-the-c-programming-language)
  - [Description](#description)
  - [Table of contents](#table-of-contents)
  - [Names in C](#names-in-c)
    - [variable](#variable)
    - [Data types](#data-types)
      - [Basic data types in C](#basic-data-types-in-c)
      - [Derived data types in C](#derived-data-types-in-c)
      - [User defined data types in C](#user-defined-data-types-in-c)
        - [Identifiers](#identifiers)   
        - [Keywords](#keywords)
    - [Input and output functions](#input-and-output-functions)
    - [Escape sequences](#escape-sequences)
    - [Switch case statement](#switch-case-statement)
    - [conditional operators](#conditional-operators)
    - [Jump statements](#jump-statements)
      - [break statement](#break-statement)
      - [continue statement](#continue-statement)
      - [goto statement](#goto-statement)
    - [macros in c](#macros-in-c)
    - [Function that exits a program - exit()](#function-that-exits-a-program---exit)
    - [❌ Why not use void main()?](#why-not-use-void-main) 
    - [TypeDef's](#typedefs)
    - [Arrays](#arrays)
    - [Strings](#strings)
    - [Pointers](#pointers)
    - [Multidimensional arrays](#multidimensional-arrays)
    - [Logical operators](#logical-operators)
    - [Decrement and increment operators](#decrement-and-increment-operators)
    - [Comparing floating point values](#comparing-floating-point-values)
    - [Bit operators](#bit-operators)
    - [sizeof operator](#sizeof-operator)  
    - [Structures in C](#structures-in-c)
        - [definition of a structure](#definition-of-a-structure)
        - [Declaration of a structure variable](#declaration-of-a-structure-variable)
        - [Accessing the members of a structure](#accessing-the-members-of-a-structure)
        - [size of a structure](#size-of-a-structure)
        - [copy structure](#copy-structure)
        - [Passing structures to functions](#passing-structures-to-functions)   
        - [typedef's for structures](#typedefs-for-structures)
    - [Bit fields](#bit-fields)
    - [Unions in C](#unions-in-c)
    - [dynamic memory allocation](#dynamic-memory-allocation)
    - [Enumerations in C](#enumerations-in-c)
    - [file handling](#file-handling)
    - [Preprocessor directives](#preprocessor-directives-1)


## Names in C
They are user-defined labels for Variables, Functions, Constants, Arrays, Structures, Unions, Enums, Macros and Typedefs.

- They are case sensitive.
- They may start with a letter or a underscore "_".
- Can use letters (A-Z, a-z), digits (0-9), and underscores _
- Cannot be a keyword.

## variable
Variables are used to store data that can be modified during program execution. They have a name, a type, and a value.

- local variables: declared inside a function or block and can only be accessed within that function or block.
- global variables: declared outside any function and can be accessed from any function in the program.
- static variables: declared with the `static` keyword, they retain their value between function calls and are only accessible within the function they are declared in.
- register variables: declared with the `register` keyword, they are stored in CPU registers for faster access, but their address cannot be taken.
- extern variables: declared with the `extern` keyword, they are used to access global variables defined in other files or translation units.
- auto variables: declared with the `auto` keyword, they are the default storage class for local variables and are automatically allocated and deallocated when the function is called and exited, respectively.

## Data types

### Basic data types in C
```c
char //for characters
int //for integers
float //for single precision floating point
double //for double precision floating point
void //for valueless data

//data type modifiers:
short 
long   
signed
unsigned
// example:
unsigned int num_1 = 2;

// Scientific notation
float num_2  = 5e-5;
double num_3 = 3.7e12;
```
### Derived data types in C
- Array
- Pointer

### User defined data types in C
- structure
- Union
- Enumeration (enum)

## Identifiers
Identifiers are names given to various program elements such as variables, functions, arrays, structures, unions, and enums. They must follow the same rules as names in C.

## Keywords
Keywords are reserved words in C that have special meaning and cannot be used as identifiers. They are used to define the syntax and structure of the language. Some common keywords include:
- auto, float, int, struct, break, for, long, switch, case, goto, register, typedef, char, if, return, union, continue, else, sizeof, void, default, enum, extern, static, volatile.

## Input and output functions
- scanf and printf

```c
#include <stdlib.h>

int main(void){
    int num_1 = 1;
    int num_2 = 1;
    char name[20];
    printf("The number %d + %d = %d", num_1, num_2, num_1 + num_2);

//these are called format specifiers
    //  %d    signed integer
    //  %f    floating point
    //  %c    char
    //  %s    NULL or '\0' terminated string
    //  %x    Hexadecimal number
    //  %o    Octal number
    //  %lf  double
    //  %p    pointer
    //  %u    unsigned integer
    //  %%   percent sign

    int num_3;

    scanf("%d", &num_3);

    scanf("%s", name); // Read a string until a space or newline is found.
    printf("Hello %s!\n", name);
    // scanf("%[^\n]", name); // Read a string until a newline is found.
    // or getline(name, sizeof(name)); // Read a string until a newline is found.
    return 0;
}
```
## Escape sequences
- `\n` - New line
- `\t` - Horizontal tab
- `\\` - Backslash
- `\'` - Single quote

## Switch case statement
```c
#include <stdio.h>
int main()
{
    switch (expression) {
        case constant_1:
            // code to be executed if expression == constant_1
            break;
        case constant_2:
            // code to be executed if expression == constant_2
            break;
        // more cases...
        default:
            // code to be executed if expression doesn't match any case
    }
}

```
## conditional operators
```c
//(condition) ? expression_if_true : expression_if_false;
int a = 5, b = 10;
int max = (a > b) ? a : b; // max will be 10
```
## Jump statements

### break statement
-  used to exit from a loop and bring the program control to the next statement following the loop.
### continue statement
-  used to skip the current iteration of a loop and proceed to the next iteration.
### goto statement
- used to transfer control to a labeled statement in the program. It is generally discouraged due to its potential to create unstructured code.

## macros in c
- a macro is a preprocessor directive that defines a name for a piece of code or a constant value.
```c
#define DATE 31 //object like macro
#define SQUARE(x) ((x) * (x)) //function like macro
#define MAX(a, b) ((a) > (b) ? (a) : (b)) //function like macro with conditional operator
#define INSTAGRAM_FOLLOWERS 1000  // chain macros
// Multi-line Macro definition
#define ELE 1, \
            2, \
            3
```


### Function that exits a program - exit()

```c
#include <stdlib.h>

int main(void){

    // Exit successfully. The program terminates at this point.
    exit(0);

    // Exit with error.
    exit(-1);
}

```

### ❌ Why not use void main()?**

- Undefined behavior: It’s non-standard.
- No return value to indicate success/failure to the OS.
- Portable code should conform to the standard (int main()).
- Some compilers may allow it, but it’s not guaranteed to work everywhere.


## TypeDef's

```c
// Defining a new type 1.
typedef unsigned long int ULINT_T;

ULINT_T var_A = 2;

// Defining a new type 2.
typedef char * PT_TO_CHAR_T;

PT_TO_CHAR_T p1, p2;   // This is equal to "char *p1, *p2;"

// Defining a new type 3.
typedef enum { 
              red, 
              green, 
              blue 
             } COLOR_T;

COLOR_T my_color = green;

```
## Arrays
- fixed size homogeneous collection of elements stored in contiguous memory locations.
- Elements are accessed using indices.
- size cannot be changed after declaration.

syntax:
```c    
data_type arr_name [size1];   // 1D array
data_type arr_name [size1][size2];   // 2D array
data_type arr_name [size1][size2][size3];    // 3D array
```

## Strings
- Strings in C are arrays of characters terminated by a null character `\0`.
- They can be defined using character arrays.
Syntax:
```c
// char string_name [] = "string literal";
// char string_name[size];
char str_name[5];   // Character array
char str_name[] = "Hello";   // String literal
```
### string functions:
- strlen() - returns the length of a string.
- strcpy() - copies a string to another string.
- strcat() - concatenates two strings.
- strcmp() - compares two strings.
- strchr() - finds the first occurrence of a character in a string.
- strstr() - finds the first occurrence of a substring in a string.

```c
// char message_1 [] - Array of chars.

// A array of char's, '\0' terminated, but it doesn't have to be '\0' terminated.  
char message_1[] = {'b','a','t','a','t','i','n','h','a','s','\n', '\0'};

// Also valid - '\0' terminated string.
char message_1[] = "batatinhas\n"; // '\0' (null terminator) automatically added by the compiler.


// The address of the first position of the array.
message_1

// The first character, the character 'b'.
message_1[0]

// The address of the second character, the address of character 'a'.
&message_1[1]

// Attribution of a string to an char array. ILLEGAL
message_1 = "ERROR\n";


// char * message_2 - Pointer to chars.

// It places the '\0' automatically.
char * message_2 = "cemelhas\n"; (read-only string literal)

// The address of the first character of the '\0' terminated string.
message_2

// The first character 'c' of the '\0' terminated string.
*message_2

// The third character 'm' of the '\0' terminated string.
*(message_2 + 2)

// Attribution of character 'p' to the third character.
*(message_2 + 2) = 'p'; // ILLEGAL, because the string literal is read-only.

// Makes the pointer char *, point to the array of characters, the inverse isn't valid.
message_2 = message_1;

// uint8_t * message_3 - Pointer to uint8_t, equivalent to char. 

// It places the '\0' automatically.
uint8_t * message_3 = "cemelhas_2\n";

```
[👉 Click for more details](./C_strings.md)

## Pointers

- A pointer is a variable that stores the address of another variable.
 Note : there is seperate readme file for pointer, refer [C_and_CPP/pointers.md](C_and_CPP/pointers.md)
### types of pointers:
  - Null pointer: A pointer that does not point to any valid memory location.
  - Void pointer: A pointer that can point to any data type.
  - wild pointer: A pointer that points to a random memory location, not initialized.
  - Dangling pointer: A pointer that points to a memory location that has been freed or deallocated.
  - Function pointer: A pointer that points to a function.
  - Pointer to pointer: A pointer that points to another pointer.
  - Array of pointers: An array where each element is a pointer.

## Multidimensional arrays

```c
// Initialization
int ar[4][3] = {{1, 2},          // Row x Column
                {3},
                {4, 5}};

// Generates the following array:
//
//  ar =  1  2  0  0
//        3  0  0  0
//        4  5  0  0
//        0  0  0  0
//
// ar[2][1] --> value 5        Row 2  Column 1


// Passing arrays to functions. 
// The array isn't copied only de reference is copied.

int clear(int ar_tmp[][3], size_row, size_column,
          position_clear_x, position_clear_y){
    ar_temp[position_clear_x][position_clear_y] = 0;
}

// To call the function do...

clear(ar);

// Other way of defining the function to pass an array using pointers would be...

int clear(int **ar_tmp, size_row, size_column, position_clear_x, position_clear_y){
    // but in here you would need to make some calculation for
    // the indexing of the using pointers and pointer arithmetic. 

}
// To pass to a function an array with a single dimension.

int clear(int ar_tmp[], size_row, position_clear){
    ar_temp[position_clear] = 0;
}

clear(ar);

// Or it could be done using pointers.

int clear(int *ar_tmp, size_row, position_clear){
    *(ar_temp + position_clear) = 0;
}
```

## Logical operators

1. && - AND

2. || - OR

3. !  - NOT  

## Decrement and increment operators

1. ++a - increment a, then get value of a. Prefix.

2. a++ - get value of a, then increment a. Posfix.

## Comparing floating point values

1. Never compare with equals "==".
 
```c
float x = 2.0;

if ( (x > 2.0 - 0.0001) && (x < 2.0 + 0.0001)){
    
    // Inside the tolerance interval.
    // Always compare with interval, never with exact values, 
    // because floating point representation may not have exact values. 
}
```


## Bit operators

```c
>> // Right shift
<< // Left shift
&  // Bitwise AND
|  // Bitwise OR
^  // Bitwise XOR
~  // Bitwise complement
```

## sizeof operator

```c
sizeof(char);    // 1 bytes
sizeof(int);     // 4 bytes  - Value depends on the CPU architecture and compiler.
sizeof(int *);   // 8 bytes  - For a 64 bits CPU

int ar_var[2] = { 0, 1 };

sizeof(ar_var);   // 8 bytes - Value depends on the CPU architecture,
                  //           compiler and memory alignment.

// Number of elements of the array.
int num_elems = sizeof(ar_var) / sizeof(ar_var[0]) ;   // 2 elements

```

## Structures in C

- user defined data type.
- can contain different data types.
- struct keyword is used to define a structure.

## definition of a structure
```c
// Definition of a structure.
struct structure_name {
    data_type member_1;
    data_type member_2;
    ...
}; // no memory is allocated for the structure at this point.

// Definition of a structure. Example 1
struct car{
    char brandName[];
    char *model;
    int numDoors;
};
// Definition of a structure. Example 2
struct car{
    char brandName[];
    char *model;
    int numDoors;
} cs, cs_ar[10], *pcs;  
```

## Declaration of a structure variable
```c
// Declaration of the variable of the structure to use it
struct structure_name variable_name;
// Example 
struct car mycar;
```
## Accessing the members of a structure.
 - dot operator is used to access the members of a structure.
 - in case of a pointer to a structure, the arrow operator is used.

```c
cs.numDoors=5; //Variable car structure.

cs_ar[1].numDoors=4; //Variable array of car structures.

(* pcs).numDoors=2;  // or pcs->numDoors=2; (both are same) //Pointer to structure of car.
```

Structure members cannot be initialized at the time of structure definition.

```c
// Initialization of a structure variable.
struct structure_name{
    data_type member_1=value1; //Compiler error. connot initialize members here.
    //because there is no memory allocated for the structure at this point.
};
``` 

## initialization of a structure members
- uninitialized structure variable contains garbage values.

Initialization using assignment operator.
```c
struct point{
    int x;
    int y;
};

int main(){
    struct point p1;
    p1.x = 10; //using assignment operator (=).
    p1.y = 20;
    printf("Point p1: (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```
Initialization using initializer list.
```c    
struct point {
    int x;
    int y;
};
int main()
{
    struct point p1 = {10, 20}; //using initializer list.

    printf("Point p1: (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```
Initialization using designated initializers (C99 and later).
```c
struct point {
    int x;
    int y;
};
int main()
{
    struct point p1 = {.x = 10, .y = 20}; //using designated initializers.

    printf("Point p1: (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```
Initialization using compound literals (C99 and later).
```c    
struct point {
    int x;
    int y;
};
int main()
{
    struct point p1 = (struct point){10, 20}; //using compound literals.

    printf("Point p1: (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```

Initalizing zero values for all members of a structure.
```c
struct point {
    int x;
    int y;
};
int main()
{
    struct point p1 = {0}; // All members initialized to zero.

    printf("Point p1: (%d, %d)\n", p1.x, p1.y);
    return 0;
}
```
## size of a structure
- The size of a structure is the sum of the sizes of its members, plus any padding added by the compiler for alignment purposes.
ways of structure packing:
- `#pragma pack(n)` - where `n` is the alignment boundary (e.g., 1, 2, 4, 8, etc.).
- `__attribute__((packed))` - GCC specific attribute to pack the structure without padding.

## copy structure

Copying a structure variable to another structure variable is done using the assignment operator. this method of copying is called "shallow copy".

```c    
struct point {
    int x;
    int y;
};
int main()
{
    struct point p1 = {10, 20};
    struct point p2;

    p2 = p1; // Copying structure p1 to p2.

    printf("Point p2: (%d, %d)\n", p2.x, p2.y);
    return 0;
}
```
## Passing structures to functions
- Structures can be passed to functions by value or by reference.
- When passed by value, a copy of the structure is made, and changes to the copy do not affect the original structure.
- When passed by reference, a pointer to the structure is passed, allowing the function to modify the original structure.
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
// Function to modify a structure by value.

void modifyByValue(struct point p) {
    p.x = 100; // This change will not affect the original structure.
    p.y = 200;
}
// Function to modify a structure by reference.
void modifyByReference(struct point *p) {
    p->x = 100; // This change will affect the original structure.
    p->y = 200;
}
int main() {
    struct point p1 = {10, 20};

    // Passing by value
    modifyByValue(p1);
    printf("After modifyByValue: (%d, %d)\n", p1.x, p1.y); // Original structure remains unchanged.
    //output (10, 20)

    // Passing by reference
    modifyByReference(&p1);
    printf("After modifyByReference: (%d, %d)\n", p1.x, p1.y); // Original structure is modified.
    //output (100, 200)
    return 0;
}
```


## typedef's for structures

- typedef is used to create an alias for an existing data type.

```c
// Definition of a new type with typedef of a structure.
typedef struct structName{
    //member definition
}typedef_name;
//or
struct structName{
    //member definition
};
typedef struct structName typedef_name;

// Example of typedef of a structure.
typedef struct { 
    char brandName[];
    char *model;
    int numDoors;
} CAR;

// Declaration of the variable of the structure.
CAR myCar;
CAR myCar[10];
CAR *pCar;

// Initialization  of a structure.
myCar = { "Suzuki", "Cleo", 5 };
```
```c
typedef struct{
    int val_1;
    int val_2;
} TAG_T;

void func_1(TAG_T s1){ // Passing by value.
    s1.val_1 = .... ;
}
// or...
void func_1(TAG_T * pS1){
    pS1->val_1 = .... ;
} //passing by reference.
```


## Nested structures.

- Embedded structure nesting: the structure is defined inside another structure.
- Separate structure nesting: the structure is defined separately and then used as a member of another structure.

```c
#include <stdio.h>

// Child structure declaration
struct child {
    int x;
    char c;
};

// Parent structure declaration
struct parent {
    int a;
    struct child b;
};

int main() {
    struct parent p = { 25, 195, 'A' };

    // Accessing and printing nested members
    printf("p.a = %d\n", p.a);
    printf("p.b.x = %d\n", p.b.x);
    printf("p.b.c = %c", p.b.c);
    return 0;
}
```

```c
// With struct and variable declaration.
struct car{
    char brandName[];
    int numDoors;
    struct {
        uint8_t day;
        uint8_t month;
        uint16_t year;
    } license_plate_date;
};

// Variable of struct declaration and accessing the nested members of
// the nested structure.
car car_s;
car_s.license_plate_date.day = 1;


// With TypeDef.
typedef struct{
    uint8_t day;
    uint8_t month;
    uint16_t year;
} DATE_T;


typedef struct{
    char brandName[];
    int numDoors;
    DATE license_plate_date;
} CAR_T;

CAR_T car_s;
CAR_T *pCar_s;

// Accessing the nested structure member.
car_s.license_plate_date.day   = 1;
pCar_s->license_plate_date.day = 1;
```

## Self referencing structures.

The self-referential structures are those structures that contain references to the same type as themselves i.e. they contain a member of the type pointer pointing to the same structure type.

Example:
```c
struct str {
    int mem1;

  	// Reference to the same type
   struct str* next;
};
```
Such kinds of structures are used in different data structures such as to define the nodes of linked lists, trees, etc.

```c
// IMPORTANT NOTE: You can only do self referencing structures
//                 with struct and not with typedef.
//                 But you can use typedef in the following way.
// The reasons because struct permits self referencing structures
// is because you can create a pointer to a type that doesn't exists
// yet, this is called "forward referencing"and in a typedef that 
// isn't permitted.

struct node{
    int val;
    struct node *pNext;
};

typedef struct node NODE_T;

// Other way of doing the typedef, would be...
    
    typedef struct node{
        int val;
        struct node *pNext;
    } NODE_T;
  
// Or you can also have two structures referencing each other.

struct node_A{
    int val;
    struct node_B *pNext_node_B;
};

struct node_B{
    int val;
    struct node_A *pNext_node_A;
};

typedef struct node_A NODE_A_T;
typedef struct node_B NODE_B_T;

// Personal note: See this with more attention regarding the typedef's.
```
## Structure pointer
- a pointer to a structure is a variable that holds the address of a structure. (->)arrow operator is used to access the members of a structure through a pointer.

```c
#include <stdio.h>

// Structure declaration
struct Point {
    int x, y;
};

int main() {
    struct Point p = { 1, 2 };

    // ptr is a pointer to structure p
    struct Point* ptr = &p;

    // Accessing structure members using structure pointer
    printf("%d %d", ptr->x, ptr->y);

    return 0;
}
```
# Bit Fields

struct date{
    uint32_t day:5;
    uint32_t month:4;
    uint32_t year:11;
}

typedef struct date DATE_T;

# Unions

```c
typedef union{
    struct{
        int a1;
        int a2;
    } s;
    float j;
    double k;
} UNION_NUM;

UNION_NUM value;

value.s.a1 = 1;     // Only one of those values exists in memory
value.j    = 4;     // at the same time. 
value.k    = 8;
```

# Dynamic memory allocation on the Heap 
- stdlib.h library provides functions for dynamic memory allocation.
- Memory is allocated on the heap, which allows for dynamic memory management during runtime.

1. malloc()  - Allocates one block of memory with a specific size, and returns a pointer to the first address of the block.
2. calloc()  - Allocates one or more memory blocks of specified size and initializes them to 0 (zero).
3. realloc() - Allows you to change the size of a previous allocated memory block.  
4. free()    - Free the memory allocated by a previous malloc(), calloc() or realloc(). 

- malloc() and calloc() return a pointer to the allocated memory block, or NULL if the allocation fails. it will be void pointer, so you need to cast it to the appropriate type.

```c
// malloc - Allocates a memory block for 100 int's.
int * ptr_1 = (int *) malloc(100 * sizeof(int));
if (ptr_1 == NULL) // dereferencing a NULL pointer will cause a segmentation fault.
   printf("Error in malloc().");

// calloc - Allocates a memory block for 100 int's.
int * ptr_2 = (int *) calloc(100, sizeof(int));
if (ptr_2 == NULL)
   printf("Error in calloc().");

// realloc - Reallocates memory of a new size, the new size is not initialized.
unsigned int newSize = 200;
int * ptr_3 = realloc(ptr_1, newSize);
if (ptr_3 == NULL)
   printf("Error in realloc().");

// free - Free's the memory previously allocated.
free(ptr_1);
free(ptr_2);
free(ptr_3);

```

[👉 Click for more details](./memory.md)
---

## Types of includes

- The include between < > is a standard C include, searched in the 'system' directory.
- #include <stdio.h>

- The include between "  " is a C include from my program, searched in the programs directory or similar.
- #include "main.h"

## Keyword static

1. In a file, outside of a function means that the variable scope is only the file, it's only visible in the file. Used to restrict access to the variable from outside of the file.

2. Inside a function, means that the variable is created in the first call to the function and it exists and maintains and can update the value in subsequent calls of the function.

---

## Enumerations (enum)
- user defined data type that consists of a set of named integer constants.
```c
enum color {
    RED,
    GREEN,
    BLUE
};
enum color myColor = RED; // myColor is of type enum color and initialized to RED.
```

# File handling

```c
#include <stddef.h>
#include <stdio.h>

// Open a file.

"r"  - Open text file for reading.
"w"  - Open text file for writing. If the file already
       exists it truncates to zero length. Begin of the file.
"a"  - Open the file in append mode. You write at the end of
       the file.
"r+" - Open the file for reading and for writing. File position
       at the beginning of the file. 
"w+" - Creates a new text file for reading and for writing.
       If the file already exists it will truncate the file
       to zero length.
"a+" - Open an existing file or create a new one in append mode.
       You can read data anywhere but you can only add data in
       the end of the file.  
"rb"  - Open file for reading in binary.
"wb"  - Open file for writing in binary.


FILE * open_file(char *filename){
    FILE *fp;  // file pointer

    fp = fopen(filename, "r");   // r - for read
    if (fp == NULL)
        fprintf( stderr, "Error opening file.\n");
    return fp;
}

// Read and write from / to a file.

getc()   - Macro to read one char.
fgetc()  - Function that does the same as getc().
putc()   - Macro to writes one char.
fputc()  - Function that does the same as putc().
ungetc() - Pushes a character onto a file strem.

fflush() - Flushes the buffer.
ftell()  - Returns the current file position.

fprintf() - Exactly as printf, but to a file.
fscanf()  - Exactly as scanf, but from a file.

clearerr() - Resets the error and end-of-file indicator.
feof()     - Checks if EOF (End Of File) indicator was reached
             in previous read operation. 
ferror()   - Returns the integer error code, while reading
             or writing to a stream.

tmpfile() - Creates a temporary binary file.
remove()  - Removes a file from file system.
rename()  - Renames a file in the file system.
setbuf()  - Alter the buffer proprieties for a file.
```

```c

#include <stddef.h>
#include <stdio.h>

#define FAIL    0
#define SUCCESS 1

int copy_text_or_bin_file(char * input_file, char * output_file){
    FILE *fp1, *fp2;
    if ( (fp1 = fopen(input_file, "rb")) == NULL )
        return FAIL;
    if ( (fp2 = fopen(output_file, "wb")) == NULL )
    {
        fclose( fp1 );
        return FAIL;
    }
    while (!feof(fp1))
        putc(getc( fp1), fp2);
    fclose(fp1);
    fclose(fp2);
    return SUCCESS;
}


// Line oriented reading and writing.
fgets() - Reads one line from a file stream.
fputs() - Writes one line to a file stream.

example equal to the previous but copies line by line:

#define LINESIZE 100

char line[LINESIZE];

while( fgets( line, LINESIZE - 1, fp1 ) != NULL )
   fputs( line, fp2)

// Random access.

fseek() - Moves the file pointer inside the file
          to a random position. 

// Read / write one block at a time.

fread()  - Read one block at a time.
fwrite() - Writes one block at a time.


// Close a file.

fclose(fp)

```

## Function pointers

```c
void led_sequence(void);

int main(void){

    // Declaration of a single function pointer.
    void (* const arr_pointers_to_functions) (void) = led_sequence;

    // Call the function, using the right function pointer.
    (*(arr_pointers_to_functions)) ();

    // Declaration of an array of pointers to functions with a parameter void and returning a void.
    // Fill it with functions.
    void (* const arr_pointers_to_functions[]) (void) = {led_sequence, NULL, NULL, NULL};

    int menu_option = 0;

    // Call the function, using the right function pointer.
    (*(arr_pointers_to_functions[menu_option])) ();
}

void led_sequence(void){

   // ...
}
```

## C99 - stdint.h - Primitive fixed size types

```c
// Include that has the primitive fixed size types.
#include <stdint.h>

    // Types
    uint8_t    int8_t
    uint16_t   int16_t
    uint32_t   int32_t
    uint64_t   int64_t
```

## Preprocessor Macros

- #define is used to define a preprocessor macro.
- #undef is used to undefine a preprocessor macro.
- #include is used to include a header file.
- #ifdef, #ifndef, #if, #else, #elif, #endif are used for conditional compilation.
- #pragma is used to give special instructions to the compiler.



1. Always put the Macro inside parenthesis, so that the order of execution is always the same.

```c
#define RCC_APB2_ENR_ADDR      (RCC_BASE_ADDR + RCC_APB2_ENR_OFFSET)

```

2. Always protect the **.h** file content against multiple inclusion on the same .C file.

```c
#ifndef REG_BASE_ADDRESSES_H
#define REG_BASE_ADDRESSES_H

// content of the include file reg_base_addresses.h

#endif

```

## Technique for defining more than one statement in a preprocessor Macro.

```c
// This is a technique in C programming to execute multiple C statements using a single C macro.
// Placing a do__while loop that only executes once. 
#define GPIO_G_SET()      do{ (GPIOA->MODE |= (1 << 0)); (GPIOA->MODE &= ~(1 << 0)); }while(0)
```
