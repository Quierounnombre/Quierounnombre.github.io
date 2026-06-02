---
title: "Memory leaks and C"
published: false
layout: single
tags: [Low level, software]
---

In C, a memory leak is a death sentence to any serious program.

Today we will dive into all the specifics:

1. What exactly is a leak?
2. Why we have leaks?
3. How is a leak in code?
4. Modern leaks in today software.

## What exactly is a leak?

You leak memory when you lose a reference to a pointer, so you are unable to free it, what is exactly a pointer, (this link)

This could be done in a thousand different ways, here we will provide two examples.

### Overwriting a ptr

```c
int main(void)
{
    char *ptr;

    ptr = malloc(10); // Ignore the size
    ptr = null; //LEAK
}

```

this is because ptr that holds the value of the malloc has been overwritten, and we do not have a copy of it, and thus we can recover that memory and free it.

```c
int main(void)
{
    char *ptr;
    char *fail_back;

    ptr = malloc(10); // Ignore the size
    fail_back = ptr; // STORED
    ptr = null;

    //LEAKS?
}

```

this does not leak*

The data is being saved in fail_back and this not leaked, since we can free the memory direction.

### Not freeing memory before an exit.

```c
int main(void)
{
    char *ptr;
    char *fail_back;

    ptr = malloc(10); // Ignore the size
    fail_back = ptr; //STORED
    ptr = null;
    free(fail_back); //NO LEAKS, at end of execution
}

```

the previous code had a leak, one that in todays computers is neglilable, since modern OS just free the memory at the end of the execution for you, but when a program ends its life time, it will try to free all memory
that is still reachable, in the previous case this would throw an error when using a tool like valgrind, but you are not leaking actual memory, at least no in today pcs.

## Why we have leaks in the first place?

It is cheap, RAM is cheap, thats the whole point.

Modern developers hear about stack and heap like a mythic creature that just is.

Creating a big stack, is hard, expensive, and limited to pc architecture and psychis.

Meanwhile, creating a big heap, is just creating a lot of bits and cells, and just pasting them togheter.

Due to this constraint, we use heap(or RAM) in our day to day basis, just to save cost.

## How to avoid leaking in modern software?

### Overwriting

As we discussed previously, just dont overwrite a ptr, you just need 1 reference to the pointer at all times to not lose memory.

### 
