---
title: "JAVA Learning"
date: 2024-10-02
categories: code-notes
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### JAVA 比起Python有點嚴謹，滿多定義好的規則要了解一下

<!-- 正文 -->
---
### 編譯型語言
編譯型語言是一種在執行程式之前，需要將源程式碼轉換成機器可理解的格式的程式語言。Java 是一種廣泛使用的編譯型語言，其執行過程可分為以下幾個步驟：

#### 1. 執行 Java 程式的必要條件
要執行 Java 程式，必須具備以下兩個條件：
- **Java Virtual Machine**：JVM 是一個執行環境，用於解讀和執行 Java 編譯器生成的字節碼。
- **Java Development Kit**：JDK 包含了 Java 編譯器（javac）和 JVM，提供了編譯和執行 Java 程式所需的工具。

#### 2. 編譯過程
當編寫一個 `.java` 檔案（例如 `Document.java`）後，Java 編譯器（javac）會將這個檔案轉換成對應的 `.class` 檔案（例如 `Document.class`）。這個 `.class` 檔案中包含了 Java 的字節碼，這是一種平台無關的中間表示，可以在任何支持 JVM 的環境中執行。

#### 3. 執行過程
完成編譯後，當開發者希望執行 Java 程式時，可以使用命令 `java Document` 來啟動執行。此時，JVM 會讀取 `Document.class` 檔案，解釋裡面的字節碼，並執行相應的指令，最終輸出程式的執行結果。

---

### 變數與資料類型


#### 1. 變數的意義

變數是一個用來儲存「命名記憶體的位置」。宣告一個變數實際上是在為電腦內存中的「某個特定位置」分配「名稱」，這樣就可以通過這個「名稱」來訪問和操作存儲在該位置的數據。

例如，當我們宣告一個整數變數時：

```java
int count; // 宣告一個整數變數
```

編譯器會為變數 `count` 分配一段內存，這段內存能夠存儲一個整數值。變數名稱（如 `count`）就像一個標籤，指向剛剛分配的內存位置。我們可以通過這個名稱來讀取或修改該內存位置的內容：

```java
count = 10; // 將整數值 10 儲存到 count 變數所指向的內存位置
```

#### 2. 資料類型

資料類型（Data Type）是編程語言中用來定義「變數可以存儲的數據種類」。每個資料類型都有其特定的特徵，包括可存儲的值的範圍、佔用的記憶體大小以及支持的運算，資料類型通常可以分為以下幾類：

- **基本資料類型**（Primitive Data Types）：
  - `int`（整數）
  - `double`（雙精度浮點數）
  - `char`（字符）
  - `boolean`（布林值）。
- **複合資料類型**（Composite Data Types），由基本資料類型組合而成：
  - 字串（String）
  - 陣列（Array）
  - 物件（Object）

資料類型的定義影響變數在內存中的排列和操作，使編譯器能夠有效地管理內存和執行運算。

---
### Declaration

在 Java 編程中` Variable Declaration `(變數的宣告)是一個基本且重要的概念。
#### 1. 類型安全性

Java 是一種` Strongly Typed Language `(強類型語言)，每個變數在使用前必須宣告數據類型，這種設計有助於 **避免人為操作失誤** ，假設你嘗試將字串賦值給整數變數，編譯器會及時報錯！

```java
int number;
number = "Hello"; // 這將引發編譯錯誤
```

#### 2. 記憶體管理

變數的類型決定了其在內存中佔用的大小，Java 在編譯期間根據變數的類型為其分配適當的內存、提高了程式的運行效率。例如，`int` 類型的變數通常佔用 4 字節，而 `double` 類型的變數則佔用 8 字節。使 Java 能夠在內存使用上更精確的控制。

#### 3. 編譯器優化

靜態類型讓編譯器能夠在編譯期間進行各種優化。當編譯器清楚每個變數的類型時，可以生成針對這些類型的更高效的機器碼，從而提升程式的執行效率。這使得 Java 在處理大型應用程式時，能夠保持高效的性能。

#### 4. 提高代碼可讀性和可維護性

明確的變數宣告提高了代碼的可讀性。其他開發者可以很容易理解變數的用途和預期的數據類型，這對於團隊協作和維護大型項目尤為重要。清晰的代碼結構使得未來的維護和擴展變得更加簡單。

---




---
### Keywords & Method


#### 1. Keywords

`Keyword` 定義語言的結構和行為。這些` Keyword `遵循特定的語法規則，並且具有預定義的功能。以下是一些常見的關鍵字及其用途：

