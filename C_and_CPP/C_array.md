### C-style Arrays
An array in C is a fixed-size collection of similar data items stored in contiguous memory locations. 
- contiguous memory locations means that the elements of the array are stored one after another in memory.
- The size of the array must be known at compile time.
- The size of the array is fixed and cannot be changed after it is declared.
- The array can store elements of any data type, including primitive types (like int, char, float) and user-defined types (like structs).
- The array can be initialized at the time of declaration or later in the code.
- The array can be passed to functions as arguments, and the function can modify the elements of the array.
- The array can be accessed using the index operator [].

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20230302091959/Arrays-in-C.png" alt="Arrays in C" width="600"/>    

> data_type array_name[size];

**Array initialization**
```c
data_type array_name[size] = {value1, value2, ..., valueN};
// we can also skip size 
data_type array_name[] = {value1, value2, ..., valueN};
// we can also initialize only some elements
data_type array_name[size] = {value1, value2, ..., valueN, 0}; // rest will be initialized to 0
data_type array_name[size] = {0}; // all elements will be initialized to 0

```

**Passing arrays to functions**
In C, arrays are passed to functions using pointers, as the array name decays to a pointer to the first element. There are three common methods:
- Unsized array notation, where the array is passed as a pointer without specifying its size.
- Sized array notation, where the size is included in the declaration for clarity, but it’s still passed as a pointer.
- Pointer notation, where the array is explicitly passed as a pointer, often along with its size.

```c
// Functions that take array as argument
// All three methods are equivalent in terms of functionality, but the syntax differs slightly.
// All are pointers because arrays decay to pointers when passed to functions.
// There is no pass by value for arrays in C, only pass by reference.
// The size of the array is not passed to the function, so it must be known or passed separately.
void sized_array_notation(int arr[n]) {}

void unsized_array_notation(int arr[]) {}

void pointer_notation(int* arr, int n ) {
 // Additional logic can be added here if needed
}
```

**Pointers notation**
```c
#include<stdio.h> 

void print(int * arr, int n)
{
    for(int i=0;i<n;i++)
    {
        printf("\n %d",arr[i]);
    }
}
void fun(int *arr,int n)
{
    print(arr,n);
    arr[3]=10;  // changing the value of 4th element
    print(arr,n);
}
int main()
{
    int arr[]={1,2,3,4,5};
    int n=sizeof(arr)/sizeof(int);
    fun(arr,n); //pass by reference 
    return 0;
}
```

**Output**
> 1 2 3 4 5
> 1 2 3 10 5

---
