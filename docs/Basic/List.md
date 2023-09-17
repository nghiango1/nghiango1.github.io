---
share: true
catagory: Basic
tags:
  - computer_sience
  - pascal
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

The Big O complexity look like this in a theory (until you consider CPU cache vs Memory accessing and Array List just gain [[Linked list vs Array list|massive advantage(?)]] over Linked list on real hardware).  

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

### Adding validator

A good practice to make the code useable is adding more and more validate input and return error back to the caller. Still for sake of practical Data structure, it better to focus on the core handler, and bloating them/the program it self with data validate. Here I have to wrap every function:
- Every get/delete/pop with a `validateGet`: that check if we do operation inside the List it self.
- Every insert/push with a `validateInsert`: that check if we do operation inside the List or at the end of the List it self, while not overflow our memory (which have max capacity at `100`).

```pascal
type
    list = record
        value : array [1..100] of integer;
        length : integer;
    end;

function validateGet(l : list; pos : integer) : boolean;
begin
    validateGet := (1 <= pos) and (pos <= l.length );
end;

function validateInsert(l : list; pos : integer) : boolean;
begin
    validateInsert := (1 <= pos) and (pos <= l.length + 1) and (l.length < 100);
end;

procedure push(var l : list; v : integer);
begin
    if validateInsert(l, l.length) then begin
        l.length := l.length + 1;
        l.value[l.length] := v;
    end;
end;

function pop(var l : list) : integer;
begin
    if validateGet(l, l.length) then begin
        pop := l.value[l.length];
        l.length := l.length - 1;
    end;
end;

procedure insert(var l : list; pos : integer; v : integer);
var
    i : integer;
begin
    if validateInsert(l, pos) then begin
        for i := l.length downto pos do begin
            l.value[i+1] := l.value[i]
        end;
        l.value[pos] := v;
        l.length := l.length + 1;
    end;
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
    if validateGet(l, pos) then begin
        for i := pos to l.length do begin
            l.value[i] := l.value[i+1];
        end;
        l.length := l.length - 1;
    end;
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

Now what happened, if I need to quickly change the implementation to Linked list.  You can go on and change all of the find/get/set/push/pop/insert into a caller, where it just validate the data and call for the main handle method.

This make it more followable ? But I sure am that we can split/changing code much more quickly.

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
    // writeln('Found: ', p^.value);
    element := p;
end;

procedure _insert(var l : list; pos : integer; v : integer);
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
    // writeln('Finding ', v);
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

procedure _delete(var l: list; pos : integer); 
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

procedure _push(var l : list; v : integer);
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

function _pop(var l : list) : integer;
var
    p, temp : ^node;
begin
    p := nil;
    _get(l, l.length - 1, p);
    temp := p^.next;
    p^.next := nil;
    _pop := temp^.value;
    l.length := l.length - 1;
    dispose(temp);
end;

// Above surface function

function validateGet(l : list; pos : integer) : boolean;
begin
    validateGet := (1 <= pos) and (pos <= l.length );
end;

function validateInsert(l : list; pos : integer) : boolean;
begin
    validateInsert := (1 <= pos) and (pos <= l.length + 1) and (l.length < 100);
end;

function get(l : list; pos : integer) : integer;
var
    p : ^node;
begin
    p := nil;
    if validateGet(l, pos) then begin
        _get(l, pos, p);
        get := p^.value;
    end;
end;

procedure push(var l : list; v : integer);
begin
    if validateInsert(l, l.length + 1) then begin
        _push(l, v);
    end;
end;

function pop(var l : list) : integer;
begin
    if validateGet(l, l.length) then begin
        pop := _pop(l);
    end;
end;

procedure insert(var l : list; pos : integer; v : integer);
begin
    if validateInsert(l, pos) then begin
        _insert(l,pos,v);
    end;
end;

procedure delete(var l: list; pos : integer); 
begin
    if validateGet(l, pos) then begin
        _delete(l, pos);
    end;
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
    l.head := nil;
    l.tail := nil;
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

By doing so, when changing between two List implementation (linked list to array), I could rely on most of validator from all the above surface function, just modify all the main handler (start with `_`).

Diff tree:

```diff
>git diff --no-index linkedListValidate.pas listValidate.pas
diff --git a/linkedListValidate.pas b/listValidate.pas
diff --git a/linkedListValidate.pas b/listValidate.pas
index d346d6e..0d12861 100644
--- a/linkedListValidate.pas
+++ b/listValidate.pas
@@ -1,137 +1,40 @@
 type
     list = record
-        head, tail : ^node;
+        value : array [1..100] of integer;
         length : integer;
     end;
-    node = record
-        value : integer;
-        next : ^node;
-    end;
-    ptrnode = ^node;

