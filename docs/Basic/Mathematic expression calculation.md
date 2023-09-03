---
share: true
catagory: Basic
tags:
  - computer_sience
  - coding_chalenge
  - pascal
---
# Problem

Calculation and return result of a Mathematic expression

```stdin
(1+2) * 3 - 5/2 + 1 - 4*(1-3)
```

# Solve

> For simplicity, we use positive integer only. `/` operator will be consider as a `div` instead

## Information

### Infix expression

Input's Mathematic expression is called an infix expression, which is used by us in day today life

An infix expression have operators between operands. e.g. `A`,`A + B`,`(A + B) + (C – D)`. 

### Postfix expression

And postfix expression is a single letter or an operator, preceded by two postfix strings. Every postfix string longer than a single variable contains first and second operands followed by an operator .e.g. `A`,`A B +`,`A B + C D –`

## Basic knowledge needed 
(language:: pascal)

This will cover really basic on how to Program in [[Pascal#Getting start|Pascal]] if you want to follow more

### String handler

As input having multiple type of data, typing as a free form (user input) we need foundation/basic knowledge string handling, we need:

- String insert: Adding extra padding for separating different data type from user input.

```pascal
var
    s : string;
    i : integer;
begin
    readln(s);
    for i := length(s) downto 2 do begin
        insert(',', s, i);
    end;
    writeln(s);
end.
```

- String delete/refine/trim: Delete extra space given by the user input

```pascal
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

var
    s : string;
begin
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);
    trimMoreThan1Space(s);
    writeln('Trim string = "',s, '"');
end.
```

- String to number: Extract number value from string

```pascal
// Built-in function
var
    s : string;
    current : string;
    i, v, code: integer;
begin
    readln(s);
    current := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            val(current, v, code);
            if code > 0 then
                writeln('Error parse at ', code, ' th character, c ="', current[code] ,'", value = "', current, '"')
            else
                writeln('Found: ', v);
            current := '';
        end;
    end;
end.
```

By combining multiple technique, we can handle User input and turn them to numeric value that can be used for calculation.

```pascal
// Check on String handling. trim.pas
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

// Self-code
function parse(s: string; var isParse: boolean) : integer;
var
    i, v: integer;
begin
    v := 0;
    isparse := true;
    for i:= 1 to length(s) do begin
        if (ord('0') <= ord(s[i])) and (ord(s[i]) <= ord('9')) then
            v := v * 10  + ord(s[i]) - ord('0')
        else begin
            isparse := false;
            break;
        end;
    end;
    if not isparse then
        writeln('Error parse at ', i, ' th character, c ="', s[i] ,'", value = "', s, '"')
    else 
        writeln('Found: ', v);
    parse := v;
end;

// Check on String handleing to_number.pas
var
    s : string;
    current : string;
    i: integer;
    isparse: boolean;
begin
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);
    trimMoreThan1Space(s);
    current := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            parse(current, isparse);
            current := '';
        end;
    end;
end.
```

> We can also go down the road of implement string Big number, for persistence calculation. Or parse to float number.

### Stack data structure

#### Linked stack

Postfix mathematic expression calculation is heavily rely on stack for storing calculation product data.  We implement basic operation of stack:
- Push (Last in)
- Pop (First out)
- Top (get the value of top of stack)
- Stack traversal: Check the stack data

```pascal
type
    stacktype = record
        top: ^node;
        length: integer;
    end;
    node = record
        value : integer;
        next : ^node;
    end;

procedure push(var s: stacktype; v : integer);
var
    temp : ^node;
begin
    if s.top = nil then begin
        new(s.top);
        s.top^.value := v;
        s.top^.next := nil;
    end
    else begin
        new(temp);
        temp^.value := v;
        temp^.next := s.top;
        s.top := temp;
    end;
    s.length := s.length + 1;
end;

function pop(var s: stacktype) : integer;
var
    temp : ^node;
begin
    pop := s.top^.value;
    temp := s.top;
    s.top := s.top^.next;
    s.length := s.length - 1;
    dispose(temp);
end;

procedure stackTraversal(stack: stacktype);
var
    temp : ^node;
begin
    writeln('Stack length: ', stack.length, ' element');
    temp := stack.top;
    write('Stack state: ');
    while temp <> nil do begin
        write(temp^.value, ', ');
        temp := temp^.next;
    end;
    writeln('nil');
end;

function top(stack: stacktype) : integer;
begin
    top := stack.top^.value;
end;

var
    s : stacktype;
begin
    s.top := nil;
    s.length := 0;
    push(s, 1);
    writeln(top(s));
    push(s, 2);
    writeln(top(s));
    stackTraversal(s);
    while s.top <> nil do begin
        pop(s);
    end;
end.
```

Combining with User input handling, we can add number value from user input to push directly into the stack:

```pascal
type
    stacktype = record
        top: ^node;
        length: integer;
    end;
    node = record
        value : integer;
        next : ^node;
    end;

procedure push(var s: stacktype; v : integer);
var
    temp : ^node;
begin
    if s.top = nil then begin
        new(s.top);
        s.top^.value := v;
        s.top^.next := nil;
    end
    else begin
        new(temp);
        temp^.value := v;
        temp^.next := s.top;
        s.top := temp;
    end;
    s.length := s.length + 1;
end;

function pop(var s: stacktype) : integer;
var
    temp : ^node;
begin
    pop := s.top^.value;
    temp := s.top;
    s.top := s.top^.next;
    s.length := s.length - 1;
    dispose(temp);
end;

procedure stackTraversal(stack: stacktype);
var
    temp : ^node;
begin
    writeln('Stack length: ', stack.length, ' element');
    temp := stack.top;
    write('Stack state: ');
    while temp <> nil do begin
        write(temp^.value, ', ');
        temp := temp^.next;
    end;
    writeln('nil');
end;

function top(stack: stacktype) : integer;
begin
    top := stack.top^.value;
end;

// Check on String handling. trim.pas
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

// Check on string handleing to_number.pas
function parse_integer(var s: string; var isParse: boolean) : integer;
var
    v, code: integer;
begin
    val(s, v, code);
    if code > 0 then begin
        writeln('Error parse at ', code, ' th character, c ="', s[code] ,'", value = "', s, '"');
        isParse := false;
    end
    else begin
        writeln('Found: ', v);
        isParse := true;
    end;
    parse_integer := v;
end;

procedure to_integer(var stack : stacktype);
var
    s : string;
    current : string;
    i, value: integer;
    isparse: boolean;
begin
    isparse := FALSE; // Stop compiler warning
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);
    trimMoreThan1Space(s);
    current := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            value := parse_integer(current, isparse);
            if isparse then
                push(stack, value);
            current := '';
        end;
    end;
end;

var
    s : stacktype;
begin
    s.top := nil;
    s.length := 0;
    push(s, 1);
    writeln(top(s));
    push(s, 2);
    writeln(top(s));
    stackTraversal(s);
    while s.top <> nil do begin
        pop(s);
    end;

    // input to stack
    s.top := nil;
    s.length := 0;
    to_integer(s);
    stackTraversal(s);
    while s.top <> nil do begin
        writeln('Pop: ', pop(s));
    end;
    if s.top <> nil then dispose(s.top);
end.
```

#### Array stack

Stack can also be implementation using array, which have fixed size. Because of that, there is a maximum element that stack can have. 

This is quite easy to implement than linked list.

```pascal
type
    stringStack = record
        value: array [1..100] of char;
        length: integer;
    end;

procedure stringpush(var stack: stringStack; value : char);
begin
    stack.length := stack.length + 1;
    stack.value[stack.length] := value;
end;

function stringpop(var stack: stringStack) : char;
begin
    stringpop := stack.value[stack.length];
    stack.length := stack.length - 1;
end;

function stringtop(var stack: stringStack) : char;
begin
    stringtop := stack.value[stack.length];
end;

...
```

## Calculation implementation

### Postfix expression calculation

Calling back from definition

![[Mathematic expression calculation#Postfix expression|Mathematic expression calculation > Postfix expression]]

We can do all of the calculation on the stack
- A number found, we add it to stack
- A operation found, we take two element at the top of stack, do calculation, then push result back to stack

By combine this with string comparison, input handling, stack implementation.

```pascal
type
    stacktype = record
        top: ^node;
        length: integer;
    end;
    node = record
        value : integer;
        next : ^node;
    end;

const
    operation = ['+', '-', '*', '/'];

procedure push(var s: stacktype; v : integer);
var
    temp : ^node;
begin
    if s.top = nil then begin
        new(s.top);
        s.top^.value := v;
        s.top^.next := nil;
    end
    else begin
        new(temp);
        temp^.value := v;
        temp^.next := s.top;
        s.top := temp;
    end;
    s.length := s.length + 1;
end;

function pop(var s: stacktype) : integer;
var
    temp : ^node;
begin
    pop := s.top^.value;
    temp := s.top;
    s.top := s.top^.next;
    s.length := s.length - 1;
    dispose(temp);
end;

procedure stackTraversal(stack: stacktype);
var
    temp : ^node;
begin
    writeln('Stack length: ', stack.length, ' element');
    temp := stack.top;
    write('Stack state: ');
    while temp <> nil do begin
        write(temp^.value, ', ');
        temp := temp^.next;
    end;
    writeln('nil');
end;

//function top(stack: stacktype) : integer;
//begin
//    top := stack.top^.value;
//end;

// Check on String handling. trim.pas
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

// Check on string handleing to_number.pas
function parse_integer(var s: string; var isParse: boolean) : integer;
var
    v, code: integer;
begin
    val(s, v, code);
    if code > 0 then begin
        writeln('Error parse at ', code, ' th character, c ="', s[code] ,'", value = "', s, '"');
        isParse := false;
    end
    else begin
        writeln('Found: ', v);
        isParse := true;
    end;
    parse_integer := v;
end;

procedure processingCurrentString(current : string; var stack: stacktype);
var
    x, y, value: integer;
    isparse : boolean;
begin
    writeln('Processing current = ', current);
    // try parse to integer
    if (current[1] in operation) then begin
        if stack.length < 2 then begin
            writeln('Formula error, not enough element for calculation');
        end;
        y := pop(stack);
        x := pop(stack);
        writeln('Found valid operation, do: ', x, ' ', current, ' ', y);
        if current[1] = '+' then push(stack, x + y);
        if current[1] = '-' then push(stack, x - y);
        if current[1] = '*' then push(stack, x * y);
        if current[1] = '/' then push(stack, x div y);
    end
    else begin
        isparse := false;
        value := parse_integer(current, isparse);
        if isparse then
            push(stack, value)
        else
            writeln('Cant parse to integer, skipping');
    end;
end;

procedure inputHandling(var stack : stacktype);
var
    s : string;
    current : string;
    i : integer;
begin
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);

    for i:= length(s) downto 2 do begin
        if (s[i] in operation) and (s[i-1] in operation) then begin
            insert(' ', s, i);
        end;
    end;

    trimMoreThan1Space(s);

    writeln(s);


    current := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            processingCurrentString(current, stack);
            current := '';
        end;
    end;
    if current <> '' then
        processingCurrentString(current, stack);
end;

var
    s : stacktype;
begin
    // input to stack
    s.top := nil;
    s.length := 0;
    inputHandling(s);
    stackTraversal(s);
    while s.top <> nil do begin
        writeln('Pop: ', pop(s));
    end;
    if s.top <> nil then dispose(s.top);
end.
```

### Infix to Postfix

A processing data middle step, as our input are in Infix format.

This can be done using a stack to hold operation and bracket:
- `)`:  This have the most priority, we want to translating infix expression between two bracket  to postfix first, which mean to close this bracket till we pop out a `(` bracket.
- `*`, `/`: This have high priority, we want to only translating all of `*` and `/` operation in the stack before adding another of this to stack.
- `+`, `-`: This have low priority, we want to translating all of `*`,  `/`, `+` and `-` operation (till we found a open bracket `(` or stack is empty) in the stack before adding another of this to stack.
- `(`: We just add it to stack, and wait for `)` to pop it back.

```pascal
type
    stacktype = record
        top: ^node;
        length: integer;
    end;
    node = record
        value : string;
        next : ^node;
    end;

const
    operation = ['+', '-', '*', '/', '(', ')'];

procedure push(var s: stacktype; v : string);
var
    temp : ^node;
begin
    if s.top = nil then begin
        new(s.top);
        s.top^.value := v;
        s.top^.next := nil;
    end
    else begin
        new(temp);
        temp^.value := v;
        temp^.next := s.top;
        s.top := temp;
    end;
    s.length := s.length + 1;
end;

function pop(var s: stacktype) : string;
var
    temp : ^node;
begin
    pop := s.top^.value;
    temp := s.top;
    s.top := s.top^.next;
    s.length := s.length - 1;
    dispose(temp);
end;

procedure stackTraversal(stack: stacktype);
var
    temp : ^node;
begin
    writeln('Stack length: ', stack.length, ' element');
    temp := stack.top;
    write('Stack state: ');
    while temp <> nil do begin
        write(temp^.value, ', ');
        temp := temp^.next;
    end;
    writeln('nil');
end;

function top(stack: stacktype) : string;
begin
    top := stack.top^.value;
end;

// Check on String handling. trim.pas
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

// Check on string handleing to_number.pas
function parse(var s: string; var isParse: boolean) : integer;
var
    v, code: integer;
begin
    val(s, v, code);
    if code > 0 then begin
        writeln('Error parse at ', code, ' th character, c ="', s[code] ,'", value = "', s, '"');
        isParse := false;
    end
    else begin
        writeln('Found: ', v);
        isParse := true;
    end;
    parse := v;
end;

function priority(current: string) : integer;
begin
    if (current = '*') or (current = '/') then priority := 2;
    if (current = '+') or (current = '-') then priority := 1;
    if (current = '(') then priority := 0;
end;

procedure processingCurrentString(current : string; var stack: stacktype; var postfix : string);
var
    isparse : boolean;
    temp : string;
begin
    writeln;
    stackTraversal(stack);
    writeln('Current postfix = ', postfix);
    isparse := false;
    writeln('Processing current = ', current);
    if (current[1] in operation) then begin
        if current = '(' then push(stack, current);
        if current = ')' then begin
            temp := pop(stack);
            while temp <> '(' do begin
                postfix := postfix + temp + ' ';
                temp := pop(stack);
            end;
        end;
        if current[1] in ['+', '-', '*', '/'] then begin
            while stack.length > 0 do begin 
                if priority(current) > priority(top(stack)) then 
                    break;
                postfix := postfix + pop(stack) + ' ';
            end;
            push(stack, current);
        end;
    end
    else begin
        parse(current, isparse);
        if isparse then begin
            postfix := postfix + current + ' ';
        end
        else begin
            writeln('Found unknown value "', current,'" skipping');
        end;
    end;
end;

var
    s : string;
    current : string;
    i : integer;
    postfix : string;
    stack : stacktype;
begin
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);

    // adding space between operation and number
    for i:= length(s) downto 2 do begin
        writeln(s[i]);
        if (s[i] <> ' ') and ((s[i] in operation) or (s[i-1] in operation)) then begin
            insert(' ', s, i);
        end;
    end;

    trimMoreThan1Space(s);

    writeln(s);

    stack.top := nil;
    stack.length := 0;
    current := '';
    postfix := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            processingCurrentString(current, stack, postfix);
            current := '';
        end;
    end;
    if current <> '' then
        processingCurrentString(current, stack, postfix);
    while stack.top <> nil do begin
        postfix := postfix + pop(stack) + ' ';
    end;
    writeln(postfix);
end.
```

### Infix expression calculation

We have two stack type:
- One for Infix to postfix coinventing, to handle and hold `string` value (operation + bracket)
- Second for postfix calculation stack, holding integer value.

This mean two type implementation, with their own implement of push, pop, top. We combine Infix to Postfix convention and Postfix expression calculation for final implement

```pascal
type
    integerStack = record
        top: ^node;
        length: integer;
    end;
    node = record
        value : integer;
        next : ^node;
    end;
    stringStack = record
        value: array [1..100] of char;
        length: integer;
    end;

const
    operation = ['+', '-', '*', '/', '(', ')'];

procedure push(var s: integerStack; v : integer);
var
    temp : ^node;
begin
    if s.top = nil then begin
        new(s.top);
        s.top^.value := v;
        s.top^.next := nil;
    end
    else begin
        new(temp);
        temp^.value := v;
        temp^.next := s.top;
        s.top := temp;
    end;
    s.length := s.length + 1;
end;

function pop(var s: integerStack) : integer;
var
    temp : ^node;
begin
    pop := s.top^.value;
    temp := s.top;
    s.top := s.top^.next;
    s.length := s.length - 1;
    dispose(temp);
end;

procedure stringpush(var stack: stringStack; value : char);
begin
    stack.length := stack.length + 1;
    stack.value[stack.length] := value;
end;

function stringpop(var stack: stringStack) : char;
begin
    stringpop := stack.value[stack.length];
    stack.length := stack.length - 1;
end;

function stringtop(var stack: stringStack) : char;
begin
    stringtop := stack.value[stack.length];
end;

// Check on String handling. trim.pas
procedure trimSpaceRight(var s: string);
begin
    while (length(s) > 0) and (s[length(s)] = ' ') do begin
        delete(s, length(s), 1);
    end;
end;

procedure trimSpaceLeft(var s: string);
begin
    while (length(s) > 0) and (s[1] = ' ') do begin
        delete(s, 1, 1);
    end;
end;

procedure trimMoreThan1Space(var s: string);
var
    i : integer;
begin
    for i:= length(s) downto 2 do begin
        if (s[i] = s[i-1]) and (s[i] = ' ') then
            delete(s, i, 1);
    end;
end;

// Check on string handleing to_number.pas
function parse(s: string; var isParse: boolean) : integer;
var
    v, code: integer;
begin
    val(s, v, code);
    if code > 0 then begin
        writeln('Error parse at ', code, ' th character, c ="', s[code] ,'", value = "', s, '"');
        isParse := false;
    end
    else begin
        writeln('Found: ', v);
        isParse := true;
    end;
    parse := v;
end;

function priority(current: string) : integer;
begin
    if (current = '*') or (current = '/') then priority := 2;
    if (current = '+') or (current = '-') then priority := 1;
    if (current = '(') then priority := 0;
end;

procedure processingInfixString(current : string; var stack: stringStack; var postfix : string);
var
    isparse : boolean;
    temp : string;
begin
    writeln('Current postfix = ', postfix);
    isparse := false;
    writeln('Processing current = ', current);
    if (current[1] in operation) then begin
        if current = '(' then stringpush(stack, current[1]);
        if current = ')' then begin
            temp := stringpop(stack);
            while temp <> '(' do begin
                postfix := postfix + temp + ' ';
                temp := stringpop(stack);
            end;
        end;
        if current[1] in ['+', '-', '*', '/'] then begin
            while stack.length > 0 do begin 
                if priority(current) > priority(stringtop(stack)) then 
                    break;
                postfix := postfix + stringpop(stack) + ' ';
            end;
            stringpush(stack, current[1]);
        end;
    end
    else begin
        parse(current, isparse);
        if isparse then begin
            postfix := postfix + current + ' ';
        end
        else begin
            writeln('Found unknown value "', current,'" skipping');
        end;
    end;
end;

function toPostfix(s : string): string;
var
    current : string;
    i : integer;
    postfix : string;
    stack : stringStack;
begin
    stack.length := 0;

    current := '';
    postfix := '';
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            processingInfixString(current, stack, postfix);
            current := '';
        end;
    end;
    if current <> '' then
        processingInfixString(current, stack, postfix);
    while stack.length > 0 do begin
        postfix := postfix + stringpop(stack) + ' ';
    end;

    toPostfix := postfix;
end;

procedure processingPostfixString(current : string; var stack: integerStack);
var
    x, y, value: integer;
    isparse : boolean;
begin
    writeln('Processing current = ', current);
    isparse := false;
    // try parse to integer
    if (current[1] in operation) then begin
        if stack.length < 2 then begin
            writeln('Formula error, not enough element for calculation');
        end;
        y := pop(stack);
        x := pop(stack);
        writeln('Found valid operation, do: ', x, ' ', current, ' ', y);
        if current[1] = '+' then push(stack, x + y);
        if current[1] = '-' then push(stack, x - y);
        if current[1] = '*' then push(stack, x * y);
        if current[1] = '/' then push(stack, x div y);
    end
    else begin
        value := parse(current, isparse);
        if isparse then
            push(stack, value)
        else
            writeln('Cant parse to integer, skipping');
    end;
end;

var
    s : string;
    current : string;
    i : integer;
    stack: integerStack;
begin
    readln(s);
    trimSpaceRight(s);
    trimSpaceLeft(s);

    // adding space between operation and number
    for i:= length(s) downto 2 do begin
        writeln(s[i]);
        if (s[i] <> ' ') and ((s[i] in operation) or (s[i-1] in operation)) then begin
            insert(' ', s, i);
        end;
    end;

    trimMoreThan1Space(s);
    s := toPostfix(s);

    current := '';
    stack.top := nil;
    stack.length := 0;
    for i := 1 to length(s) do begin
        if s[i] <> ' ' then
            current := current + s[i]
        else begin
            processingPostfixString(current, stack);
            current := '';
        end;
    end;
    if current <> '' then
        processingPostfixString(current, stack);
    while stack.top <> nil do begin
        writeln(pop(stack));
    end;
end.
```