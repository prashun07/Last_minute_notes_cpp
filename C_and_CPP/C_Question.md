### Implement a generic dynamic array in C.

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

// enum to make generic
typedef enum {
    TYPE_INT,
    TYPE_CHAR
}DATATYPE;

//struct will contain void data pointer and data type
typedef struct {
    void *data;
    DATATYPE type;
}Element;

//will contain array pointer of element structure and size and capacity
typedef struct vector {
    Element *arr;
    int size;
    int capacity;
    
    // to make function pointer
    void (*init)(struct vector *);
    void (*push_back)(struct vector* , DATATYPE , void *);
    void (*resize)(struct vector* , int);
    void* (*get)(struct vector* , int);
}vector;

//work like a constructor and initialize vector
void vector_init(vector *v)
{
    v->size=0;
    v->capacity=8;
    v->arr=(Element*)malloc(sizeof(Element)*v->capacity);
}
//will resize the size of vector
void vector_resize(vector *v, int new_size)
{
    //commented logic is brute force way of doing it
    // Element *new_arr;
    
    // new_arr=(Element*)malloc(sizeof(Element)*v->capacity);
    
    // for(int i=0;i<size;i++)
    // {
    //     new_arr[i]=v->arr[i];
    // }
    // free(v->arr);
    // v->arr=new_arr;
    
    v->arr= (Element*)realloc(v->arr, sizeof(Element) * new_size);
    v->capacity = new_size;
}
//push back function
void vector_push_back(vector *v, DATATYPE type, void *data )
{
    //check if size is equals to capacity
    if(v->size >= v->capacity)
    {
        v->capacity=2*v->size; //double the previous size as per dynamic array behavior
        vector_resize(v,v->size);//call resize function
    }
    Element e;
    e.type=type;
    //copy data from some void location to proper datatype location
    switch (type){
        case TYPE_INT: 
        e.data=malloc(sizeof(int));
        memcpy(e.data,data,sizeof(int));
        break;
        case TYPE_CHAR:
        e.data=malloc(sizeof(char));
        memcpy(e.data,data,sizeof(char));
        break;
    }
    
    v->arr[v->size++]=e; //assign the value and increase size by 1
}
//get function also void pointer is used
void *vector_get(vector *v ,int index)
{
    if(index < 0 || index >= v->size)
    {
        return NULL; //when index goes out of bound
    }
    
    return v->arr[index].data; //return value
}

//assign functions to the function pointer
void create_vector(vector *v)
{
    v->init=vector_init;
    v->get=vector_get;
    v->push_back=vector_push_back;
    v->resize=vector_resize;
    
    vector_init(v);
}
//free up the allocated memory to avoid data leaks
void vector_free(vector *v)
{
    for(int i=0;i<v->size;i++)
    {
        free(v->arr[i].data);
    }
    free(v->arr);
}
int main()
{
    vector v; //create v instance of vector
    
    create_vector(&v); //pass address of v to initialize vector address
    
    int i=3;
    char c='A';
    
    v.resize(&v,3); //resize
    v.push_back(&v,TYPE_INT, &i);
    v.push_back(&v,TYPE_CHAR, &c);
    printf("%d \n",*(int*)v.get(&v,0));
    printf("%c \n",*(char*)v.get(&v,1));
    
    vector_free(&v);
    return 0;
}
```
---