-procedure listset(var l : list; pos, v : integer);
-var
-    p : ^node;
-    i : integer;
+procedure _push(var l : list; v : integer);
 begin
-    p := nil;
-    for i := 1 to pos do begin
-        if p = nil then begin
-            p := l.head;
-        end
-        else begin
-            p := p^.next;
-        end;
-    end;
-    p^.value := v;
+    l.length := l.length + 1;
+    l.value[l.length] := v;
 end;

-procedure _get(l : list; pos : integer; var element : ptrnode);
-var
-    p : ^node;
-    i : integer;
+function _pop(var l : list) : integer;
 begin
-    p := nil;
-    for i := 1 to pos do begin
-        if p = nil then
-            p := l.head
-        else begin
-            p := p^.next;
-        end;
-    end;
-    // writeln('Found: ', p^.value);
-    element := p;
+    _pop := l.value[l.length];
+    l.length := l.length - 1;
 end;

 procedure _insert(var l : list; pos : integer; v : integer);
 var
-    p, temp : ^node;
-begin
-    if (l.head = nil) or (pos = 1) then begin
-        new(temp);
-        temp^.value := v;
-        temp^.next := l.head;
-        l.head := temp;
-        if l.tail = nil then begin
-            l.tail := l.head;
-            l.length := 0;
-        end;
-        l.length := l.length + 1;
-    end
-    else begin
-        p := nil;
-        _get(l, pos-1, p);
-        if p <> nil then begin
-            new(temp);
-            temp^.value := v;
-            temp^.next := p^.next;
-            p^.next := temp;
-            l.length := l.length + 1;
-        end;
-    end;
-end;
-
-function find(l : list; v : integer) : integer;
-var
-    p : ^node;
     i : integer;
 begin
-    // writeln('Finding ', v);
-    p := l.head;
-    i := 1;
-    while p <> nil do begin
-        if p^.value = v then begin
-            find := i;
-            break
-        end;
-        i := i + 1;
-        p := p^.next;
+    for i := l.length downto pos do begin
+        l.value[i+1] := l.value[i]
     end;
+    l.value[pos] := v;
+    l.length := l.length + 1;
 end;

 procedure _delete(var l: list; pos : integer);
 var
-    p, temp : ^node;
-begin
-    p := nil;
-    _get(l, pos-1, p);
-    if p <> nil then begin
-        temp := p^.next;
-        p^.next := temp^.next;
-        dispose(temp);
-        l.length := l.length - 1;
-    end;
-end;
-
-procedure _push(var l : list; v : integer);
+    i : integer;
 begin
-    if l.head = nil then begin
-        new(l.head);
-        l.head^.value := v;
-        l.head^.next := nil;
-        l.tail := l.head;
-        l.length := 1;
-    end
-    else begin
-        new(l.tail^.next);
-        l.tail := l.tail^.next;
-        l.tail^.value := v;
-        l.tail^.next := nil;
-        l.length := l.length + 1;
+    for i := pos to l.length do begin
+        l.value[i] := l.value[i+1];
     end;
-end;
-
-function _pop(var l : list) : integer;
-var
-    p, temp : ^node;
-begin
-    p := nil;
-    _get(l, l.length - 1, p);
-    temp := p^.next;
-    p^.next := nil;
-    _pop := temp^.value;
     l.length := l.length - 1;
-    dispose(temp);
 end;

 function validateGet(l : list; pos : integer) : boolean;
@@ -144,17 +47,6 @@ begin
     validateInsert := (1 <= pos) and (pos <= l.length + 1) and (l.length < 100);
 end;

-function get(l : list; pos : integer) : integer;
-var
-    p : ^node;
-begin
-    p := nil;
-    if validateGet(l, pos) then begin
-        _get(l, pos, p);
-        get := p^.value;
-    end;
-end;
-
 procedure push(var l : list; v : integer);
 begin
     if validateInsert(l, l.length + 1) then begin
@@ -176,6 +68,18 @@ begin
     end;
 end;

+function find(l : list; v : integer) : integer;
+var
+    i : integer;
+begin
+    for i := 1 to l.length do begin
+        if l.value[i] = v then begin
+            find := i;
+            break
+        end;
+    end;
+end;
+
 procedure delete(var l: list; pos : integer);
 begin
     if validateGet(l, pos) then begin
@@ -185,18 +89,13 @@ end;

 procedure printList(l : list);
 var
-    p : ^node;
+    i : integer;
 begin
     write('List size ', l.length, ' with item: ');
-    p := l.head;
-    write(p^.value);
-    p := p^.next;
-    while p <> nil do begin
-        write(', ');
-        write(p^.value);
-        p := p^.next;
+    for i := 1 to l.length-1 do begin
+        write(l.value[i], ', ');
     end;
-    writeln;
+    writeln(l.value[i+1]);
 end;
```
