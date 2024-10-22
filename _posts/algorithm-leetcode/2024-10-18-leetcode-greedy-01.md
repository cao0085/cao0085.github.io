---
title: "Greedy & LeetCode (1)"
date: 2024-10-18
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python & Java 。


<!-- #### []()


*** -->

### Greedy
1.	局部最優選擇：在每一步驟做出對當前最有利的選擇，不去回溯。
2.	可行性：貪心演算法所做的每一步選擇，必須滿足問題的約束條件。
3.	希望能找到全局最優解：在一些特定問題中，這種策略能夠保證最終結果也是全局最優解。

---
#### [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
若當日>前日，Profit++

```java
public class JavaLeetcode {
    public static int solution(int[] price) {
        int maxprofit = 0;
        for (int i = 1; i < price.length; i++) {
            if (price[i] > price[i - 1]) {
                maxprofit += price[i] - price[i - 1];
            }
        }
        return maxprofit;
    }

    public static void main(String[] args) {
        int[] price = { 7, 1, 5, 3, 6, 4 };
        int answer = solution(price);
        System.out.println(answer);
    }
}
```

---

#### [747. Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others/description/)
找到最大＆第二大來判斷

```java
public class JavaLeetcode {
    public static int solution(int[] nums) {
        int largest = -1;
        int second = -1;
        int index = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > largest) {
                second = largest;
                largest = nums[i];
                index = i;
            } else if (nums[i] > second) {
                second = nums[i];
            }
        }
        ;
        if (largest >= 2 * second) {
            return index;
        } else {
            return -1;
        }
    }

    public static void main(String[] args) {
        int[] nums = { 3, 6, 1, 0 };
        int answer = solution(nums);
        System.out.println(answer);
    }
}
```

---

#### [1827. Minimum Operations to Make the Array Increasing](https://leetcode.com/problems/minimum-operations-to-make-the-array-increasing/description/)
從最後一位數開始往前推是否為基數

```java
public class JavaLeetcode {
    public static int solution(int[] nums) {
        // [index] > [index-1]
        int NeedToAddVaule = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i - 1] >= nums[i]) {
                NeedToAddVaule += (nums[i - 1] + 1) - nums[i];
                nums[i] = (nums[i - 1] + 1);
            }
        }
        return NeedToAddVaule;
    }

    public static void main(String[] args) {
        int[] nums = { 1, 5, 2, 4, 1 };
        int answer = solution(nums);
        System.out.println(answer);
    }
}
```

---


#### [1903. Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string/description/)

```java
public class JavaLeetcode {
    public static String solution(String num) {
        for (int i = num.length() - 1; i >= 0; i--) {
            char ch = num.charAt(i);
            if (ch == '1' || ch == '3' || ch == '5' || ch == '7' || ch == '9') {
                return num.substring(0, i + 1);
            }
        }
        return "";
    }

    public static void main(String[] args) {
        String num = "35427";
        String answer = solution(num);
        System.out.println(answer);
    }
}
```

---

#### [55. Jump Game](https://leetcode.com/problems/jump-game/description/)
用 for loop 紀錄目前能到的最遠處，再判斷是否能超過長度。

```java

public class JavaLeetcode {
    public static Boolean solution(int[] nums) {
        // Record can arrive's position
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) {
                return false;
            }
            ;
            int currentReach = i + nums[i];
            maxReach = Math.max(maxReach, currentReach);

            if (maxReach >= nums.length - 1) {
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[] nums = { 3, 2, 1, 0, 4 };
        boolean answer = solution(nums);
        System.out.println(answer);
    }
}
```

---

#### [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)
題目有說明必定能到達最終點，所以不用判斷只需要累加就好
```python
class Solution:
    def jump(self, nums):
        n = len(nums)
        jumps = 0
        currentEnd = 0
        maxReach = 0

        for i in range(n - 1):
            maxReach = max(maxReach, i + nums[i])
            if i == currentEnd:
                jumps += 1
                currentEnd = maxReach

                if currentEnd >= n - 1:
                    break
        return jumps
```




