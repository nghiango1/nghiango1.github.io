---
share: true
catagory: "Program Language"
---
# What is Pascal?

This is the main language in Vietnam's educational for student bellow high school. Also, this is the main language for most of competition in the nation.

Most of my learning programming concepts and programming practices is between times spend on Pascal.
- Compiling: A pascal program need to be compile to run on computer
    - What we do is writing source code, a set of instruction and other information
    - Compiler will read all of that, producing the actual program
    - Program then need to be run by the OS to return result.
- Variable: Data type, **User defined Structure type**, Array, Pointer
- Logic: Expression, Logical (If-else) program flow, Loop
- Module: Procedure, Function
- Input/Output: User input, File handlining for input/output. (Most of time, input are from file)

> This contain basic usage of the language, which can over most used case for Pascal as a tool to learn about **Algorithms and data structures**.

Overcomplicate: Any Importing, Standard Library, Graphical User interface, Source File splitting, OS argument input etc.. is not needed for this type of educational purpose.

When I talk about **Algorithms and data structures**, I mean this much as we can't cover everything:

1. Brute force:
    - Generation process: Permutation, Combination
    - Backtracking (and Recursion): 
    - Branch and Bound (Nhánh và Cận): Traveling sale man problem
1. Algorithms features: 
    - Time/Calculation complexity:  Big O, theoretically measuring algorithms base on an approximately formula `f` base on the size of input (calling `n`), or `f(n) <= c*g(n) or O(g(n)) (c is a constanst)`
1. Hand on data structure:
    - [[List|List]]: A structure contain set of indexed element with the same type, with basic operation: Find, Insert, Delete.
        - Array list
        - Linked list
        - Double linked list
        - Connected Linked list: End connected with Head
        - Connected Double linked list:
    - Queue: A list that only insert at the end (rear) element and delete at the first (front) element  (FIFO - First in first out)
        - Linked list Queue
        - Cycler Linked list Queue: No, we not learn this.
        - Array Queue (Buffer Ring)
    - Stack: A list that only insert at the end (top) element  and delete at the end (top) element  (LIFO - Last in first out)
        - Linked list stack
        - Array Stack
    - Tree: Root, Subtree, Branch/Leaf, Deep. 
        - Binary tree:
            - Array Binary tree: I'm lazy, and this is fair enough, good old `2*x + 1, 2*x` child
            - Linked Binary tree: It bearable after I come to modern OOP language, pointer are the pain (In Pascal, C, C++ with all of `^, @, *, &` syntax) 
            - Tree traversal: Preorder (Node Left Right), In-order (Left Node Right), Post-order(Left Right Node). 
            - [[Mathematic expression calculation|Mathematic expression calculation]] tree representation:
                ![[Pasted image 20230902095115.png|Pasted image 20230902095115.png]]
                - Prefix (Preorder traversal): `* + / 6 2 3 - 7 4` 
                - Infix (In-order traversal):   `((6 / 2) + 3) * (7 - 4)`
                - Postfix (Post-order traversal): `6 2 / 3 + 7 4 - *`
        - K-ary tree:
            - Array K-ary tree
            - Linked K-ary tree
        - Heap
        - Array heap
        - Binary search tree
    - Array binary tree
    - Graph Edges
    - Graph Edges Matrix
    - Graph Adjected  Array
    - Directed graph

