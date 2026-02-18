
# Data Structures & Algorithms – Interview Notes

---

## 1. What is a Linked List? Types and Differences

A linked list is a linear data structure in which elements, called nodes, are stored non-contiguously in memory. Each node contains data and a pointer that references the next node in the sequence.

There are several types of linked lists. A singly linked list contains nodes where each node points only to the next node. A doubly linked list contains nodes that maintain two pointers: one to the next node and one to the previous node. A circular linked list is a variation where the last node points back to the first node instead of pointing to NULL.

The main difference between these types lies in traversal capability and memory usage. Doubly linked lists allow backward traversal but consume more memory due to the extra pointer. Circular lists eliminate the NULL termination condition.

---

## 2. Difference Between Array and Linked List. When to Use Which?

An array stores elements in contiguous memory locations, allowing direct indexing. A linked list stores elements in scattered memory locations connected by pointers.

Arrays provide constant-time access to elements using indexing, but insertion and deletion operations are expensive because elements must be shifted. Linked lists allow efficient insertion and deletion since only pointer adjustments are required, but accessing an element requires sequential traversal.

Arrays are preferred when fast access by index is required and the size is known. Linked lists are preferred when frequent insertions and deletions are expected and memory allocation needs to be dynamic.

---

## 3. How Does a Linked List Work? How to Insert and Delete Nodes?

A linked list works by storing data in nodes that point to the next node. The first node is called the head.

To insert a node at the beginning, a new node is created, its next pointer is set to the current head, and the head is updated to the new node.

To insert at the end, traverse until the last node and update its next pointer to the new node.

To delete a node, find the previous node of the node to be removed and update its next pointer to skip the target node. The memory for the deleted node must then be freed.

---

## 4. How to Traverse a Linked List and Count Elements?

Traversal starts from the head node and continues until the pointer becomes NULL. During traversal, a counter variable can be incremented to count the number of elements.

Traversal complexity is linear because each node must be visited once.

---

## 5. Difference Between Singly and Doubly Linked List

A singly linked list allows traversal in one direction only and uses one pointer per node. A doubly linked list allows traversal in both directions and uses two pointers per node.

Doubly linked lists simplify deletion operations because the previous node is directly accessible. However, they consume more memory and require extra pointer maintenance.

---

## 6. Difference Between Tree and Graph

A tree is a hierarchical data structure consisting of nodes connected by edges, with one root node and no cycles. Each node (except root) has exactly one parent.

A graph is a more general structure consisting of vertices and edges. It can contain cycles and may not have a root. Graphs can be directed or undirected.

Every tree is a graph, but not every graph is a tree.

---

## 7. Complexity of Searching in Binary Tree

In a balanced binary search tree, the time complexity of searching is O(log n). In the worst case, when the tree becomes skewed like a linked list, the complexity becomes O(n).

---

## 8. What is Stack and Queue? Difference Between Them

A stack is a linear data structure that follows the Last In First Out (LIFO) principle. The last element inserted is the first one removed.

A queue follows the First In First Out (FIFO) principle. The first element inserted is the first one removed.

The key difference lies in the order of data removal.

---

## 9. Real-Time Examples of Stack and Queue

A stack is used in function call management where the most recent function call is completed first. It is also used in undo operations in text editors.

A queue is used in printer job scheduling where the first document sent is printed first. It is also used in task scheduling and buffering systems.

---

## 10. Difference Between Binary Search and Linear Search

Linear search checks each element sequentially until the target is found. Its time complexity is O(n).

Binary search works only on sorted arrays. It repeatedly divides the search space into half by comparing the middle element with the target. Its time complexity is O(log n).

Binary search is significantly faster for large sorted datasets.

---

## 11. Searching Algorithms and Their Complexity

Linear search has time complexity O(n) and constant space complexity O(1).

Binary search has time complexity O(log n) and space complexity O(1) for iterative implementation.

Hash-based searching has average time complexity O(1) but worst-case O(n) due to collisions.

---

## 12. Sorting Algorithms and Their Complexity

Bubble sort has time complexity O(n²) and is inefficient for large datasets.

Insertion sort has average time complexity O(n²) but performs well for nearly sorted data.

Merge sort has time complexity O(n log n) and requires extra space O(n).

Quick sort has average time complexity O(n log n) and worst-case O(n²), but in practice it performs efficiently.

Heap sort guarantees O(n log n) time complexity with O(1) extra space.

---

## 13. What is Binary Search Tree and Its Advantages?

A Binary Search Tree (BST) is a binary tree in which the left child of a node contains values smaller than the parent and the right child contains values greater than the parent.

The main advantage of BST is efficient searching, insertion, and deletion operations with average complexity O(log n) if the tree is balanced.

---

## 14. Hashing and Its Use

Hashing is a technique that maps data to a fixed-size value using a hash function. The resulting value is used as an index in a hash table.

Hashing allows very fast data retrieval with average time complexity O(1). It is widely used in databases, symbol tables, and caching systems.

Collisions occur when different keys produce the same hash value. Techniques such as chaining or open addressing are used to resolve collisions.

