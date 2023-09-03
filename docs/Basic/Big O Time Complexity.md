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
| [[Array|Array]]         | 9:34 PM - August 29, 2023  |
| [[50. Pow(x, n)|50. Pow(x, n)]] | 12:01 AM - August 30, 2023 |


- `O(log n)`

    | Problem                                | Last update                |
| -------------------------------------- | -------------------------- |
| [[Binary search|Binary search]]                      | 11:11 PM - August 29, 2023 |
| [[33. Search in Rotated Sorted Array|33. Search in Rotated Sorted Array]] | 11:43 PM - August 29, 2023 |
| [[74. Search a 2D Matrix|74. Search a 2D Matrix]]             | 12:05 AM - August 30, 2023 |
| [[50. Pow(x, n)|50. Pow(x, n)]]                      | 12:01 AM - August 30, 2023 |


- `O(n)`

    | Problem                                                    | Last update                  |
| ---------------------------------------------------------- | ---------------------------- |
| [[338. Counting Bits|338. Counting Bits]]                                     | 7:54 PM - September 01, 2023 |
| [[1326. Minimum Number of Taps to Open to Water a Garden|1326. Minimum Number of Taps to Open to Water a Garden]] | 12:22 PM - August 31, 2023   |
| [[2366. Minimum Replacements to Sort the Array|2366. Minimum Replacements to Sort the Array]]           | 10:56 PM - August 30, 2023   |
| [[Linear search|Linear search]]                                          | 10:31 PM - August 29, 2023   |
| [[225. Implement Stack using Queues|225. Implement Stack using Queues]]                      | 11:46 AM - August 29, 2023   |


- `O(n log n)`

    | Problem                                                    | Last update                  |
| ---------------------------------------------------------- | ---------------------------- |
| [[338. Counting Bits|338. Counting Bits]]                                     | 7:54 PM - September 01, 2023 |
| [[1326. Minimum Number of Taps to Open to Water a Garden|1326. Minimum Number of Taps to Open to Water a Garden]] | 12:22 PM - August 31, 2023   |


- `O(sqrt n)`

    | Problem              | Last update                |
| -------------------- | -------------------------- |
| [[Two Crystal Ball|Two Crystal Ball]] | 11:38 PM - August 30, 2023 |


- `O(n ** 2)`

    | Problem                                  | Last update                |
| ---------------------------------------- | -------------------------- |
| [[Bubble sort|Bubble sort]]                          | 12:06 AM - August 31, 2023 |
| [[63. Unique Paths II|63. Unique Paths II]]                  | 12:04 AM - August 30, 2023 |
| [[84. Largest Rectangle in Histogram|84. Largest Rectangle in Histogram]]   | 12:12 AM - August 30, 2023 |
| [[121. Best Time to Buy and Sell Stock|121. Best Time to Buy and Sell Stock]] | 2:15 PM - August 30, 2023  |
| [[118. Pascal's Triangle|118. Pascal's Triangle]]               | 12:35 AM - August 30, 2023 |


- `O(n ** 3)`

    | Problem | Last update |
| ------- | ----------- |


- `O(2 ** n)`

    | Problem | Last update |
| ------- | ----------- |


- `O(n!)`

    | Problem              | Last update                |
| -------------------- | -------------------------- |
| [[77. Combinations|77. Combinations]] | 12:06 AM - August 30, 2023 |
| [[46. Permutations|46. Permutations]] | 11:56 PM - August 29, 2023 |


