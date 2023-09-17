---
share: true
catagory: Basic
tags:
  - computer_sience
  - pascal
  - c
  - O(n log n)
---

# Problem

Given a array `arr`, sort the array (in memory)

```python
arr = [124, 1241, 412, 4, 54 ,5, 34 , 12, 4, 12, 321,3 ,33]
```

# Solve

Pretty much my all in choice when sorting (on every contest).

## Quick sort
(time:: O(n log n))

I some how always done this wrong, the errors I usually get are:
- Either in swap function (not pass by referent)
- Forget to setup target (using `a[m]` directly)
- For get that `j` pointer go backward (`j := j + 1` in either while loop or inner if)
- Quick sort recursive call on wrong parameter (like calling recursion `a[left .. i]`, `a[j .. right]`, `a[i .. j]`) 

### Implementation
(language:: pascal)

```pascal
type
    int8array = array [1..1000] of integer;

procedure swap(var x : integer; var y : integer);
var
    temp : integer;
begin
    temp := x;
    x := y;
    y := temp;
end;


procedure quicksort(var a: int8array; l:integer; r: integer);
var
    m, i, j : integer;
    target : integer;
begin
    if l > r then  exit;
    m := (l + r) div 2;
    target := a[m];

    i := l;
    j := r;
    repeat
        while a[i] < target do i := i + 1;
        while a[j] > target do j := j - 1;
        if i <= j then begin
            swap(a[i], a[j]);
            i := i + 1;
            j := j - 1;
        end;
    until i > j;
    quicksort(a, l, j);
    quicksort(a, i, r);
end;

procedure printArray(var a: int8array; len: integer);
var
    i : integer;
begin
    for i := 1 to len do begin
        write(a[i]);
        write(', ');
    end;
    writeln;
end;

var
    a : int8array;
    i, len : integer;
begin
    len := 1000;
    for i := 1 to len do a[i] := random(10000);

    quicksort(a, 1, len);

    printArray(a, len);
end.
```

(language:: c)
```c
#include <time.h>
#include <stdlib.h>
#include <stdio.h>

void quicksort(int* a, int l, int r) {
    int i = l;
    int j = r;
    if (l > r) return;

    int target = a[(l + r) / 2];
    while (i <= j) {
        while (a[i] < target) i ++;
        while (a[j] > target) j --;
        if (i <= j) {
            int temp = a[i];
            a[i] = a[j];
            a[j] = temp;
            i ++;
            j --;
        }
    }

    quicksort(a, l, j);
    quicksort(a, i, r);
}

void printArray(int* a, int length) {
    for (int i = 0; i < length; i ++) {
        printf("%d, ", a[i]);
    }
    printf("\n");
}

int main() {
    int len = 1000; 
    int a[1000];

    srand(time(NULL)); 
    for (int i = 0; i < len; i ++) {
        a[i] = rand(); 
    }

    quicksort(a, 0, len-1);
    printArray(a, len);
}
```