- **數據類型**：`int`、`double`、`boolean`、`char` 等，定義變數的數據類型。
- **控制結構**：`if`、`else`、`for`、`while` 等，用於控制程式的流向。
- **類和物件**：`class` 用於定義類，`extends` 表示繼承。
- **訪問修飾符**：`public`、`private`、`protected`，控制類和方法的可見性。
- **其他**：`return`、`static`、`void` 等，指定方法的返回類型和功能。


#### 2. Method

如何使用 Keyword 來定義 Method

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```
- 自定義 Method
    - 利用 `keyword` 的`public` 、 `int` 定義 Method 的可見性和返回類型。
    - `add` 是自定義的 `Method` 名稱。
    - `(int a, int b)` 是方法的參數列表，表示這個方法接受兩個整數。


---

### 複合資料類型 & Class

複合資料類型（Composite Data Types）可以理解為由基本資料類型組合而成的資料結構。

#### 基本資料類型
複合資料類型通常包含一個或多個基本資料類型的元素。例如，數組和物件可以包含整數、字串或其他基本資料類型。

#### 方法
複合資料類型（如類和物件）通常會定義方法，這些方法用來操作和管理內部數據。這些方法可以執行特定的操作，如添加、刪除、查詢等。

- 例如 
    - **字串（String）**：由字符（`char`）組成的複合資料類型。它包含許多方法，如 `length()`、`substring()` 等，用於操作字串。

    - **陣列（Array）**：可以存儲固定大小的同類型元素的資料結構。雖然陣列本身不具有方法，但可以使用 `Arrays` 類中的靜態方法（如 `Arrays.sort()`、`Arrays.copyOf()`）來操作陣列。

    - **集合（Collection）**：如 `ArrayList` 和 `HashMap` 等類型，這些類型可以存儲多個物件並提供方法來操作這些物件，如 `add()`、`remove()`、`get()` 等。

    - **物件（Object）**：使用者自定義的資料類型，可以包含多個基本資料類型的屬性和方法。方法封裝了對這些屬性的操作，提供特定的功能。

    ```java
    class Person {
        String name; // 基本資料類型
        int age;     // 基本資料類型

        // 方法
        void introduce() {
            System.out.println("My name is " + name + " and I am " + age + " years old.");
        }
    }
    ```


#### Class & Object  
`Class` 是一種用來定義 `Object` 的模板。當你創建 `Object` 時它將根據 `Class` 的定義來分配內存，並初始化其屬性，讓物件能夠在程序中封裝數據和行為。

<br>

建立 Class
```java
    class Person {
        String name; // 基本資料類型
        int age;     // 基本資料類型
        // 方法
        void introduce() {
            System.out.println("My name is " + name + " and I am " + age + " years old.");
        }
    }
```

<br>

新建 Object  
創建物件：使用 new `keyword`來創建類的實例
```java
public class Main {
    public static void main(String[] args) {
        // 創建 Person 類的物件
        Person person = new Person();
        person.name = "Alice"; // 設定屬性
        person.age = 30;       // 設定屬性
        person.introduce();    // 調用方法
    }
}
```

---
### 執行.JAVA

```java
public class Frist {

    public static void main(String[] args) {
        System.out.println("Hello world");
    }
}
```
若要執行這個 JAVA 文件，我需要把檔案名稱命名為 Frist.java ，再使用 `java Frist` command。
1. JVC（編譯器）會把這個文件轉換成 Frist.Class
2. JVM（虛擬機）會執行 Frist.Class
3. 會找到 public static void main(String[] args) 當作進入點開始執行。

每個檔案只能有一個公開類別（`public class`），而且這個「公開類別的名稱必須與檔案名稱相同」，並且這個公開類別是唯一可以作為程序的進入點（即主方法 `main` 所在的類別）。

- **其他的類別**：
   - **私有類別（`private class`）**：只能在包含它的公開類別內部使用，無法被外部直接訪問。
   - **默認類別（包私有類別）**：如果沒有訪問修飾符，則該類別只能在同一個包內訪問。
   - **保護類別（`protected class`）**：可以在同一包內或其子類別中訪問。
   - **其他公開類別**：如果需要將其他類別公開訪問，它們必須各自放在單獨的檔案中，並且這些檔案的名稱必須與類別名稱相同。

常見的 import java.util.ArrayList 就是調用存放在 java/util 資料夾中的 ArrayList.java 中的 ArrayList Class !

### 總結
1. 更了解什麼是"程式語言"，背後的運作邏輯和不同語言間的不同。
2. 認識 Keywords 和自定義的 Method。
3. 理解` 呼叫 `寫好的函式邏輯，為什麼可以輕鬆使用和使用方法。