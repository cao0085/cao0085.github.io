---
title: "Algorithm & LeetCode (2)"
date: 2024-08-25
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。


<!-- #### []()


*** -->

#### [1720. Decode XORed Array](https://leetcode.com/problems/decode-xored-array/description/)

了解XOR運算邏輯，使用環境(數據加密解密、數據壓縮與編碼、錯誤檢測與校正、異或樹（XOR Tree）問題、密碼學應用...)  

<br>

題目算法是用 arr[i] XOR encoded[i] 來得到  arr[i+1]，然後我們已知arr[0] = first 就可以求出來
```python 
def function_one(encoded, first):
    arr = [first]
    for vaule in encoded:
        arr.append(vaule ^ arr[-1])
        #當跑到encoded[1]時arr長度也會是[index,index1]，所以可直接取arr的最後一項運算

    return arr
```

***


#### [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/description/)
因為羅馬值判讀只和後項有關，用 Greedy Algorithm 局部最優選擇、不回溯特性解題。
先做一個dic把羅馬字相對應到的值列出，羅馬數值的規律是 若當前的字母轉換後的值 > 後項，則為負數。  


```python
# 我的code，可以再簡化但這樣我自己才看得直觀
def function_one(s):
    # 列出對應的值
    roman_to_int = {'I': 1,'V': 5,'X': 10,'L': 50,'C': 100,'D': 500,'M': 1000}
    
    # 先把"每個"字母對應的值轉換成list
    value_list = []
    for char in s:
        value_list.append(roman_to_int[char])

    # 用value_list[index]的方法+if判段，取出對應的值算出總和
    value = 0
    n = len(value_list)
    for i in range(n-1):
        # 判斷 當前的值 是要加法還是減法

        # 若前項目<後項目，則此值為負數。齊於都加
        if value_list[i] < value_list[i + 1]:
            value -= value_list[i]
        else:
            value += value_list[i]
    # 最後一位數永遠為正數，加回去
    value += value_list[-1]

    #ＧＰＴ的Greedy Algorithm解法，差別是一率先加，再判斷前後項關係if ture 扣回去
    total = 0
    for i in range(len(s)):
        # 先加上當前字符的值
        total += roman_values[s[i]]
        # 如果當前字符值大於前一個字符值，則減去兩倍的前一個字符值（因為之前已經加過一次，所以要再減去兩次）
        if i > 0 and roman_values[s[i]] > roman_values[s[i - 1]]:
            total -= 2 * roman_values[s[i - 1]]

    return total
    return value
```

***

#### [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/description/)


```python
def function_one(strs):
    if not strs:
        return ""

    # 以第一個字符串作為初始前綴
    prefix = strs[0]

    for i in range(len(prefix)):
        for string in strs[1:]:
            # 如果當前字符串長度不足
            # 或字符不匹配
            if i == len(string) or string[i] != prefix[i]:
                return prefix[:i]
    #若完全一樣
    return prefix
```

GPT優化後更直接，找出最小和最大字串來比較。
```python
def function_one(strs):
    if not strs:
        return ""

    # 找到最小和最大字符串
    min_str = min(strs)
    max_str = max(strs)

    # 比較最小和最大字符串的字符來決定最長公共前綴
    for i in range(len(min_str)):
        if min_str[i] != max_str[i]:
            return min_str[:i]

    return min_str

# 測試
strs = ["flower", "flow", "flight"]
print(function_one(strs))  # 輸出: "fl"
```

***

#### [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)
題目規則理解後 1.字串一定為偶數、2.括號一定是成對的、3.符合某種排列方式。  
- 我的解題核心：  
  1. 每遇到一個左括號，就放入對應的右括號在暫存表等待消除。  
  2. 每遇到一個右刮號，從暫存表的index[-1]提出來比較。

- Stack Algorithm
  - 後進先出（LIFO）
        - 堆疊算法的核心特性是「後進先出」（Last In, First Out, LIFO）
  - 操作簡單 
        - 堆疊支持兩種基本操作：push（將元素壓入堆疊）和 pop（將元素彈出堆疊）。這些操作的時間複雜度都是 O(1)
  - 逆向思考
        - 堆疊的特性使其適合逆向思考的問題，例如在處理回溯算法或逆序操作時（例如計算機語言的編譯器、解析器中）。
  - 應用場景
        - 括號匹配、撤銷（Undo）、棧排序、深度優先搜索（DFS）。

<br>

```python
def function_one(s):
    dic = {"(": ")", "[": "]", "{": "}"}
    count = len(s)
    closeB_storage = []
    if count % 2:
        print("字串是奇數")
        return
    else:
        for char in s:
            # 如果當前字母是左括號(dic key)
            # 把對應到的右括號暫存closeB_storage
            if char in dic:
                closeB_storage.append(dic[char])

            # 如果當前字母是右括號(dic value)
            # closeB_storage最右邊的一位彈出/比較
            else:
                n = closeB_storage.pop()
                if n != char:
                    print("N")
                    return False
    return True
```


***

#### [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
題目是要總共有幾個元素(類似set())，且要符合排序+原本的array修改。我使用 Two-Pointer Technique 當作核心，但不會原地修改只做成新建list+排序。


```python
def function_one(s):
    # 設立一個基準點(左指針)，被左指針指到的值加入new_s
    left_index = 0
    new_s = [s[left_index]]

    # 左指針先固定，右指針開始跑遍字串
    # 若左指針值 = 右指針值 繼續跑
    # 若不一樣時，把左指針位置換成當前右指針位置
    for right_index, current_value in enumerate(s):
        if s[left_index] != current_value:
            left_index = right_index
            new_s.append(s[left_index])
    return len(new_s)
```

ChatGPT  更正版本(原array修改)：  
1.直接覆蓋原本的array  
2.left_index變成計步器的概念
```python
def function_one(nums):
    if not nums:  # 如果數組是空的，返回0
        return 0

    left_index = 0  # 初始化左指標，指向數組的第一個位置

    # 遍歷數組的每個元素
    for right_index in range(1, len(nums)):
        if nums[left_index] != nums[right_index]:
            left_index += 1  # 移動左指標到下一個位置
            nums[left_index] = nums[right_index]  # 將新元素移動到左指標位置

    return left_index + 1  # 返回唯一元素的數量
# 示例測試
s = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4, 6]
result = function_one(s)
print(result)  # 輸出：6
print(s[:result])  # 輸出：[0, 1, 2, 3, 4, 6]
```


***