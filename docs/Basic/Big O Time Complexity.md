---
share: true
catagory: "Basic"
tags:
    - computer_sience
---

# Define

We use Big O to express the growing of total Program Instruction need to be done for a algorithm base on it's input.

Key point:
- The total count of input is call `n` 
- Big O specifically ignore/drop any constant.
# Detecting

It not a fully clear, but we can focus on loop appearance in the algorithm.

A example of `O(n)`:

```python
arr = [1, 23, 5, 7, 8, 8, 9, 25, 3, 423, 43, 25, 4, 36, 56, 7, 525, 68,  44, 7, 8, 9, 8, 9, 809, 233, 445, 6, 34, 6]
n = len(arr)
sum = 0
for i in range(n):
   sum += arr[i]
print(sum)
```

We can use a **best case** and **worst case** (or even **average case**) if the loop can be logically end before hand. But we mostly want to use **worst case** to represent the algorithm. 
# Example with problem:

- `O(1)`: It a constant time algorithm that not take into account of input

    | Problem | Last update |
| ------- | ----------- |


- `O(log n)`

    | Problem                    | Last update               |
| -------------------------- | ------------------------- |
| [[Binary search|Binary search]]          | 8:46 PM - August 28, 2023 |
| [[74. Search a 2D Matrix|74. Search a 2D Matrix]] | 8:38 PM - August 28, 2023 |


- `O(n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n log n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(sqrt n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n ** 2)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n ** 3)`

    | Problem | Last update |
| ------- | ----------- |


- `O(2 ** n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n!)`

    | Problem | Last update |
| ------- | ----------- |


