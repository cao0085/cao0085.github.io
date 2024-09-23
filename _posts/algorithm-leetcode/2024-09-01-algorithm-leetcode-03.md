---
title: "Algorithm & LeetCode (3)"
date: 2024-09-01
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。


<!-- #### []()


*** -->

#### [1688. Count of Matches in Tournament](https://leetcode.com/problems/count-of-matches-in-tournament/description/)

```python
def function_one(n):
    games = 0
    while n != 1:
        if n % 2 != 0:
            n = (n-1)//2
            games += n
            n += 1
        else:
            n = n // 2
            games += n
    return games
```


*** 

#### [2181. Merge Nodes in Between Zeros](https://leetcode.com/problems/merge-nodes-in-between-zeros/description/)
滿單純的題目。  
實際應用情境：在處理分段數據或壓縮數據流時很常見，  

```python
def function_one(n):
    new_n = []
    current_value = 0
    # 遍歷整個列表 n
    for j in n:
        if j == 0:
            if current_value != 0:  # 只有在有累積值時才添加
                new_n.append(current_value)
                current_value = 0  # 重置累加值
        else:
            current_value += j  # 累加非零值
    
    return new_n

```
***

#### [2182. Construct String With Repeat Limit](https://leetcode.com/problems/construct-string-with-repeat-limit/description/)
Greedy Algorithm 太難了**待處理**，目前還會有若尾數 > repeatLimit 的問題要處理
像是 caaaa -> aacaa 才對 

概念：選擇前2個最大字母，組合成一個符合規定的字串放入list，

```python
def repeatLimitedString(s: str, repeatLimit: int) -> str:
    # Step 1: 計算字符頻率
    from collections import Counter
    char_count = Counter(s)
    
    # Step 2: 按字典序排序字符（從大到小）
    sorted_chars = sorted(char_count.keys(), reverse=True)
    
    # Step 3: 構建結果字串
    result = []
    
    while sorted_chars:
        # 取當前最大字符
        current_char = sorted_chars[0]
        # 可以使用的次數
        current_count = min(char_count[current_char], repeatLimit)
        
        # 添加字符到結果字串中
        result.extend(current_char * current_count)
        
        # 減少字符的頻率
        char_count[current_char] -= current_count
        
        # 如果當前字符仍然有剩餘，且後面還有其他字符可以插入來打破連續限制
        if char_count[current_char] > 0:
            if len(sorted_chars) > 1:
                # 插入下一個較小的字符來打破連續
                next_char = sorted_chars[1]
                result.append(next_char)
                
                # 減少插入字符的頻率
                char_count[next_char] -= 1
                
                # 如果插入字符的頻率為0，則從排序列表中移除
                if char_count[next_char] == 0:
                    sorted_chars.pop(1)
            else:
                # 如果沒有更多字符可以插入，結束構建
                break
        
        # 如果當前字符的頻率已經用完，從排序列表中移除
        if char_count[current_char] == 0:
            sorted_chars.pop(0)
    
    # 將結果列表轉換為字符串並返回
    return ''.join(result)

# 測試範例
s = "cczazcc"
repeatLimit = 3
print(repeatLimitedString(s, repeatLimit))  # Output: "zzcccac"
```
***
#### [2325. Decode the Message](https://leetcode.com/problems/decode-the-message/description/)
題目核心:先判定key是否為重複值，若為否的話從a-z找出相對應的value。  
python、JS、Java 實現 set()、 a-z 差滿多的。
若應用在爬蟲的話，有可能應用在防止網絡爬蟲的反爬措施

Python：
```python
def function_one(key, message):
    # Step 1: Create a substitution table from the key
    substitution_table = {}
    seen = set()  # To track the letters we have already added
    current_letter = 'a'  # Start aligning with the English alphabet from 'a'

    # Iterate through the key and create the substitution table
    for char in key:
        if char.isalpha() and char not in seen:
            substitution_table[char] = current_letter
            seen.add(char)
            # Move to the next letter in the alphabet
            current_letter = chr(ord(current_letter) + 1)

    # Step 2: Decode the message using the substitution table
    decoded_message = []

    for char in message:
        if char == ' ':
            decoded_message.append(' ')
        else:
            decoded_message.append(substitution_table[char])

    return ''.join(decoded_message)
```
***

#### [2236. Root Equals Sum of Children](https://leetcode.com/problems/root-equals-sum-of-children/description/)
題目很簡單所以延伸詢問實際應用場景:  
1.數據驗證：檢查抓取的數據是否符合某種規則。例如某個商品的總價是否等於部件總和。  
2.內容一致性檢查：驗證網頁上顯示的統計數據是否與各個子項數據相符。  
3.動態數據監控：在一些動態網頁中可能需要監控某些元素的變化，確保這些變化符合預期規則。比如，在股市行情的網頁中，主指數應該等於若干子指數的和，如果不相符可能意味著抓取錯誤或者網頁數據更新有問題。  

