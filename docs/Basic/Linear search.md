---
title: Binary search
share: true
catagory: "Basic"
tags:
    - computer_sience
---

# Problem

Find a element `x` in a array `arr`

> For easier to follow:
> 1. We assuming array `arr` is value is sorted from the lowest to highest.
> 2. The array is zero-index (`arr` start from 0)

```python
arr = [1,2424, 22, 122, 24, 73, 764, 66, 82, 19, 4142, 1442, 12, 1542, 545, 2545, 25, 56, 127, 127, 127, 254, 254, 48, 72, 924]
x = 567
```

# Solve

## Loop from left to right
(language:: typescript) (time:: O(n))

We go one by one, check if element in `arr` have the same value with `x`:
- If found we return it `true`
- If we can't found any after looping through all of `arr` array's elements, we return `false`

```ts
export default function linear_search(haystack: number[], needle: number): boolean {

    for (let i = 0; i < haystack.length; i++) {
        if (haystack[i] == needle)
            return true
    }

    return false
}
```