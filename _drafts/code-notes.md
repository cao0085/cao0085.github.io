---
title: "Algorithm & LeetCode (2)"
date: 2024-08-22
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。


<!-- #### []() -->
#### [1929. Concatenation of Array](https://leetcode.com/problems/concatenation-of-array/description/)

了解各語言的資料存放特性，把兩組資料有序的合併成一組。  
>JavaScript：Array在JS裡相加會轉成Str type,所以要使用「擴展運算符」"...nums"依序放入[]。
>JAVA：須先算出解答的array長度，創立空白array後再使用for loop放入。

***

#### [2011. Final Value of Variable After Performing Operations](https://leetcode.com/problems/final-value-of-variable-after-performing-operations/description/)

Key & Vaule 配對後，用 For Loop 計算最終值。
>JAVA： Hash map 的 key & Vaule 須維持一個type。

***

#### [2942. Find Words Containing Character](https://leetcode.com/problems/find-words-containing-character/description/)  

題目是此單字有無(Ture)關鍵字，所以可以使用無序且取出不重複的set( )快速查找。若需計算關鍵字數量的話使用Nested For Loop。

```python
# Python
    def function(words, target):
        output = []
        for index, word in enumerate(words):
            if target in set(word):
                output.append(index)
        print(output)
        return output
```

***

#### [1470. Shuffle the Array](https://leetcode.com/problems/shuffle-the-array/description/)

```python
# Python
    def function(nums, n):
        ans = []
        for i in range(n):
            ans.append(nums[i])
            ans.append(nums[i+n])
        ans.extend(nums[2 * n:]) # 如果還有剩下的數字
        print(ans)
```

***

#### [2824. Count Pairs Whose Sum is Less than Target](https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target/description/)

題目給的nums是無序排列，可使用For Loop列出所有組合比較。若是有序的nums可以用 Two Point 方法減少運算時間。

```python
# Python for loop
def function(nums, target):
    count = 0
    for index, value1 in enumerate(nums):
        for value2 in nums[index+1:]:
            total = value1+value2
            if total < target:
                count += 1
    print(count)
    return count
```
```python
# Python Two Point
def function2(nums, target):
    nums.sort()  # 首先對數組進行排序
    # 設定左右指針和計數器
    left = 0
    right = len(nums) - 1
    count = 0
    # 當左指針 < 右指針時，不斷執行循環
    # 可以將左指針視為外層 for 迴圈的基準 i，右指針視為內層 for 迴圈的變數 j
    while left < right:
        if nums[left] + nums[right] < target:
            # 當 nums[left] + nums[right] < target
            # 時從左指針到右指針之間的所有數對都滿足條件
            count += right - left
            # 將左指針右移一位，類似於外層 for 迴圈中 i 的下一次迭代
            left += 1
        else:
            # 若 nums[left] + nums[right] >= target，則需要減少右指針的值
            # 這相當於內層 for 迴圈中嘗試減小變數 j 的值
            right -= 1
    
    print(count)
    return count
```

