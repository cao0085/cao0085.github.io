---
title: "JAVA Array & Map"
date: 2024-09-14
categories: code-notes
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 之前只學過 Python JS應用，讀 JAVA Leetcode 有點障礙，了解一下 JAVA 的資料結構型態

<!-- 正文 -->

### Array

<br>


##### 1. int[] : 有序且資料型態 & 長度固定
這是 Java 中最嚴格的陣列類型，元素的資料型態和陣列的長度在初始化時就固定了，無法改變。
- 特性
  - 資料型態：必須是單一的基本型態（如 `int`、`double`、`char`）。
  - 長度：一旦宣告，長度不能改變。
  - 有序性：按照插入順序存儲，固定長度。

```java
int[] numbers = {1, 2, 3, 4}; // 固定長度且型態為整數
```

<br>
<br>

##### 2. Object[] : 有序且長度固定
Object[] 是一個靜態陣列且長度固定，但它允許存儲不同類型的物件。
- 特性：
  - 資料型態：可以是多種類型的物件（`Object` 及其子類）。
  - 長度：一旦宣告，無法改變長度。
  - 有序性：按照插入順序存儲。

```java
Object[] objects = new Object[3];
objects[0] = "Java";
objects[1] = 42;
objects[2] = 3.14;
```

<br>
<br>

#### 3. ArrayList : 有序且資料型態靈活
ArrayList 是一個動態陣列，長度可隨資料的增減自動調整，並且支援泛型，使資料型態可以靈活指定。
- 特性：
  - 資料型態：支援單一型態（可用泛型指定）或 `Object` 來接受多種型態。
  - 長度：動態調整，隨資料增減變化。
  - 有序性：按照插入順序存儲。

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
```

<br>
<br>

##### 4. HashSet : 無序且資料型態靈活
HashSet 是一個集合，它不保證元素的插入順序，並且不允許重複元素。資料型態可以靈活使用泛型。
- 特性：
  - 資料型態：使用泛型指定型態，可以是 `Object` 或具體的類型。
  - 長度：動態調整。
  - 無序性：元素的存儲順序不保證。

```java
HashSet<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple"); // 重複的元素不會被加入
```
<br>
<br>

---

### Map

在 Java 中，`Map` 是一個非常重要的資料結構，專門用來儲存鍵值對（key-value pair）。不同於陣列或集合，`Map` 的資料是通過唯一的鍵來存取對應的值，而不是基於索引位置。

<br>

##### 1. Hashtable：無序且鍵值型態固定，具同步性
`Hashtable` 是一個鍵值對集合，鍵和值的資料型態是固定的（泛型）。`Hashtable` 不允許 `null` 鍵或 `null` 值，而且它是線程安全的（具同步性），這意味著在多個執行緒同時操作它時不會出現問題。  

- 特性：
  - 資料型態：鍵值的型態必須固定，通常透過泛型來指定。
  - 無序性：`Hashtable` 不保證插入的順序，鍵值對的順序是無序的。
  - 同步性：線程安全的集合，適合多執行緒情境。
  
  ```java
  Hashtable<String, Integer> table = new Hashtable<>();
  table.put("Apple", 3);
  table.put("Banana", 5);
  ```

<br>
<br>

##### 2. HashMap：無序且鍵值型態靈活
`HashMap` 是最常用的 Map 實現之一，允許使用 `null` 鍵或 `null` 值，但它是無序的。鍵和值的型態可以透過泛型靈活設定。`HashMap` 不是線程安全的，因此在多執行緒情況下需要特別小心。

- 特性：
  - 資料型態：鍵和值的資料型態可以不同，並且可以為 `null`。
  - 無序性：不保證插入順序，資料存取順序也可能與插入順序不同。
  - 靈活度：適合需要快速查找的情境，因為其查找速度通常是常數時間 O(1)。
  
  ```java
  HashMap<String, String> map = new HashMap<>();
  map.put("name", "Alice");
  map.put("city", "Taipei");
  ```

<br>
<br>

##### 3. LinkedHashMap：有序且鍵值型態靈活
`LinkedHashMap` 與 HashMap 類似，唯一的區別是它保留了插入順序，也就是說，當你遍歷 `LinkedHashMap` 時，元素會按照插入的順序返回。這在某些需要保持資料順序的情境下非常有用。
- 特性：
  - 資料型態：靈活，可以是任何型態，並允許 `null`。
  - 有序性：保留插入順序，適合需要按插入順序進行遍歷的場景。
  
  範例：
  ```java
  LinkedHashMap<String, Integer> lmap = new LinkedHashMap<>();
  lmap.put("First", 1);
  lmap.put("Second", 2);
  lmap.put("Third", 3);
  ```

<br>
<br>

##### 4. TreeMap：有序且自動排序，鍵值型態靈活
`TreeMap` 是有序的 Map，它基於紅黑樹實現，元素會按照鍵的自然順序（或提供的比較器）進行排序。`TreeMap` 適合需要按順序操作鍵值對的情況。
- 特性：
  - 資料型態：鍵和值的型態靈活，但鍵必須是可比較的（例如實現 `Comparable` 介面）。
  - 有序性：`TreeMap` 根據鍵的自然順序排序，也可以使用自定義比較器來控制排序。

  ```java
  TreeMap<String, Integer> tmap = new TreeMap<>();
  tmap.put("Banana", 2);
  tmap.put("Apple", 3);
  tmap.put("Pear", 1); // 自動按鍵進行排序
  ```

<br>
<br>

### 總結

<br>

1.int[]：有序且資料型態與長度固定  
2.Object[]：有序且長度固定，但資料型態靈活  
3.ArrayList：有序且支援動態長度與資料型態靈活  
4.HashSet：無序且資料型態靈活  

<br>


1.Hashtable：無序且具同步性，適合多執行緒環境。  
2.HashMap：無序但性能高，適合鍵值靈活且不要求有序的場景。  
3.LinkedHashMap：有序且保留插入順序，適合需要遍歷順序與插入順序一致的應用。  
4.TreeMap：自動排序，適合需要有序鍵值對並且要求鍵可排序的情況。  

