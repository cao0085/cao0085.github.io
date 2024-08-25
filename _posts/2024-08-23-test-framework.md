---
title: "Test Framework"
date: 2024-08-23
categories: code-notes
---


###### Test Framework 通常包含：測試案例編寫、測試執行器、模擬和替身工具、報告與分析。

### 單元測試 (Unit Testing)
針對代碼中的最小可測試單元（函數或方法）進行測試，驗證單個單元的功能是否正確。  

<br>

#### 1.Doctest
測試案例直接嵌入在函數的說明文件中，便於查閱和維護，不需要額外的測試框架或文件，測試案例直接撰寫在 docstring 中。

```python
def multiply_by_two(x):
    """
    接收一個數字，並返回該數字乘以 2 的結果。
    >>> multiply_by_two(3)
    6
    >>> multiply_by_two(0)
    0
    >>> multiply_by_two(-4)
    -8
    """
    return x * 2
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

#### 2.Unittest
優點是可以**同時**測試不同變數和情境...。

<br>

準備兩個檔案(測試和函式)，把函式 A.py 導入執行測試的 test_A.py 內進行測試。  
```python 
# A File
def function_one(num):
    return num + 1
```  

```python  
# test_A File
import unittest
import A File

class MyTest(unittest.TestCase):
    def test_one(self):
        num = 1
        # 調用 A File 裡面的function_one(num)，並放入剛剛定義的變數num
        result = A.function_one(num)
        #比較 result 是否和預訂的值 2 一樣
        self.assertEqual(result, 2) 

if __name__ == "__main__":
    unittest.main()
    # unittest.main() 是用來自動搜索並執行所有在當前模組中定義的測試用例
    # 即所有繼承自 unittest.TestCase 並以 test_ 開頭的方法
```