- 購物網站
  - 商品價格：29 元（商品價格元素的 XPath 或 CSS 選擇器 #product-price）
  - 運費：60 元（運費元素的 XPath 或 CSS 選擇器 #shipping-fee
  - 總金額：89 元（總金額元素的 XPath 或 CSS 選擇器 #total-price）
  - 目標: 確認網頁上顯示的總金額是否等於商品價格和運費的和。

```python 
from bs4 import BeautifulSoup
import requests

# 假設目標網頁的 URL
url = "https://example.com/product-page"

# 發送 GET 請求到網頁
response = requests.get(url)

# 解析網頁內容
soup = BeautifulSoup(response.content, 'html.parser')

# 抓取商品價格、運費和總金額
product_price = float(soup.select_one('#product-price').text.strip().replace('元', ''))
shipping_fee = float(soup.select_one('#shipping-fee').text.strip().replace('元', ''))
total_price = float(soup.select_one('#total-price').text.strip().replace('元', ''))

# 驗證總金額是否正確
if total_price == product_price + shipping_fee:
    print("總金額正確！")
else:
    print("總金額不正確！")
```
就可以驗證此筆資料是否為有效數據

***

#### [2535. Difference Between Element Sum and Digit Sum of an Array](https://leetcode.com/problems/difference-between-element-sum-and-digit-sum-of-an-array/description/)
把list相加、取出個別list值轉成字串for loop相加後比對。


***

#### [2006. Count Number of Pairs With Absolute Difference K](https://leetcode.com/problems/count-number-of-pairs-with-absolute-difference-k/description/)
用類似九九乘法表 + if 條件來暴力解題
```python 
def function_one(nums, k):
    count = len(nums)
    correct_pair = 0
    for i in range(count):
        for j in range(i + 1, count):  # 這裡從 i+1 開始，確保 i < j
            if abs(nums[i] - nums[j]) == k:  # 使用絕對值來計算差異
                correct_pair += 1
    print(correct_pair)
    return correct_pair
```
Hash Method：  
1.先建立一個字典來記錄當下 for loop 值，並儲存出現次數。  
2.計算的方法是把 for loop 選到的值 - 符合目標的 dic[] 值。  
3.假設為目標k = 2 (3-1 =2 , 3-5 =-2)。    
4.那就要找出目前 dic 內 dic[1],dic[5] 的值當做成對的數量。  

```python
def function_one(nums, k):
    count_dict = {}
    correct_pair = 0
    for num in nums:
        # 找出num2 , if find pair count++ 
        # num1 - num2 = k or num2 - num1 = k -> num2 = num1+k 
        if num - k in count_dict:
            correct_pair += count_dict[num - k]
        if num + k in count_dict:
            correct_pair += count_dict[num + k]

        # 處理過的值放入dic,更新出現次數
        if num in count_dict:
            count_dict[num] += 1
        else:
            count_dict[num] = 1

    print(correct_pair)
    return correct_pair
```

***

#### [202. Happy Number](https://leetcode.com/problems/happy-number/description/)
題目的結束條件是 1. HappyNum 2. 無限循環，當遇到重複的數就是進入無限循環。

- 在網頁爬蟲中的可能應用場景
  - 循環檢測:在爬取網頁時，如果爬蟲不小心進入了循環鏈接（例如網站上有多個頁面互相連接），這可能導致爬蟲無限地爬取相同的頁面。就像快樂數中的「循環檢測」一樣，我們可以使用類似的方法來檢查已經訪問過的 URL。如果一個 URL 已經被訪問過，爬蟲應該停止爬取該頁面，避免進入無窮循環

  - 避免重複資料抓取：在爬取動態網站或具有大量重複內容的網站時，爬蟲可以使用一個集合或列表來追蹤已經抓取過的內容（例如：文章 ID、圖片 URL）。如果一個資料已經出現在追蹤集合中，則爬蟲可以避免重新抓取該內容。這類似於在快樂數問題中，追蹤出現過的數字以避免進入無窮循環。

  - 深度限制 : 在一些爬蟲應用中，我們需要設置深度限制（即爬取的頁面層數）。如果一個爬蟲達到設定的深度限制，則應該停止爬取。這可以視為一種防止無限循環的方式。快樂數問題中的「達到 1 就停止」類似於設定一個目標或限制條件來停止操作。

```python
def function_one(nums):
    # Check the input number
    if nums <= 0:
        print("Please enter a positive number")
        return
    appeared_word = []

    # Infinite check while nums > 1
    while nums > 1:
        numList = []
        sum = 0

        # Calculate the value according to the problem's formula
        while nums > 0:
            x = nums // 10  # Quotient
            z = nums % 10   # Remainder
            sum += z ** 2
            nums = x
        nums = sum

        # Check if the value meets the condition
        if nums == 1:
            return True
        if nums in appeared_word:
            return False
        
        # If none of the conditions are met, record the value and return to the while loop
        appeared_word.append(nums)
```

*** 