3. Hand on Algorithms:
    - String handling:
        - Insert
        - Delete - String refine/trim
        - String to number
        - String splitting
        - Big number operation: Add, Multiple, etc..
    - Recursion: I can come up to this question - Can we call a procedure inside it self? - even before studded/being told about recursion, back at that time this feel like magic, example: Hanoi Tower
    - Postfix formula calculation 
    - Infix to Postfix transfer
    - Sort
        - Selection sort
        - Bubble sort
        - Insertion sort: Assuming we have a sorted array with 0 length, we adding the number one by one to it while maintaining the array sorted. 
        - Shell sort: never use this
        - Quick sort: All in this one
        - Heap sort: You not even sort, just create a heap.
        - Distribution counting (I call it Count sort)
        - Radix sort: never use this, but should be the best here.
        - Merge sort: almost never use this
    - Search
        - Sequential Search (Linear Search)
        - Binary Search
        - Binary Search tree
        - Hash
        - Binary value search: Assuming input data as a binary sequence (don't care about representation of the data: String/Number/Floating point/ etc.)  
            - Digital search tree: Trie tree for binary but search value in both branch and leaf
            - Radix search tree: A full/trim Trie tree for binary, search value in leaf only.
    - Dynamic programing:
        - Recursive formulation implementation
            - Bottom - up approach: This work when we know exactly what needed to calculate before hand and build up to final result.
            - Recursion approach:  This is a more general approach when we can't analyzing what needed to calculate first:
                - Storing every calculation result.
                - Check store value, start calculating if we not calculate it yet.
        - Tracing: All result require providing trace that lead to that result, different on each problem.
        - Longest increasing sequence
        - Knapsack problem
# Getting start

## Install Pascal - Windows

To start with pascal programming, we can download and install Free Pascal. We can use `scoop` to installing it.

```cmd
scoop install freepascal
```

To validate the installation, you can use `where`

```cmd
C:\Users\...>where fp
C:\Users\...\scoop\shims\fp.exe
```

To start the IDE, open new terminal tab and press `fp` 

```cmd
fp
```

This not including helper on how to used this IDE for basic create new file or save, etc. So you can test some of the program function before going to the next session.

## Program basic syntax

A pascal main program start at `begin` and end with `end.`, the `.` providing compiler that it the end of the program. Compiler won't send any error for character that come after `.` 

![[Pasted image 20230831180047.png|Pasted image 20230831180047.png]]

## Hello world program

Create new file with this content

```pascal
begin
    writeln('Hello');
end.
```

To run the program:
- Go to menu `Compile` > `Compile <Alt + 9>` to compile the program
- Go to menu `Run` > `Run <Ctrl + 9>` to run the program
- Go to menu `Debug` > `Output` to read the output of the program

This UI is the result of using `Window `> `Title`

![[Pasted image 20230831175800.png|Pasted image 20230831175800.png]]

## After note

- While using the GUI can help a lot, from now on, we will only focus on the programming it self. 
- For further and above, I will used other editor (`neovim` with [this configuration](https://github.com/ylsama/nvim)) and `fpc` command to compile the code.
- This config is not providing any code completion, nor any helper directly for pascal. You could keep using `fp`, or other IDE like `vscode`, ... to following.

# Variable

Variable is a representation of a value that will be determine when the program running. We can reference it using it's name.
## Variable initiation and assign

Variable initiation have is own code block, that come between `var` and `begin`. This is example for:
1. Main program:
    - Example:
    ```pascal
    var
        x : integer;
        hello: string;
    begin
        x := 1;
        hello := 'Hello World!';
        writeln('This is your first variable ', x);
        writeln('This is your seccond variable ', hello);
    end.
    ```
    - Output:
    ```
    This is your first variable 1
    This is your seccond variable Hello World!
    ```

1.  Function, Procedure: This will be cover later, see [[Pascal#Module#Basic declaration|more here]]

We use operation `:=` to assign variable.

## Variable data type

Variable need to be initiation with Data type
### Predefined type

#### Basic type

Ref: https://www.freepascal.org/docs-html/ref/refsu4.html#x26-250003.1.1

I will focus on what we mostly used first:
- `boolean`: can contain TRUE/ FALSE, which can be used for programing logic
- `shortint`: signed integer 8-bit (between range [2 ** 8 .. 2 ** 8 -1] )
- `integer`: signed integer 16-bit (between range [2 ** 16 .. 2 ** 16 -1] )
- `longint`: signed integer 32-bit (between range [2 ** 32 .. 2 ** 32 -1] )
- `int64`: signed integer 64-bit (between range [2 ** 64 .. 2 ** 64 -1] )
- `char`: Single character, 8-bit
- `array`: A static structure of same element, with a unchangeable size and element type after initiation
- `string`: A variable can storing dynamic array of character, let assuming it is a dynamic character array

Example:

```pascal
var
    arr1 : array [1..10] of shortint;
    arr2 : array [1..10] of integer;
    arr3 : array [1..10] of longint;
    arr4 : array [1..10] of int64;
    c : char;
    ac : Array [1..100] of char;
    s : string;
begin
    // length can tell the length of array 'like' structure
    writeln(length(arr1), ' ', sizeof(arr1[1]));
    writeln(length(arr2), ' ', sizeof(arr2[1]));
    writeln(length(arr3), ' ', sizeof(arr3[1]));
    writeln(length(arr4), ' ', sizeof(arr4[1]));

    // a single character have 1 length, with size 1 bytes
    c := 'A';
    writeln(c, ' ', length(c), ' ', sizeof(c));

    // a static character array have 100 length
    ac:= 'ABCDEFGH';
    writeln(ac,' ', length(ac));

    // a dynamic character array have length modified by the assigned contents
    s := 'Hello';
    writeln(s, ' ', length(s));
    s := 'Many more character';
    writeln(s, ' ', length(s));
end.
```
Output:
```stdout
10 1
10 2
10 4
10 8
A 1 1
ABCDEFGH^@^@^@^@^@^@^@...@^@^@ 100
Hello 5
Many more character 19
``` 

#### Real number

Ref: https://www.freepascal.org/docs-html/ref/refsu5.html

To storing real number, example `1.4`, `pi = 3.14`, etc. We can use two type of real number Data type:
1. Approximate:
    - `Real`: signed real 32-bit (or 16-bit) 
    - `Double`: signed integer 32-bit 
2. Exact number:
    - `currency`: up to 4 number after floating point

|Type|Range|Significant digits|Size|
|---|---|---|---|
|Real|platform dependant|???|4 or 8|
|Single|1.5E-45 .. 3.4E38|7–8|4|
|Double|5.0E-324 .. 1.7E308|15–16|8|
|Extended|1.9E-4932 .. 1.1E4932|19–20|10|
|Comp|-2E64+1 .. 2E63-1|19–20|8|
|Currency|-922337203685477.5808 .. 922337203685477.5807|19–20|8|

Example:

```pascal
var
    x: real;
    y: double;
    z: currency;
begin
    x := 1.4;
    writeln(x, ' ', sizeof(x));
    y := 1.4;
    writeln(y, ' ', sizeof(y));
    z := 1.4;
    writeln(z, ' ', sizeof(z));
    z := 1.23456789;
    writeln(z, ' ', sizeof(z));
end.
```

Output:
```stdout
 1.3999999999999999E+000 8
 1.3999999999999999E+000 8
 1.400000000000000000E+00 8
 1.234600000000000000E+00 8
```

### User defined Structural type

User defined Structural type need to be initiation, that have is own code block, come between `type` and `var`
- We use `record` syntax to create new Data type

```pascal
type
    point = record
        x: integer;
        y: integer;
    end;
var
    a : point;
    b : point;
begin
    a.x := 1;
    a.y := 4;
    writeln(a.x);
    writeln(a.y);
end.
```

Output:
```stdout
1
4
```

## Assign Structural type

You can assign each children variable via `.`

```pascal
    a.x := 1;
    a.y := 4;
```

Or can assign directly if they have the same Structural type:

```pascal
type
    point = record
        x: integer;
        y: integer;
    end;
var
    a : point;
    b : point;
begin
    a.x := 1;
    a.y := 4;
    writeln(a.x);
    writeln(a.y);
    b := a;
    writeln(b.x);
    writeln(b.y);
end.
```

Output:

```stdout
1
4
1
4
```
### Pointer Data type

#### Basic usage

A variable with Pointer Data type can contain a memory address of another variable with the same corresponded Data type, useful for other advanced data structure, basic syntax:

Initiation:
- `<var-name> : ^<data-type>` 

Usage:
- `<var-name>`: Accessing pointer variable
- `<var-name>^` : Accessing the memory address of our pointer variable storing
- `addr(<var-name>)` or `@<var-name>`: Getting the address of any variable, this is useful for assign normal variable to a pointer variable

Example:
```pascal
var
    a : integer;
    b : ^integer;
begin
    a := 2;
    b := @a;
    writeln(a);
    writeln(b^);
    b := addr(a);
    writeln(b^);
end.
```

Output:
```stdout
2
2
2
```

Pointer Data type will force that variable can only be use to storing a address that have the same corresponded Data type (Which generating compiler error).
- Data Type can be a user defined structure type
- Data Type can be a predefine type

> Pointer Data type can't be `writeln()`, this will generate compile error 

Creating a Pointer Data type: 
- You add `^` before the corresponded Data type.
- You can create nested type, which use it self Pointer Data type. This is allowed as Pascal compiler allowing **not completely defined Pointer Data type**, which is call **Forward type** (this still have to be resolve).

```pascal
type
    MyRec = record
        value: integer;
        next: ^MyRec;
    end;
var
    linkedList : MyRec;
    a : integer;
    b : ^integer;
begin
    linkedList.value := 1;
    linkedList.next := nil;
    writeln(linkedList.value);
    writeln(linkedList.next = nil);
    a := 2;
    b := @a;
    writeln(a);
end.
```

Output:

```stdout
1
TRUE
2
```

#### Assigning Pointer variable

Like every other variable, you want to assign value to a pointer before accessing it

1. To already knew variable:
    ```pascal
    var
        ...
        a : integer;
        b : ^integer;
        c : ^integer;
    begin
        ...
        b := @a;
        writeln(a);
        writeln(b^);
    ```
    
    or we can used `addr()`, which do the same:
    ```pascal
        b := addr(a);
        writeln(a);
        writeln(b^);
    end.
    ```

1. To another pointer:
    ```pascal
    var
        ...
        b : ^integer;
        c : ^integer;
    begin
        ...
        // Assign to another pointer
        writeln('=============');
        c := b;
        writeln(b^);
        writeln(c^, ' ', b = c);
    end.
    ```

1. Create new memory: By using `new()`, this tell computer to give a new memory to our program. 
    - We need to giving memory back to computer using `dispose()`, so that computer can give memory to other programs running on it.
    - Still, if the program is terminated, all memory will be given back to the computer.
    ```pascal
        new(b);
        b^ := 2;
        writeln(b^);
        dispose(b);
    ```

1. To nothing: `nil`, this is a special value that any pointer can point to.
```pascal
        linkedList.next = nil
```
#### Full Example

```pascal
type
    MyRec = record
        value: integer;
        next: ^MyRec;
    end;
var
    linkedList : MyRec;
    a : integer;
    b : ^integer;
    c : ^integer;
    pointerLinkedList : ^MyRec;
begin
    linkedList.value := 1;
    linkedList.next := nil;
    writeln(linkedList.value);
    writeln(linkedList.next = nil);
    a := 2;
    b := @a;
    writeln(a);
    writeln(b^);

    // Or use addr()
    writeln('=============');
    b := addr(a);
    writeln(a);
    writeln(b^);

    // Assign to another pointer
    writeln('=============');
    c := b;
    writeln(b^);
    writeln(c^, ' ', b = c);

    // Alocating new memory
    writeln('=============');
    new(b);
    b^ := 4;
    writeln(a);
    writeln(b^, ' (we can see that a and b is different)');
    dispose(b); // Remember to dispose after new

    // User defined struct type is simmilar
    writeln('=============');
    pointerLinkedList := @linkedList;

    writeln(pointerLinkedList^.value);
    writeln(pointerLinkedList^.next = nil);

    // User defined struct type is simmilar
    writeln('=============');
    new(pointerLinkedList);
    pointerLinkedList^.value := 2;
    pointerLinkedList^.next := nil;

    writeln(pointerLinkedList^.value);
    writeln(pointerLinkedList^.next = nil);
    dispose(pointerLinkedList); // Remember to dispose after new
end.
```

Output:

```
1
TRUE
2
2
=============
2
2
=============
2
2 TRUE
=============
2
4 (we can see that a and b is different)
=============
1
TRUE
=============
2
TRUE
```

# Logic

## Expression

Expression: any thing that result TRUE/FALSE (`boolean` Data type):

- Comparation: ` =, >=, >, <=, <`  and `<>` for check different of value

    ```pascal
    var
        b: boolean;
    begin
        b := 1 = 1;
        writeln(b); // 1 equal 1 -> true
        b := 1 <> 1;
        writeln(b); // 1 different 1 -> false
        b := 1 >= 1;
        writeln(b); // 1 equal or bigger than 1 -> true
        b := 1 <= 1;
        writeln(b); // 1 equal or less than 1 -> true
        b := 1 > 1;
        writeln(b); // 1 bigger 1 -> false
        b := 1 < 1;
        writeln(b); // 1 less 1 -> false
    end.
    ```
    
    Output:
    
    ```stdout
    TRUE
    FALSE
    TRUE
    TRUE
    FALSE
    FALSE
    ```

- Actual value:

    ```pascal
    b := TRUE
    ```

- Logic operation: `xor`, `or`, `and`, this cover most basic logic operation.

    ```pascal
    var
        b : boolean;
        t : integer;
    begin
        b := TRUE xor FALSE;
        writeln(b);
        b := TRUE or FALSE;
        writeln(b);
        b := TRUE and FALSE;
        writeln(b);
        // bytes wise logic operation does logic calculation
        // on coresponsed bit one by one
        t := %1010110101100101 xor %1010100010101010;
        writeln(binstr(t, sizeof(t) * 8), ' integer have size = ', sizeof(t), ' which mean it have ', sizeof(t) * 8, ' bits');
        t := %0110101001010101 or %0010101000101001;
        writeln(binstr(t, sizeof(t) * 8));
        t := %1001010000101001 and %0101001010101000;
        writeln(binstr(t, sizeof(t) * 8));
    end.
    ```

    Output:
    ```
    TRUE
    TRUE
    FALSE
    0000010111001111 integer have size = 2 which mean it have 16 bits
    0110101001111101
    0001000000101000
    ```
## If - then - else

Logical code block after `then` will only run when expression between `if ` and `then` is `TRUE`

```pascal
    if TRUE then begin
        x := x + 1;
        writeln('logic true ', x);
    end;
    if FALSE then begin
        x := x + 2;
        writeln('logic false, this is not running ', x);
    end;
```

You can adding `else`, so that expression between `if ` and `then` is `FALSE`, code block after `else` will be run instead.

```pascal
var
    x : integer;
begin
    x := 1;
    writeln(x);

    if TRUE then begin
        x := 2;
        writeln('logic true ', x);
    end;

    if FALSE then begin
        x := 100;
        writeln('logic false, this is not running ', x);
    end
    else begin
        x := 3;
        writeln('logic false, so this run instead ', x);
    end;
end.
```

Output:

```stdout
1
logic true 2
logic false, so this run instead 3
```

## While do (loop)

Logical code block after `while do` will run on repeat until expression between `while` and `do` is `FALSE`. We call this type of code is a loop.

```pascal
var
    x : integer;
begin
    x := 1;
    while x <= 5 do begin
        writeln(x);
        x := x + 1;
    end;
end.
```

Output

```stdout
1
2
3
4
5
```

## For - to - do loop

A special on loop that run in a fixed times, syntax look like this.
- `For - to - do` loop need a variable, which contain the current value of the range given to our `For - to - do` loop

```pascal
var
    x : integer;
begin
    for x:= 1 to 5 do begin
        writeln(x);
    end;
end.
```

- We can see it the same as this while do code:
 
```pascal
var
    x : integer;
begin
    x := 0;
    while x <= 5 do begin
        x := x + 1;
        writeln(x);
    end;
end.
```

Output

```stdout
1
2
3
4
5
```

# Module

A repeated code block that can be reused multiple time. We can used this type of Code block via calling it name inside our program after initiation (from now on, we use term `caller` for this interaction).

## Procedure: 

Basic module. Share a lot similarity with normal main program.

### Basic declaration

We use `procedure` keyword do define a procedure. We will want to know basic: 
- Name: module can be call via it's name.
    ```pascal
    procedure echo(x : string);
    // -------^^^^ This is procedure name
    ```
- Argument: Some information need to be pass to the procedure module from the caller.
    ```pascal
    procedure echo(x : string);
    // ------------^^^^^^^^^^ This is procedure agrument
    ```
- Local variable: Similar to main program, it come after the initiation of procedure, start at `var` and end at `begin`, which will only used and available for this procedure

    > A module is separated from it self, which mean you can't reuse variable after initiation 

    ```pascal
    procedure echo(hello : string);
    var
        world : string;
    begin
    end;
    ```
- Code block: Set of instruction that will be execute when this module call, it come after the initiation of procedure, start at `begin` and end at `end`
    ```pascal
    procedure echo();
    begin
        writeln('Hello world!');
    end;
    ```

Example:

```pascal
procedure echo(hello : string);
var
    world : string;
begin
    world := 'world!';
    writeln(hello, ' ', world);
end;

var
    hello: string;
begin
    hello := 'Hello';
    echo(hello);
    echo(hello);
    echo(hello);
    echo(hello);
end.
```

Output:

```stdout
Hello world!
Hello world!
Hello world!
Hello world!
```

### Pass by reference

When `caller` passing information to procedure as a argument, we can passing the actual variable to the procedure.

This mean any modification of this argument inside our procedure will take effect directly to the variable passed.

Example:

```pascal
procedure add1(step: integer; var x : integer);
// -------------------------------^^^^^^^^^^^ x being pass as a reference
begin
    x := x + step;
end;

var
    a: integer;
begin
    a := 0;
    writeln(a);
    add1(1, a);
    writeln(a);
    add1(2, a);
    writeln(a);
    add1(3, a);
    writeln(a);
    
end.
```

Output:

```stdout
0
1
3
6
```

This is important if you want to get some result back from the procedure, which lead to `function` 
## Function:

This special module need to be initiation with a Data type, which will return back a result with the same Data type to `caller` code. This return value is a disposable/temporary product, it won't be store in anything.

```pascal
function add(x : integer; y: integer) : integer;
begin
    add := x + y;
end;
var
    a : integer;
begin
    writeln(add(3, 3));
    a := add(5, 3);
    writeln(a);
end.
```

Output:

```stdout
6
8
```

## Recursion/Nested module

- Nested module: A module can call other module
- Recursion: is a term that describe a module call it self.

```pascal
function fibonachi(x: integer) : integer;
begin
    if x < 0 then begin
        fibonachi := -1;
    end
    else if x = 0 then begin
        fibonachi := 0;
    end
    else if x <= 2 then begin
        fibonachi := 1;
    end
    else begin
        fibonachi := fibonachi(x-1) + fibonachi(x-2);
    // --------------^^^^^^^^^^^^^----^^^^^^^^^^^^^^^ recursion
    end
end;
var
    i : integer;
begin
    for i := 0 to 10 do begin
        writeln(fibonachi(i));
    end;
end.
```

Output:

```stdout
0
1
1
2
3
5
8
13
21
34
55
```

# Input/output

## User input

We can use `read()`, `readln()` for most of input handling.

```pascal
var
    i: integer;
    v: integer;
    s: integer;
begin
    s := 0;
    for i:= 1 to 3 do begin
        readln(v);
        writeln('You input v = ', v);
        s := s + v;
    end;

    writeln('Total = ', s);
end.
```

This is a User interaction program.

## File handling

### Specials File Data type `text`

To work with file, we use Data type `text`
- Assign syntax: `assign(f, '<path_to_input>')`
- Open file for read: `reset(f)`
- Open file for write: 
    1. `rewrite(f)`: Delete all file content if existed, then start adding new data to the file 
    2. `append(f)`: Keep all file content, adding new data to the end of file
- Close: `close(f)`
    1. Using file (may) cause block other program from accessing that file (OS level handling)
    2. By closing, we release the file back to the Computer
    3. Also, this confirm all of our change to that file in the process.
### File input - Read from file

Reading from file is another way to send input to the program, still, read/writing have a lot more limitation on speed the Hardware.

```input.txt
1
1 2 3
string
string string
```

```pascal
var
    f: text;
    i: integer;
    v: integer;
    s: integer;
    str: string;
begin
    assign(f, 'input.txt');
    reset(f);
    s := 0;
    for i:= 1 to 4 do begin
        read(f, v);
        s := s + v;
        writeln(v)
    end;
    writeln(s);
    writeln('========');

    // Real until end of file
    while not eof(f) do begin
        readln(f, str);
        writeln(str);
    end;

    close(f);
end.
```

```output
1
1
2
3
7
========

string
string string
```

### File output - Write to file

Writing to file is another way to send result of the program to user, still, read/writing have a lot more limitation on speed the Hardware.

```pascal
var
    f: text;
    i: integer;
begin
    assign(f, 'output.txt');
    rewrite(f);
    for i := 1 to 10 do begin
        writeln(f, i);
    end;
    close(f);
end.
```

```output.txt
1
2
3
4
5
6
7
8
9
10
```