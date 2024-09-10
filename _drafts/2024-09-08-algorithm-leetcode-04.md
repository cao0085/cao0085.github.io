---
title: "Algorithm & LeetCode (4)"
date: 2024-09-08
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。


<!-- #### []()
```python

```

*** -->

#### [2485. Find the Pivot Integer](https://leetcode.com/problems/find-the-pivot-integer/description/)
求出 x  -> 1 +...+ X = X + ... + N。  
數列中求出一個數讓我聯想到 猜數字 遊戲的二分搜尋法，這題卡最久的就是忘記 while 條件如何設定。 

- 網頁爬蟲中的應用場景:
  - 搜尋特定資料的頁面：如果網站提供按日期排序的分頁數據，你可以使用二分搜尋法快速定位到特定數據所在的頁面。
  - 產品的價格範圍：希望找到特定價格範圍內的產品，網站有多個頁面，每頁顯示一定數量的產品，且產品按價格排序。

<br>

```python
def function_one(n):
    min = 1
    max = n
    while min <= max:
        currentNum = (max+min)//2
        sum1 = sum(range(1, currentNum + 1))  # 從 1 到 currentNum 的總和
        sum2 = sum(range(currentNum, n + 1))  # 從 currentNum 到 n 的總和

        if sum1 == sum2:
            return currentNum
        elif sum1 > sum2:
            max = currentNum
        elif sum1 < sum2:
            min = currentNum

```


*** 

#### []()
```python

```

***