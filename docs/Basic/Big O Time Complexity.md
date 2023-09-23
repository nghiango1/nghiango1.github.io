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

    | Problem           | Last update                   |
| ----------------- | ----------------------------- |
| [[Array|Array]]         | 12:38 PM - September 07, 2023 |
| [[50. Pow(x, n)|50. Pow(x, n)]] | 12:01 AM - August 30, 2023    |


- `O(log n)`

    | Problem                                | Last update                   |
| -------------------------------------- | ----------------------------- |
| [[Binary search|Binary search]]                      | 12:33 PM - September 03, 2023 |
| [[33. Search in Rotated Sorted Array|33. Search in Rotated Sorted Array]] | 11:43 PM - August 29, 2023    |
| [[74. Search a 2D Matrix|74. Search a 2D Matrix]]             | 12:05 AM - August 30, 2023    |
| [[50. Pow(x, n)|50. Pow(x, n)]]                      | 12:01 AM - August 30, 2023    |


- `O(n)`

    | Problem                                                    | Last update                   |
| ---------------------------------------------------------- | ----------------------------- |
| [[392. Is Subsequence|392. Is Subsequence]]                                    | 9:39 AM - September 23, 2023  |
| [[1658. Minimum Operations to Reduce X to Zero|1658. Minimum Operations to Reduce X to Zero]]           | 10:12 PM - September 20, 2023 |
| [[92. Reverse Linked List II|92. Reverse Linked List II]]                             | 9:12 AM - September 07, 2023  |
| [[338. Counting Bits|338. Counting Bits]]                                     | 11:40 AM - September 03, 2023 |
| [[1326. Minimum Number of Taps to Open to Water a Garden|1326. Minimum Number of Taps to Open to Water a Garden]] | 11:40 AM - September 03, 2023 |


- `O(n log n)`

    | Problem                                          | Last update                   |
| ------------------------------------------------ | ----------------------------- |
| [[4. Median of Two Sorted Arrays|4. Median of Two Sorted Arrays]]               | 9:27 PM - September 22, 2023  |
| [[1658. Minimum Operations to Reduce X to Zero|1658. Minimum Operations to Reduce X to Zero]] | 10:12 PM - September 20, 2023 |
| [[1337. The K Weakest Rows in a Matrix|1337. The K Weakest Rows in a Matrix]]         | 10:06 AM - September 18, 2023 |
| [[148. Sort List|148. Sort List]]                               | 11:00 PM - September 17, 2023 |
| [[Quick sort|Quick sort]]                                   | 8:07 PM - September 04, 2023  |


- `O(sqrt n)`

    | Problem              | Last update                   |
| -------------------- | ----------------------------- |
| [[Two Crystal Ball|Two Crystal Ball]] | 12:33 PM - September 03, 2023 |


- `O(n ** 2)`

    | Problem                                  | Last update                   |
| ---------------------------------------- | ----------------------------- |
| [[1337. The K Weakest Rows in a Matrix|1337. The K Weakest Rows in a Matrix]] | 10:06 AM - September 18, 2023 |
| [[377. Combination Sum IV|377. Combination Sum IV]]              | 10:57 AM - September 09, 2023 |
| [[Bubble sort|Bubble sort]]                          | 5:53 PM - September 04, 2023  |
| [[63. Unique Paths II|63. Unique Paths II]]                  | 12:04 AM - August 30, 2023    |
| [[134. Gas Station|134. Gas Station]]                     | 6:19 PM - September 17, 2023  |


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


