---
share: true
catagory: "Basic"
tags:
    - computer_sience
---

# 1. Problem

Tell about `array`, what is it?

# 2. Solve

Array is a continue space of memory

Memory it self is a problem, every language do different thing to allocating memory, which affecting how array work under the hood.

By focusing on C language, we can have a deeper talk of array concept
## 2.1. C language array and memory allocation

This being compile with this flag (using vim config and vim make)

```vim
set makeprg=gcc\ -fdiagnostics-plain-output\ -g\ -I.\ -o\ dist/%:r\ %
```

> If you using -O3, then this meh, not even work. As they change the stack layout to optimize.

```c
#include <stdio.h>
#include <string.h>

char hello[13] = "Hello world!";
long long arr[4] = {1, 1, 1, 1};
int arr_2[8] = {4, 4, 4, 4, 4, 4, 4, 4};

int printHex(unsigned int* arr, int length){
    printf("Array with %d length:\n", length);
    for (int i = 0; i < length; i++) {
        printf("%08x", ((unsigned int*)arr)[i]);
        if (i != length - 1) printf(", ");
    }
    printf("\n");

    return 0;
}

int main(){
    printf("size of arr = %lu\n", sizeof(arr));
    printf("size of arr element = %lu\n", sizeof(arr[0]));
    printHex((unsigned int*)arr, sizeof(arr)/sizeof(unsigned int));


    unsigned int *arr_view = (unsigned int *) &arr;
    unsigned int arr_length = (unsigned int) (sizeof(arr)/sizeof(unsigned int));
    printHex(arr_view, arr_length);

    arr_view[1] = (unsigned int)~0;
    printHex(arr_view, arr_length);
    printf("New value of arr[0] = %lld\n\n\n", arr[0]);


    printf("String: %s with length of %lu, memory size %lu\n", hello, strlen(hello), sizeof(hello) );
    for (int i = 0; i <= strlen(hello); i++) {
        printf("%2x ", hello[i]);
    }
    printf("\n");
    for (int i = 0; i <= strlen(hello); i++) {
        printf("%2c ", hello[i]);
    }
    printf("\n%s\n",hello);
    printHex((unsigned int*) hello, strlen(hello)/(4*sizeof(char)) + 1);
    printHex((unsigned int*) hello, 40);

    printf("\n");
    printf("\n");
    printf("\n");

    // The continue of the stack alocation
    printf("We working with arr_2 (all init with 4)\n");
    printHex((unsigned int*)arr_2, sizeof(arr_2)/sizeof(unsigned int));
    
    // Change arr, but arr_2 will be affected
    for (int i = 4; i < 8; i++) {
        arr[i] = 2;
    }

    printHex((unsigned int*)arr_2, sizeof(arr_2)/sizeof(unsigned int));

    return 0;
}
```

```stdout
size of arr = 32
size of arr element = 8
Array with 8 length:
00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
Array with 8 length:
00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
Array with 8 length:
00000001, ffffffff, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
New value of arr[0] = -4294967295


String: Hello world! with length of 12, memory size 13
48 65 6c 6c 6f 20 77 6f 72 6c 64 21  0
 H  e  l  l  o     w  o  r  l  d  !
Hello world!
Array with 4 length:
6c6c6548, 6f77206f, 21646c72, 00000000
Array with 40 length:
6c6c6548, 6f77206f, 21646c72, 00000000, 00000000, 00000000, 00000000, 00000000, 00000001, ffffffff, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000



We working with arr_2 (all init with 4)
Array with 8 length:
00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004
Array with 8 length:
00000002, 00000000, 00000002, 00000000, 00000002, 00000000, 00000002, 00000000
```

## 2.2. What happening here?

### 2.2.1. Created an array variable:

```c
char hello[13] = "Hello world!";
long long arr[4] = {1, 1, 1, 1};
int arr_2[8] = {4, 4, 4, 4, 4, 4, 4, 4};
```

- `hello` is an array of `char` type, that being initiation with `"Hello world!"`
- `arr` is an array of `long long` (8 bytes number) type, that all element initiation with 1;
- `arr_2` is an array of `int` (4 bytes number) type, that all element initiation with 4;

### 2.2.2. Helper function that printout the exact value (in hex) of a memory

```c
int printHex(unsigned int* arr, int length){
    printf("Array with %d length:\n", length);
    for (int i = 0; i < length; i++) {
        printf("%02x", ((unsigned int*)arr)[i]);
        if (i != length - 1) printf(", ");
    }
    printf("\n");

    return 0;
}
```

### 2.2.3. Check the memory of array `arr`:

Which mean how much byte size we used to allocating `arr`, we doing it by calling `sizeof()` std function.

```c
printf("size of arr = %lu\n", sizeof(arr));
printf("size of arr element = %lu\n", sizeof(arr[0]));
```

which return:

```stdout
size of arr = 32
size of arr element = 8
```

> If using this on `arr_2`, we will get `(array size, element size) = (32, 4)` 

We also try to print out the array `arr` memory:

```c
printHex((unsigned int*)arr, sizeof(arr)/sizeof(unsigned int));
```

which return:
```stdout
Array with 8 length:
00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
```

