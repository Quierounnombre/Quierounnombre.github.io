---
title: "Memory leaks and C"
published: true
layout: single
tags: [Low level, software]
---

In C, a memory leak is a death sentence to any serious program.

Today we will dive into all the specifics:

1. What exactly is a leak?
2. Why we have leaks?
3. How is a leak in code?

## What exactly is a leak?

You leak memory when you lose a reference to a memory address that you have allocated, so you are unable to free it.

This could be done in a thousand different ways, later we will see many examples.

## Why we have leaks in the first place?

Modern developers hear about stack and heap like a mythic creature that just is.

Creating a big stack is limited to pc architecture. each program as an independent stack, and each stack size is fixed.
this means, that the stack shouldn't be use to store an element of size N, and even if so, we have the scope problem, that is
you can't return a stack address, in C code.

```c
char *create_string(uint size)
{
    char str[size]; //Stack addrs
    return str;
}

int main(void)
{
    //This would do absolutly nothing
    str = create_string(100);
}
```

This is why we have a heap, since the stack just dissapears when it leaves the scope.

This means that we the developers have the control, and thus we need to take into account memory allocation, even if
you are working with a language with a Garbage collector(GC) like `go`, `c#`, `java`... The memory is still being allocated, just you
are not in control, but that dosen't means that you are free from its constrains.

## How does a leak look in code?

### Overwriting a ptr

```c
int main(void)
{
    char *ptr;

    ptr = malloc(10); // Ignore the size
    ptr = NULL; //LEAK
}

```

this is because `ptr` that holds the value of the malloc has been overwritten, and we do not have a copy of it, and thus we can't recover that memory and free it.

```c
int main(void)
{
    char *ptr;
    char *fail_back;

    ptr = malloc(10); // Ignore the size
    fail_back = ptr; // STORED
    ptr = NULL;

    //LEAKS?
}

```

this does not leak*

The data is being saved in fail_back and this not leaked, since we can free the memory address.

### Not freeing memory before an exit.

```c
int main(void)
{
    char *ptr;
    char *fail_back;

    ptr = malloc(10); // Ignore the size
    fail_back = ptr; //STORED
    ptr = NULL;
    free(fail_back); //NO LEAKS, at end of execution
}

```

the previous code had a leak, one that in todays computers is negligible, since modern OS just free the memory at the end of the execution for you, but when a program ends its life time, it will try to free all memory
that is still reachable, in the previous case this would throw an error when using a tool like valgrind, but you are not leaking actual memory, at least no in today pcs.

### Not freeing at all.

This is not a leak with the formal definition in hand, but having more resources than necesary and doing nothing with them has the same issues has a normal leak, reducing the machine capabilities.

```c
int main(void)
{
    char    *str;
    char    *tmp;
    int     i;
    char    example[] = "This is a bad practice\n";

    str = malloc(100);
    i = 0;
    while (i <= 10000)
    {
        strcpy(str + i, example);
        i += strlen(example);
        tmp = malloc(sizeof(str) + sizeof(example));
        strcpy(tmp, str);
        free(str); //We have already realocated the memory above.
        str = tmp;
    }
    *(str + i) = '\0';
    //Here you can print it if you want, u2u
}
```

Not only the code above is badly abstracted and written, it is also a bad practice, as you see, the `str` keep getting resized time and time again, until it reaches a arbitrary point, this is a very bad practice.

But bad practices aside, this is a oversimplify example of a ton of resources wasted, there are other examples like not freeing bullets in a shooter once they leave the scene, but I wanted to stick with `C`.

This is a basic guide on how to not lose your machine resources, hope it helped, and text me in linkeding if you didn't understand something!
