---
share: true
catagory: Basic
tags:
  - computer_sience
---
# Problem

What is a List?

# Solve

This is a basic structure, contain set of indexed element with the same type, with basic operation like: Find, Insert, Delete, Sort, ... 

## Implementation

### Array List
(language :: pascal)

This just better inn many way, but we need to handle the extending of the List, which cost O(n). The `maxsize` of the array can be a problem, and we may need to implement memory copy to move to a bigger array if we running out.
- Get: O(1)
- Set: O(1)

- Push: O(1)
- Pop: O(1)
- Insert: O(n)
- Find: O(n)
- Delete: O(n)

```pascal
type
    list = record
        value : array [1..100] of integer;
        length : integer;
    end;

procedure push(var l : list; v : integer);
begin
    l.length := l.length + 1;
    l.value[l.length] := v;
end;

function pop(var l : list) : integer;
begin
    pop := l.value[l.length];
    l.length := l.length - 1;
end;

procedure insert(var l : list; pos : integer; v : integer);
var
    i : integer;
begin
    for i := l.length downto pos do begin
        l.value[i+1] := l.value[i]
    end;
    l.value[pos] := v;
    l.length := l.length + 1;
end;

function find(l : list; v : integer) : integer;
var
    i : integer;
begin
    for i := 1 to l.length do begin
        if l.value[i] = v then begin
            find := i;
            break
        end;
    end;
end;

procedure delete(var l: list; pos : integer); 
var
    i : integer;
begin
    for i := pos to l.length do begin
        l.value[i] := l.value[i+1];
    end;
    l.length := l.length - 1;
end;

procedure printList(l : list);
var
    i : integer;
begin
    write('List size ', l.length, ' with item: ');
    for i := 1 to l.length-1 do begin
        write(l.value[i], ', ');
    end;
    writeln(l.value[i+1]);
end;

var
    l : list;
    i, pos : integer;
begin
    l.length := 0;
    for i := 1 to 10 do begin
        push(l,i);
    end;
    printList(l);

    for i := 0 downto -10 do begin
        insert(l, 1, i);
    end;
    printList(l);

    for i := -2 to 4 do begin
        pos := find(l, i);
        delete(l, pos);
    end;
    printList(l);

    for i := 1 to 5 do begin
        pop(l);
    end;
    printList(l);
end.
```

Output:
```
List size 10 with item: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
List size 21 with item: -10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
List size 14 with item: -10, -9, -8, -7, -6, -5, -4, -3, 5, 6, 7, 8, 9, 10
List size 9 with item: -10, -9, -8, -7, -6, -5, -4, -3, 5
```

### Linked list
(language:: pascal)

- Get: O(n)
- Set: O(n)

- Push: O(1)
- Pop: O(n)
- Insert: O(n)
- Find: O(n)
- Delete: O(n)

```pascal
type
    list = record
        head, tail : ^node;
        length : integer;
    end;
    node = record
        value : integer;
        next : ^node;
    end;
    ptrnode = ^node;

procedure listset(var l : list; pos, v : integer);
var
    p : ^node;
    i : integer;
begin
    p := nil;
    for i := 1 to pos do begin
        if p = nil then begin
            p := l.head;
        end
        else begin
            p := p^.next;
        end;
    end;
    p^.value := v;
end;

procedure _get(l : list; pos : integer; var element : ptrnode);
var
    p : ^node;
    i : integer;
begin
    p := nil;
    for i := 1 to pos do begin
        if p = nil then
            p := l.head
        else begin
            p := p^.next;
        end;
    end;
    writeln('Found: ', p^.value);
    element := p;
end;

function get(l : list; pos : integer) : integer;
var
    p : ^node;
    i : integer;
begin
    p := nil;
    for i := 1 to pos do begin
        if p = nil then
            p := l.head
        else begin
            p := p^.next;
        end;
    end;
    get := p^.value;
end;

procedure insert(var l : list; pos : integer; v : integer);
var
    p, temp : ^node;
begin
    if (l.head = nil) or (pos = 1) then begin
        new(temp);
        temp^.value := v;
        temp^.next := l.head;
        l.head := temp;
        if l.tail = nil then begin
            l.tail := l.head;
            l.length := 0;
        end;
        l.length := l.length + 1;
    end
    else begin
        p := nil;
        _get(l, pos-1, p);
        if p <> nil then begin
            new(temp);
            temp^.value := v;
            temp^.next := p^.next;
            p^.next := temp;
            l.length := l.length + 1;
        end;
    end;
end;

function find(l : list; v : integer) : integer;
var
    p : ^node;
    i : integer;
begin
    writeln('Finding ', v);
    p := l.head;
    i := 1;
    while p <> nil do begin
        if p^.value = v then begin
            find := i;
            break
        end;
        i := i + 1;
        p := p^.next;
    end;
end;

procedure delete(var l: list; pos : integer); 
var
    p, temp : ^node;
begin
    p := nil;
    _get(l, pos-1, p);
    if p <> nil then begin
        temp := p^.next;
        p^.next := temp^.next;
        dispose(temp);
        l.length := l.length - 1;
    end;
end;

procedure push(var l : list; v : integer);
begin
    if l.head = nil then begin
        new(l.head);
        l.head^.value := v;
        l.head^.next := nil;
        l.tail := l.head;
        l.length := 1;
    end
    else begin
        new(l.tail^.next);
        l.tail := l.tail^.next;
        l.tail^.value := v;
        l.tail^.next := nil;
        l.length := l.length + 1;
    end;
end;

function pop(var l : list) : integer;
var
    p, temp : ^node;
begin
    p := nil;
    _get(l, l.length - 1, p);
    temp := p^.next;
    p^.next := nil;
    pop := temp^.value;
    l.length := l.length - 1;
    dispose(temp);
end;

procedure printList(l : list);
var
    p : ^node;
begin
    write('List size ', l.length, ' with item: ');
    p := l.head;
    write(p^.value);
    p := p^.next;
    while p <> nil do begin
        write(', ');
        write(p^.value);
        p := p^.next;
    end;
    writeln;
end;

var
    l : list;
    i, pos : integer;
begin
    l.length := 0;
    for i := 1 to 10 do begin
        push(l,i);
    end;
    printList(l);

    for i := 0 downto -10 do begin
        insert(l, 1, i);
    end;
    printList(l);

    for i := -2 to 4 do begin
        pos := find(l, i);
        delete(l, pos);
    end;
    printList(l);

    for i := 1 to 5 do begin
        pop(l);
    end;
    printList(l);
end.
```

Output:

```
List size 10 with item: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
List size 21 with item: -10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
List size 14 with item: -10, -9, -8, -7, -6, -5, -4, -3, 5, 6, 7, 8, 9, 10
List size 9 with item: -10, -9, -8, -7, -6, -5, -4, -3, 5
```