What we see, is hex value of array `arr`, that spanning over length of 32 bytes, with each hex number separated by "," (example `00000001`) above representing 4 byte.

As each element is represented by 8 bytes, which mean, a element in array `arr`, which is set as `1` is represented as:

```stdout
00000001, 00000000
```

> The number not actually representing as `00000...0001`,  this meaning we having a little-endian architecture, where the byte order is reverse. But we're not covering this yet, there a lot to know to even understanding this type of problem (We have 32 bit computer memory here).

### 2.2.4. A different view/accessing way to the memory of array `arr`:

I create a new view for `arr` (via [[pass around variable|C pointer]] ):

```c
unsigned int *arr_view = (unsigned int *) &arr;
unsigned int arr_length = (unsigned int) (sizeof(arr)/sizeof(unsigned int));
printHex(arr_view, arr_length);
```

This `arr_view` is a helper to accessing the same memory that `arr` being allocated. Which mean, printing `arr_view` return the same as `arr` 

```stdout
Array with 8 length:
00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
```

Next, i set a new value into the index `[1]` of the `arr_view` to `fffffff` (which just a flip bit of `00000000`). This effect our `arr[0]` value (which spanning over `arr_view[0]` and `arr_view[1]`)

```c
arr_view[1] = (unsigned int)~0;
printHex(arr_view, arr_length);
printf("New value of arr[0] = %lld\n\n\n", arr[0]);
```

```stdout
Array with 8 length:
00000001, ffffffff, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000
New value of arr[0] = -4294967295
```

### 2.2.5. String/Char array

String/Char array look suck when we care about memory. C use ACSII for character representation, but by creating variable differently we can land the string in a totally different memory.

This will be store into a separated string memory (that store all static string that appear in the source):
```c
char *hello = "Hello world!"; // We not using this
```

While this type of create will be allocated into stack memory:
```c
char hello[13] = "Hello world!";
```

```c
    printf("String: %s with length of %lu, memory size %lu\n", hello, strlen(hello), sizeof(hello) );
    for (int i = 0; i <= strlen(hello); i++) {
        printf("%2x ", hello[i]);
    }
    printf("\n");
    for (int i = 0; i <= strlen(hello); i++) {
        printf("%2c ", hello[i]);
    }
    printf("\n%s\n",hello);
    printHex((unsigned int*) hello, strlen(hello)/(4*sizeof(char)) + 1);
    printHex((unsigned int*) hello, 40);
```

```
String: Hello world! with length of 12, memory size 13
48 65 6c 6c 6f 20 77 6f 72 6c 64 21  0
 H  e  l  l  o     w  o  r  l  d  !
Hello world!
Array with 4 length:
6c6c6548, 6f77206f, 21646c72, 00000000
Array with 40 length:
6c6c6548, 6f77206f, 21646c72, 00000000, 00000000, 00000000, 00000000, 00000000, 00000001, ffffffff, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000
```

- The output can't express this, so here is reference when we use `char* hello`, by printing `hello` we see that there is a `00` at the `[3]`. This is because the initiation of string value "Hello..." will always have a `null` padding (`00` character).

    ```stdout
    Array with 12 length:
    6c6c6548, 6f77206f, 21646c72, 72724100, 77207961, 20687469, 6c206425, ...
    ```

- Also, we can see that our string is store in revert order, little-endian again. Each char is effectively store into 1 byte. Sharing 32 bit instead of storing each one in a 32 bit memory. (if we return back - or using embed/IOT device, `int` is 16 bits, `long` is 32 bits but now general memory is more efficiency handling 32 bits number, which `int` now default 32 bits, which the same as `long`)

    ```
    48 65 6c 6c 6f 20 77 6f 72 6c 64 21  0
     H  e  l  l  o     w  o  r  l  d  !
    Hello world!
    Array with 4 length:
    6c6c6548, 6f77206f, 21646c72, 00000000
    ```

- We can see the appearance of `arr` and `arr_2` on the stack, next to our `hello` value.

    ```
    Array with 40 length:
    6c6c6548, 6f77206f, 21646c72, 00000000, 00000000, 00000000, 00000000, 00000000, 00000001, ffffffff, 00000001, 00000000, 00000001, 00000000, 00000001, 00000000, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000, 00000000
    ```
### 2.2.6. Overflow the stack

```c
    // The continue of the stack alocation
    printf("We working with arr_2 (all init with 4)\n");
    printHex((unsigned int*)arr_2, sizeof(arr_2)/sizeof(unsigned int));
    
    // Change arr, but arr_2 will be affected
    for (int i = 4; i < 8; i++) {
        arr[i] = 2;
    }

    printHex((unsigned int*)arr_2, sizeof(arr_2)/sizeof(unsigned int));
```

```output
Array with 8 length:
00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004, 00000004
Array with 8 length:
00000002, 00000000, 00000002, 00000000, 00000002, 00000000, 00000002, 00000000
```

C handle memory next to each other. While `arr[x]` will literally just a offset accessing memory over a starting point. So we can accessing other variable memory and modify it (even a memory/assembly instruction of the program can be access - but modify is prevent by specify writeable memory of runtime executable). 