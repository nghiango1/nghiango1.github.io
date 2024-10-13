---
share: true
catagory: Basic
tags:
  - computer_sience
---

# 1. Problem

Tell about `array`, what is it?

# 2. Solve

Array is a continue space of memory

Memory it self is a problem, every language do different thing to allocating memory, which affecting how array work under the hood.

- **Get**: Which usually come with a offset, which the computer will jump to array stored memory, take into the element size of that array, multiple it with `offset`. Then load it to the CPU.
    ```asm
    load <arr> + <offset> * <size>
    ```
- **Set**: We accessing the memory need to change and modify it data
    ```asm
    set <arr> + <offset> * <size>, <value>
    ```

By using the randomize accessing memory, knowing exact where we want to access, and how much size we want to get. By leverage our Hardware (RAM, L Cache), this can count as (time:: O(1)) 


# 3. Array reference

> By focusing on a specific language, we can have a deeper talk of array concept:

## 3.1. C language array and memory allocation

### 3.1.1. LAP 1 - Array in C

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

The above program it self, return this:

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

### 3.1.2. Explaination - What happening here?

#### 3.1.2.1. Created an array variable:

```c
char hello[13] = "Hello world!";
long long arr[4] = {1, 1, 1, 1};
int arr_2[8] = {4, 4, 4, 4, 4, 4, 4, 4};
```

- `hello` is an array of `char` type, that being initiation with `"Hello world!"`
- `arr` is an array of `long long` (8 bytes number) type, that all element initiation with 1;
- `arr_2` is an array of `int` (4 bytes number) type, that all element initiation with 4;

#### 3.1.2.2. Helper function that printout the exact value (in hex) of a memory

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

#### 3.1.2.3. Check the memory of array `arr`:

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

#### 3.1.2.4. A different view/accessing way to the memory of array `arr`:

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

#### 3.1.2.5. String/Char array

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
#### 3.1.2.6. Overflow the stack

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
## 3.2. Javascript

We can create a new buffer array using `new ArrayBuffer()`. Which it self can be feed into `new Uint8Array()`, `new Uint16Array()`, `new Uint32Array()`, `Uint8ClampedArray()`constructor to create direct view of that memory.

```node
> a = new ArrayBuffer(16)
ArrayBuffer {
  [Uint8Contents]: <00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>,
  byteLength: 16
}
> b = new Uint8Array(a)
Uint8Array(16) [
  0, 0, 0, 0, 0, 0,
  0, 0, 0, 0, 0, 0,
  0, 0, 0, 0
]
> c = new Uint16Array(a)
Uint16Array(8) [
  0, 0, 0, 0,
  0, 0, 0, 0
]
> d = new Uint32Array(a)
Uint32Array(4) [ 0, 0, 0, 0 ]
> e = new Uint8ClampedArray(a)
Uint8ClampedArray(16) [
  0, 0, 0, 0, 0, 0,
  0, 0, 0, 0, 0, 0,
  0, 0, 0, 0
]
```

By assigning any of them, we directly change the memory that allocated to a

```node
> d[0] = 4
4
> d
Uint32Array(4) [ 4, 0, 0, 0 ]
> a
ArrayBuffer {
  [Uint8Contents]: <04 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>,
  byteLength: 16
}
```

```node
> b[2] = ~b[2]
-1
> a
ArrayBuffer {
  [Uint8Contents]: <04 00 ff 00 00 00 00 00 00 00 00 00 00 00 00 00>,
  byteLength: 16
}
```

Because all a, b, c, d, e point to the same memory, we also changing all of them as once. Resulting the number appearance update when querying them back:

```node
> a
ArrayBuffer {
  [Uint8Contents]: <04 00 ff 00 00 00 00 00 00 00 00 00 00 00 00 00>,
  byteLength: 16
}
> b
Uint8Array(16) [
  4, 0, 255, 0, 0, 0,
  0, 0,   0, 0, 0, 0,
  0, 0,   0, 0
]
> c
Uint16Array(8) [
  4, 255, 0, 0,
  0,   0, 0, 0
]
> d
Uint32Array(4) [ 16711684, 0, 0, 0 ]
> e
Uint8ClampedArray(16) [
  4, 0, 255, 0, 0, 0,
  0, 0,   0, 0, 0, 0,
  0, 0,   0, 0
]
```

We can see that, `d[0] = 16711684` (32 bit array) is the result of true decimal value hold by `"04 00 ff 00"`.

The following number `[0]` is there to tell us the offset, that how we want the computer to access the memory, by increasing it, the computer jump over 32 bit each.

While `b[2]` (8 bit array) is pointing to the start memory + an offset of `8 * 2 bit`. This type of traversal do cost time, base on RAM, but still can be count as instant for the most of case.

## 3.3. Python array

Not this:

```
a = [1,2,3]
```

This is a [[List|List]], a data structure that could be implement using array (it is implement using array, after I look at python source code)

```python
→ python
Python 3.10.12 (main, Jun 11 2023, 05:26:28) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = [1,2,3]
>>> help(a)

Help on list object:

class list(object)
 |  list(iterable=(), /)
 |
 |  Built-in mutable sequence.
 |
 |  If no argument is given, the constructor creates a new empty list.
 |  The argument must be an iterable if specified.
 |
 |  Methods defined here:
 |
 |  __add__(self, value, /)
 |      Return self+value.
```

There is a array object, you can use by importing from array module :

```python
import array from array

a = array('l') # signed integer 4 bytes array
help(a)
```

Output:
```python
class array(builtins.object)
 |  array(typecode [, initializer]) -> array
 |
 |  Return a new array whose items are restricted by typecode, and
 |  initialized from the optional initializer value, which must be a list,
 |  string or iterable over elements of the appropriate type.
 |
 |  Arrays represent basic values and behave very much like lists, except
 |  the type of objects stored in them is constrained. The type is specified
 |  at object creation time by using a type code, which is a single character.
 |  The following type codes are defined:
 |
 |      Type code   C Type             Minimum size in bytes
 |      'b'         signed integer     1
 |      'B'         unsigned integer   1
 |      'u'         Unicode character  2 (see note)
 |      'h'         signed integer     2
 |      'H'         unsigned integer   2
 |      'i'         signed integer     2
 |      'I'         unsigned integer   2
 |      'l'         signed integer     4
 |      'L'         unsigned integer   4
 |      'q'         signed integer     8 (see note)
 |      'Q'         unsigned integer   8 (see note)
 |      'f'         floating point     4
 |      'd'         floating point     8
```

Still, it just a [[List|List]], with extra type config, with a way to define type using a hidden Type code for no reason, why?

```c
static PyObject *ins(arrayobject *self, Py_ssize_t where, PyObject *v) {
  if (ins1(self, where, v) != 0)
    return NULL;
  Py_RETURN_NONE;
}
```