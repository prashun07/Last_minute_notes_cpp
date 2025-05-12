### Memory in C and C++

**Memory Layout**
The memory layout of a program refers to how the program’s data is stored in the computer memory during its execution. 
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20250122155858092295/Memory-Layout-of-C-Program.webp" alt="Memory Layout" width="600"/>

**Memory Segments**
The memory layout of a C/C++ program is divided into several segments, each serving a specific purpose. Here’s a breakdown of the main segments:
| Segment | Stores                            | Lifetime          | Access Type |
| ------- | --------------------------------- | ----------------- | ----------- |
| Text    | Compiled machine code & instructions| Entire program  | Read-only   |
| Data    | Initialized globals/statics       | Entire program    | Read/Write  |
| BSS     | Uninitialized globals/statics     | Entire program    | Read/Write  |
| Heap    | Dynamically allocated memory      | Until freed       | Read/Write  |
| Stack   | Local variables, return addresses | Per function call | Read/Write  |

---

### Dynamic Memory Allocation
Dynamic memory allocation is a powerful feature in C and C++ that allows you to allocate memory at runtime. This is particularly useful when the size of the data structure is not known at compile time. 

**malloc()**
The malloc() (stands for memory allocation) function is used to allocate a single block of contiguous memory on the heap at runtime. The memory allocated by malloc() is uninitialized, meaning it contains garbage values.
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20250305233229499334/Malloc-function-in-c.webp" alt="Malloc Function" width="600"/>

- if no memory is available, malloc() returns NULL.
- malloc() returns a pointer of type void*, which can be cast to any other pointer type.

**calloc()**
The calloc() (stands for contiguous allocation) function is used to allocate memory for an array of elements, initializing all bytes in the allocated storage to zero. It is often used when you need memory with default value zero.

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20250305233554088828/calloc-function-in-c.webp" alt="Calloc Function" width="600"/>
- If no memory is available, calloc() returns NULL.
- calloc() returns a pointer of type void*, which can be cast to any other pointer type.
- The first argument is the number of elements, and the second argument is the size of each element.

**realloc()**
The realloc() function is used to resize a previously allocated memory block. It can either increase or decrease the size of the memory block. If the new size is larger, the additional memory is uninitialized. If the new size is smaller, the excess memory is freed.

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20250305235104974360/realloc-function-in-c.webp" alt="Realloc Function" width="600"/>
- if no memory is available, realloc() returns NULL and the original memory block remains unchanged.
- Don't overwrite the pointer returned by realloc() without checking for NULL, as it may lead to memory leaks.

**free()**
The free() function is used to deallocate memory that was previously allocated using malloc(), calloc(), or realloc(). It is important to free dynamically allocated memory to avoid memory leaks.
- free() does not return any value. It simply releases the memory back to the system.
- After freeing memory, the pointer becomes a dangling pointer. It is a good practice to set the pointer to NULL after freeing it.
- Attempting to access memory after it has been freed results in undefined behavior.

**Issues Assciated with Dynamic Memory Allocation**
Memory Leaks: Failing to free dynamically allocated memory leads to memory leaks, exhausting system resources.
Dangling Pointers: Using a pointer after freeing its memory can cause undefined behavior or crashes.
Fragmentation: Repeated allocations and deallocations can fragment memory, causing inefficient use of heap space.
Allocation Failures: If memory allocation fails, the program may crash unless the error is handled properly.

---

### Memory Management in C++
C++ provides several features and techniques for memory management, including constructors, destructors, smart pointers, and the RAII (Resource Acquisition Is Initialization) principle. These features help manage memory more effectively and reduce the risk of memory leaks and dangling pointers.

**new operator**
The new operator requests for the allocation of the block of memory of the given size of type on the Free Store. If sufficient memory is available, a new operator initializes the memory to the default value according to its type and returns the address to this newly allocated memory.
- The new operator returns a pointer of type T* (where T is the type of the object being allocated).
- If the allocation fails, it throws a std::bad_alloc exception.
- The new operator can also be used to allocate arrays of objects.

> new data_type; // Example of allocating a single object
> new data_type[array_size]; // Example of allocating an array of objects

**delete operator**
The delete operator is used to deallocate memory that was previously allocated using the new operator. It frees the memory and calls the destructor for the object if applicable.

```cpp
int *ptr = new int; // Allocating a single integer
*ptr = 10; // Assigning value
int *arr = new int[5]; // Allocating an array of 5 integers
for (int i = 0; i < 5; ++i) {
    arr[i] = i * 10; // Assigning values
}
delete ptr; // Deallocating single integer
delete[] arr; // Deallocating array of integers
```
- C++ supports C style memory management functions like malloc(), calloc(), realloc(), and free() for compatibility, but it is recommended to use new and delete for C++ programs. 
- new and free() are not interchangeable, same for new and malloc().
- new calls constructors, while delete calls destructors. malloc() and free() do not call constructors or destructors.
- new and delete are type-safe, while malloc() and free() are not.
- new and delete can be overloaded, while malloc() and free() cannot.

**Memory Leak**
Memory leak occurs when the program allocates memory but forgets to deallocate it. This can cause wasted memory and eventually lead to program crashes.

**Smart Pointers**
Smart pointer is wrapper class over a pointer that acts as a pointer but automatically manages the memory it points to. 
- It ensures that memory is properly deallocated when no longer needed, preventing memory leaks. 
- It is a part of <memory> header file.

**Types of Smart Pointers**
auto_ptr :  Deprecated in C++11, use std::unique_ptr instead.
unique_ptr : unique_ptr stores one pointer only at a time. We cannot copy unique_ptr, only transfer ownership of the object to another unique_ptr using the move() method.
```cpp
    unique_ptr<Rectangle> P1(new Rectangle(10, 5));
    cout << P1->area() << endl; //suppose print 50

    unique_ptr<Rectangle> P2;

    // Copy the address of P1 into p2
    P2 = move(P1);
    cout << P2->area(); //here it will be also 50
```
shared_ptr : By using shared_ptr, more than one pointer can point to same object at a time, and it will maintain a reference counter using the use_count() method.
```cpp
    shared_ptr<Rectangle> P1(new Rectangle(10, 5));
    cout << P1->area() << endl; //suppose print 50

    shared_ptr<Rectangle> P2 = P1;
    cout << P2->area(); //here it will be also 50
```

weak_ptr : it will holds a non-owning reference to an object and is used to break the circular reference between shared_ptr. It does not increase the reference count of the object. 

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20250402152221597690/weak-pointer-in-cpp.webp" alt="Weak Pointer" width="600"/>

```cpp
    shared_ptr<Rectangle> P1(new Rectangle(10, 5));
    cout << P1->area() << endl; //suppose print 50

    weak_ptr<Rectangle> P2 = P1;
    cout << P2.lock()->area(); //here it will be also 50
```


