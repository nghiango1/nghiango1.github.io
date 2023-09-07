---
share: true
catagory: Basic
tags:
  - computer_sience
---
# Problem

Given a array `arr`, sort the array (in memory)

```python
arr = [124, 1241, 412, 4, 54 ,5, 34 , 12, 4, 12, 321,3 ,33]
```

# Solve

## Bubble sort
(language:: python) (time:: O(n ** 2))

The ideal is that large value (large bubble) flow to above first.
- After each integration, the biggest value flowing and stay at top, fixed there and the next loop skipping it
- We do the flow simulation, via: From the bottom, to the top, go to each value of array `arr` and swapping it with above value if the value is smaller than it. 

[[Bubble sort - Example white board.canvas|Bubble sort - Example white board]]
![[Pasted image 20230831000545.png|Pasted image 20230831000545.png]]

## Implementation

```python
def bubbleSort(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - i - 1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]


arr = [124, 1241, 412, 4, 54, 5, 34, 12, 4, 12, 321, 3, 33]
bubbleSort(arr)
print(arr)
```