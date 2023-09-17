---
share: true
catagory: Basic
tags:
  - computer_sience
  - python
  - typescript
  - O(log n)
---

# Problem

Find a element `x` in a sorted array `arr`

> For easier to follow:
> 1. We assuming array `arr` is value is sorted from the lowest to highest.
> 2. The array is zero-index (`arr` start from 0)

```python
arr = [1,2, 2, 2, 2, 3, 4, 6, 8, 9, 142, 142, 142, 142, 255, 255, 255, 567, 1275, 1275, 1275, 2547, 2547, 5458, 9722, 92124]
x = 567
```

# Solve

## What happening?

We leverage on the characteristic of a sorted array, where a index `i` element always have bigger or equal value than element with smaller index than `i`.

```python
arr[0] ... <= arr[i-2] <= arr[i-1] <= arr[i] == True
```

and revert

```python
arr[i] ... <= arr[i+1] <= arr[i+2] <= arr[len(arr)-1] == True
```

This mean, if we access a value `<any>` of the `arr`, we know that the finding value `x` is either in the left, or in the right of the `<any>` element.

## Pseudo code

While there is many way to choose the value `<any>` in `arr`, we specifically choose the middle of `arr`. By splitting at the middle, we can eliminate the most of `arr` in one comparing session.

1. Start with the full array `arr`, find the middle of the array `arr`, we calling it `mid` point, with value `arr[mid]`
2. Base on the comparing result of `x` and `arr[mid]`, We narrowing down array `arr` for searching:
    - If `x == arr[mid]`, return `True` as we found `x`
    - If `x < arr[mid]`, `arr` can be narrowing down to `arr[0..mid-1]`, as all value of `arr[mid .. n]` is greater than `x`
    - If `x > arr[mid]`, `arr` can be narrowing down to `arr[mid+1..len(arr) - 1]`, as all value of `arr[0 .. mid]` is less than `x`
3. Repeat the process with our new narrowed array `arr`

This result on a [[Big O Time Complexity|O(log n)]] time complexity
## Actual Implement

### Personal preference
(language:: python) (time:: O(log n))

There is something call [[Off by one error|Off by one error]] causing a lot of debug time, so my general take on binary search is always:

> `(left, right) = (lowest, highest) = (-1, len(arr))` (both left and high is not inclusive in the array from the beginning)

Why? There is several advantage problem related to binary search, that we will stump on:
- Find the first appearance of `x` in array
- Find the last appearance of `x` in array
- Find the all appearance of `x` in array
- Find the position to insert `x` into array so that array `arr` still sorted

By using both `left` and `right` not in the array give me better control on how I want to lane `left` and `right` pointer in to array. This is a example on _Find the all appearance of `x` in array `arr`_:
- `isFound` tell us if `x` in `arr` or not
- Where `lastPosition` tell you the element with the biggest position in `arr` that have `x` value

```python
from math import trunc, log


def binarySearch(arr, x):
    l, r = -1, len(arr)
    n = len(arr)
    logn = trunc(log(n+2,2))+1
    for _ in range(logn):
        m = (l + r) // 2
        if l == r-1:
            break
        if x < arr[m]:
            r = m
        else:
            l = m
    assert l == r-1
    return x == arr[l], l


arr = [1, 2, 2, 2, 2, 3, 4, 6, 8, 9, 142, 142, 142, 142, 255, 255, 255, 567, 1275, 1275, 1275, 2547, 2547, 5458, 9722, 92124]
x = 567

isFound, lastPossition = binarySearch(arr, x)
print(isFound, lastPossition)
```

### Example:

| Problem                                   | Last update                |
| ----------------------------------------- | -------------------------- |
| [[81. Search in Rotated Sorted Array II|81. Search in Rotated Sorted Array II]] | 12:10 AM - August 30, 2023 |
| [[274. H-Index|274. H-Index]]                          | 9:49 AM - August 18, 2023  |
| [[74. Search a 2D Matrix|74. Search a 2D Matrix]]                | 12:05 AM - August 30, 2023 |
| [[84. Largest Rectangle in Histogram|84. Largest Rectangle in Histogram]]    | 12:12 AM - August 30, 2023 |


## Typescript implementation
(language:: typescript) (time:: O(log n))

Not thing fancy, just pain 
- For divining, JavaScript force us to use `(float)` division result, we have to use `Math.floor()` (do not use `Math.trunc()`)
- The implement just focus on finding the value, so it return on found value immediately 

```ts
export default function bs_list(haystack: number[], needle: number): boolean {
    let l = -1;
    let r = haystack.length;
    let m = 0;

    while (true) {
        m = Math.floor((l + r) / 2);
        if (haystack[m] == needle)
            return true;
        if (l == m)
            break;
        if (haystack[m] < needle) {
            l = m;
        } else {
            r = m;
        }
    }

    return false
}
```