---## 15. What is a Vector in C? How to Implement It? 
A vector in C is a dynamic array that can resize itself automatically when elements are added or removed. It provides random access to elements and can grow or shrink as needed.
To implement a vector, we can define a structure that contains a pointer to the array, its size, and its capacity. We can then implement functions for initialization, resizing, adding elements, and retrieving elements.
```c
#include <stdio.h>
#include <stdlib.h>
typedef struct {
    void *data;
    size_t size;
    size_t capacity;
} Vector;
void vector_init(Vector *v) {
    v->size = 0;
    v->capacity = 4; // initial capacity
    v->data = malloc(v->capacity * sizeof(void*));
}   
void vector_resize(Vector *v, size_t new_capacity) {
    v->data = realloc(v->data, new_capacity * sizeof(void*));
    v->capacity = new_capacity;
}
void vector_push_back(Vector *v, void *element) {
    if (v->size == v->capacity) {
        vector_resize(v, v->capacity * 2);
    }
    ((void**)v->data)[v->size++] = element;
}
void* vector_get(Vector *v, size_t index) {
    if (index < v->size) {        
        return ((void**)v->data)[index];
    }
    return NULL; // out of bounds
}
void vector_free(Vector *v) {
    free(v->data);
    v->size = 0;
    v->capacity = 0;            
}
```This implementation provides basic functionalities of a vector, including initialization, resizing, adding elements, and retrieving elements. The vector automatically resizes when the capacity is reached, ensuring efficient memory usage while maintaining fast access to elements.

# Advanced Data Structure Interview Notes

---

## 1. What is the difference between a Balanced Binary Tree and an Unbalanced Binary Tree?

A balanced binary tree is a tree in which the height difference between the left and right subtrees of any node is limited to a small value, typically one. This ensures that operations such as search, insertion, and deletion can be performed in logarithmic time.

An unbalanced binary tree does not enforce height constraints. In the worst case, it can degrade into a structure similar to a linked list, resulting in linear time complexity for operations.

Balanced trees such as AVL trees and Red-Black trees guarantee O(log n) performance, whereas unbalanced trees may degrade to O(n).

---

## 2. What is an AVL Tree?

An AVL tree is a self-balancing binary search tree in which the difference in heights of left and right subtrees (called the balance factor) is at most one for every node.

Whenever insertion or deletion disturbs the balance, tree rotations (left rotation, right rotation, or double rotation) are performed to restore balance.

AVL trees guarantee O(log n) time complexity for search, insertion, and deletion.

---

## 3. What is a Red-Black Tree?

A Red-Black tree is a self-balancing binary search tree with additional coloring properties to maintain balance.

Each node is either red or black, and the tree satisfies specific rules such as:

* The root is always black.
* Red nodes cannot have red children.
* Every path from root to leaf contains the same number of black nodes.

Red-Black trees are less strictly balanced than AVL trees but require fewer rotations, making them efficient for insertion-heavy workloads. They are used in many systems, including C++ STL maps and Linux kernel structures.

---

## 4. What is a Heap?

A heap is a complete binary tree that satisfies the heap property.

In a max heap, the parent node is always greater than or equal to its children. In a min heap, the parent is smaller than or equal to its children.

Heaps are commonly used to implement priority queues and support operations such as insertion and extraction in O(log n) time.

---

## 5. What is the Difference Between Heap and Stack?

Heap memory is dynamically allocated at runtime and is managed manually or by a garbage collector. Stack memory is automatically managed and follows a Last In First Out structure.

Heap as a data structure (binary heap) is different from heap as memory region. In data structures, heap refers to priority queue implementation.

---

## 6. What is Graph Traversal? Explain BFS and DFS.

Graph traversal is the process of visiting all nodes in a graph systematically.

Breadth-First Search (BFS) explores nodes level by level using a queue. It is useful for finding the shortest path in unweighted graphs.

Depth-First Search (DFS) explores nodes deeply before backtracking. It uses recursion or a stack.

Both have time complexity O(V + E), where V is vertices and E is edges.

---

## 7. What is a Directed and Undirected Graph?

In a directed graph, edges have direction, meaning a connection from A to B does not imply a connection from B to A.

In an undirected graph, edges are bidirectional.

Directed graphs are used in dependency modeling, while undirected graphs represent mutual relationships.

---

## 8. What is a Cycle in a Graph?

A cycle occurs when a path starts and ends at the same vertex without repeating edges.

In directed graphs, cycle detection can be done using DFS with recursion stack. In undirected graphs, cycle detection is often done using Union-Find.

---

## 9. What is a Trie?

A Trie is a tree-like data structure used to store strings efficiently. Each node represents a character, and paths represent words.

Tries are commonly used in autocomplete systems and dictionary implementations.

Search complexity depends on the length of the word, not the number of stored words.

---

## 10. What is a Segment Tree?

A segment tree is a binary tree used for range queries and updates on arrays.

It allows efficient computation of range sums, minimums, or maximums in O(log n) time.

It is widely used in competitive programming and real-time analytics.

---

## 11. What is Dynamic Programming?

Dynamic programming is a technique used to solve problems by breaking them into smaller overlapping subproblems and storing their results to avoid recomputation.

It typically uses memoization (top-down) or tabulation (bottom-up).

Common examples include Fibonacci sequence, knapsack problem, and shortest path problems.

---

## 12. What is a Hash Table Collision? How to Handle It?

A collision occurs when two keys map to the same index in a hash table.

Collision resolution techniques include:

* Chaining (linked list at each bucket)
* Open addressing (linear probing, quadratic probing)
* Double hashing

Good hash functions minimize collisions.

---

## 13. What is Big-O Notation?

Big-O notation describes the upper bound of time or space complexity of an algorithm.

It represents the worst-case growth rate of execution time relative to input size.

For example:

* O(1) constant time
* O(log n) logarithmic
* O(n) linear
* O(n²) quadratic

---

## 14. What is Amortized Analysis?

Amortized analysis calculates the average time per operation over a sequence of operations.

For example, dynamic array resizing is expensive occasionally but averages to O(1) per insertion over many insertions.

---

## 15. What is a Priority Queue?

A priority queue is a data structure where each element has a priority, and elements are removed based on priority rather than insertion order.

It is typically implemented using a binary heap.

Operations such as insertion and removal take O(log n) time.

---