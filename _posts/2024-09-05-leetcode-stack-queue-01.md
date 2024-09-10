---
title: "Stack and Queue with LeetCode（1）"
date: 2024-09-05
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。此次目標是熟練 Stack 和 Queue 資料結構，並應用在遞迴、函數呼叫、撤銷操作、請求處理以及廣度優先搜尋等場景。

### 堆疊 (Stack)

堆疊是一種遵循「後進先出」（LIFO, Last In First Out）原則的資料結構，最後插入的元素會是最先被取出的元素。
   
2. 主要操作:
   - Push: 將元素放入堆疊的頂端。
   - Pop: 移除並返回堆疊頂端的元素。
   - Peek/Top: 查看堆疊頂端的元素但不移除它。
   - IsEmpty: 判斷堆疊是否為空。

3. 用途:
   - 函數呼叫管理: 程式語言的執行使用堆疊來管理函數呼叫（函數調用棧）。
   - 反轉資料: 由於堆疊是LIFO結構，它常被用來反轉資料順序。
   - 括號匹配: 在表達式中檢查括號是否成對出現。

***

#### [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)
每個括號必定成對，而成立的規則符合「後進先出」原則。
1.判斷左or右括號，左的話加入list，右的話開始判斷。  
2.最終列表必為空。


```python 
def function_one(s):
    # Create a dictionary to map opening brackets to their corresponding closing brackets
    brackets_dic = {"(": ")", "[": "]", "{": "}"}
    # Initialize a list to store encountered opening brackets
    save_brackets = []
    
    # Start comparing brackets
    # If the character is a key in the dictionary, add it to the list
    # If not, compare it with the last bracket in the list (save_brackets.pop())
    for i in s:
        if i in brackets_dic:
            save_brackets.append(i)
        else:
            try:
                # Get the last bracket from the list
                last_bracket = save_brackets.pop()
                # Check if the current bracket matches the expected closing bracket
                if i != brackets_dic[last_bracket]:
                    return False
            except IndexError:
                # If there is an attempt to pop from an empty list, it means there is a mismatch
                return False
    
    # After the loop, if all brackets are matched, the list should be empty; if so, return True
    if save_brackets:
        return False
    return True
```

Code Cleanup
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack, pairs = [], {')': '(', ']': '[', '}': '{'}
        for c in s:
            if c in pairs:
                if not stack or stack.pop() != pairs[c]: return False
            else:
                stack.append(c)
        return not stack

# 1.改用右括號當作key更方便
# 2.if not stack or stack.pop() != pairs[c] 取代 try-except
# 3.return not stack 取代 if save_brackets:，若棧不為空返回 False

```

***
#### [155. Min Stack](https://leetcode.com/problems/min-stack/description/)
花了一些時間才看懂題目含義，大概就是執行添加或移除 Stack 值時，同時要追蹤最小值當前值的最小值，所以會有 Main Stack & min Stack 。  

1.Main Stack 就是正常的。  
2.min Stack 可以有很多值，只要維持最新那個插入是 min value。  
3.初始值必定要是空的。

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)

    def pop(self) -> None:
        if self.stack and self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]

```



***

#### [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)
題目是要找出 num 之後"第一個"比 num 還要大的值，最終目標是用 Map 的方式來儲存 next 值，用 for loop 跑出來，理解很久最後用圖想像才會。關鍵 NumList 裡面的值都會要經過存放在 Stack (**暫存區**)後才能做成 Map。

<br>
  
- 迴圈圖像想像：  
    - 抓取 Num 抓起 Stack Top 來比較  
    - 條件判斷
        - (y) Stack Top 丟去 Map 成對且繼續判斷下一個 Stack Top 是否符合條件  
        - (n) Stack Top 放回去 Stack 
    - 把 Num 丟進 Stack 成為 new Stack Top                                   
    - 結束循環後 Stack 剩餘的值被判定成無符合條件用 -1 行成 Map。

```python
def function_one(nums1, nums2):
    # Create a stack to store numbers and a map to record the next greater number
    stack = []
    greater_map = {}

    # Start the first loop
    # When the stack is not empty and the current number is greater than the top of the stack
    # Then, push the current number onto the stack.
    for num in nums2:
        while stack and num > stack[-1]:
            greater_map[stack.pop()] = num
        stack.append(num)

    # Remaining numbers in the stack (those that don't have a next greater number)
    while stack:
        greater_map[stack.pop()] = -1

    # Return the result for nums1
    output = [greater_map[num] for num in nums1]
    return output
```

#### [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)
簡單版本的括號判斷題。

```python
def function_one(s):
    stack = []
    for i in s:
        if stack and stack[-1] == i:
            stack.pop()
        else:
            stack.append(i)
    return ''.join(stack)
```


***

#### [946. Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/description/)
496題的簡單版本，差異在於用一個指針 j 把 popped index 紀錄起來，剩下的就 pushed array 丟進 stack 同時判斷 `stack[-1]` 和 popped 的關係。

```python
def function_one(pushed, popped):
    stack = []
    j = 0
    for num in pushed:
        stack.append(num)
        while stack and stack[-1] == popped[j]:
            j += 1
            stack.pop()
    return not stack
```


***

#### [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)
這次的 Stack 是來存放 nums 的 index，每當找到比較大的值時用`result[index]`取代預設值 -1

```python
def nextGreaterElements(nums):
    n = len(nums)
    result = [-1] * n  # Initialize the result array with -1 as default values
    stack = []  # Stack to store indices of the elements in nums

    # Traverse the array twice to simulate circular behavior
    for i in range(2 * n):
        # While the stack is not empty and the current element is greater than the element
        # at the top of the stack (based on its index), update the result for that index
        while stack and nums[stack[-1]] < nums[i % n]:
            result[stack.pop()] = nums[i % n]  # Update result array with the next greater element
        if i < n:
            stack.append(i)  # Push the index onto the stack in the first pass only

    return result

# Test example
nums = [1, 2, 3, 4, 3]
print(nextGreaterElements(nums))  # Output: [2, 3, 4, -1, 4]
```


***

***


### 佇列 (Queue)

1. **定義**: 佇列是一種遵循「先進先出」（FIFO, First In First Out）原則的資料結構。這意味著最先插入的元素會是最先被取出的元素。

2. **主要操作**:
   - **Enqueue**: 將元素添加到佇列的尾端。
   - **Dequeue**: 移除並返回佇列的前端元素。
   - **Front**: 查看佇列前端的元素但不移除它。
   - **IsEmpty**: 判斷佇列是否為空。

3. **用途**:
   - **排程任務**: 作業系統和應用程式使用佇列來管理多個任務或請求的排程。
   - **廣度優先搜尋 (BFS)**: 圖的廣度優先搜尋算法使用佇列來追踪下一個要訪問的節點。
   - **資料緩衝區**: 在需要有序處理資料的情況下（例如打印隊列、網路請求等），佇列非常有用。

### 總結

- **堆疊** 是一種「後進先出」的結構，適合需要逆序處理資料的情境。
- **佇列** 是一種「先進先出」的結構，適合需要保持資料順序的情境。

這兩種資料結構各有各的應用場景，在不同的程式設計和算法設計中發揮著重要作用。

<!-- #### []()


*** -->





***

#### []()


***