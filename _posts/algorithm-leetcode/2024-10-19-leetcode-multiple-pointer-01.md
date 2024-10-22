---
title: "Multiple Pointers & LeetCode (1)"
date: 2024-10-19
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python & Java。


<!-- #### []()


*** -->

### Pointers


---
#### [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)
題目需要在原本的array操作，把非 0 的數字往前移動(直接取代)，剩餘的位置用 0 填滿。

```python
class Solution:
    def moveZero(self, nums):
        # point_L to Record next non-zero number
        point_L = 0
        # point_R = for loop index
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[point_L] = nums[i]
                point_L += 1

        # Fill the remaining positions with 0
        for i in range(point_L, len(nums)):
            nums[i] = 0

        print(nums)
```




---
#### [1089. Duplicate Zeros](https://leetcode.com/problems/duplicate-zeros/description/)
第一個 for loop 先計算出最終會有幾個 0 , 第二個 for loop 分配位置。

```python
def duplicateZeros(arr):
    zeros_to_duplicate = 0
    length = len(arr) - 1

    for i in range(len(arr)):
        if i > length - zeros_to_duplicate:
            break
        if arr[i] == 0:
            if i == length - zeros_to_duplicate:
                arr[length] = 0
                length -= 1
                break
            zeros_to_duplicate += 1

    for i in range(length - zeros_to_duplicate, -1, -1):
        if arr[i] == 0:
            arr[i + zeros_to_duplicate] = 0
            zeros_to_duplicate -= 1
            arr[i + zeros_to_duplicate] = 0
        else:
            arr[i + zeros_to_duplicate] = arr[i]
```




---
#### []()


---
#### []()


---
#### []()


---