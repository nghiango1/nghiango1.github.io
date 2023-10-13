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

    | Problem           | Last update                |
| ----------------- | -------------------------- |
| [[50. Pow(x, n)|50. Pow(x, n)]] | 8:52 PM - October 02, 2023 |
| [[Array|Array]]         | 8:52 PM - October 02, 2023 |


- `O(log n)`

    | Problem                                | Last update                |
| -------------------------------------- | -------------------------- |
| [[74. Search a 2D Matrix|74. Search a 2D Matrix]]             | 8:52 PM - October 02, 2023 |
| [[50. Pow(x, n)|50. Pow(x, n)]]                      | 8:52 PM - October 02, 2023 |
| [[33. Search in Rotated Sorted Array|33. Search in Rotated Sorted Array]] | 8:52 PM - October 02, 2023 |
| [[Binary search|Binary search]]                      | 8:52 PM - October 02, 2023 |


- `O(n)`

    | Problem                                                              | Last update                 |
| -------------------------------------------------------------------- | --------------------------- |
| [[746. Min Cost Climbing Stairs|746. Min Cost Climbing Stairs]]                                    | 12:55 AM - October 14, 2023 |
| [[2038. Remove Colored Pieces if Both Neighbors are the Same Color|2038. Remove Colored Pieces if Both Neighbors are the Same Color]] | 12:06 AM - October 14, 2023 |
| [[905. Sort Array By Parity|905. Sort Array By Parity]]                                        | 8:52 PM - October 02, 2023  |
| [[88. Merge Sorted Array|88. Merge Sorted Array]]                                           | 8:52 PM - October 02, 2023  |
| [[92. Reverse Linked List II|92. Reverse Linked List II]]                                       | 8:52 PM - October 02, 2023  |


- `O(n log n)`

    | Problem                                          | Last update                |
| ------------------------------------------------ | -------------------------- |
| [[Quick sort|Quick sort]]                                   | 8:52 PM - October 02, 2023 |
| [[4. Median of Two Sorted Arrays|4. Median of Two Sorted Arrays]]               | 8:52 PM - October 02, 2023 |
| [[338. Counting Bits|338. Counting Bits]]                           | 8:52 PM - October 02, 2023 |
| [[1658. Minimum Operations to Reduce X to Zero|1658. Minimum Operations to Reduce X to Zero]] | 8:52 PM - October 02, 2023 |
| [[148. Sort List|148. Sort List]]                               | 8:52 PM - October 02, 2023 |


- `O(sqrt n)`

    | Problem              | Last update                |
| -------------------- | -------------------------- |
| [[Two Crystal Ball|Two Crystal Ball]] | 8:52 PM - October 02, 2023 |


- `O(n ** 2)`

    | Problem                                | Last update                |
| -------------------------------------- | -------------------------- |
| [[84. Largest Rectangle in Histogram|84. Largest Rectangle in Histogram]] | 8:52 PM - October 02, 2023 |
| [[63. Unique Paths II|63. Unique Paths II]]                | 8:52 PM - October 02, 2023 |
| [[518. Coin Change II|518. Coin Change II]]                | 8:52 PM - October 02, 2023 |
| [[377. Combination Sum IV|377. Combination Sum IV]]            | 8:52 PM - October 02, 2023 |
| [[134. Gas Station|134. Gas Station]]                   | 8:52 PM - October 02, 2023 |


- `O(n ** 3)`

    | Problem | Last update |
| ------- | ----------- |


- `O(2 ** n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n!)`

    | Problem              | Last update                |
| -------------------- | -------------------------- |
| [[77. Combinations|77. Combinations]] | 8:52 PM - October 02, 2023 |
| [[46. Permutations|46. Permutations]] | 8:52 PM - October 02, 2023 |


