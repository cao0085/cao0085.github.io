---
title: "Linked List with Leetcode（1）"
date: 2024-10-06
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。

```java

```

<!-- #### []()


*** -->

#### [1763. Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring/description/)
```python
class Solution:
    def longestNiceSubstring(self, s: str) -> str:
        if len(s) < 2: return ''
        S = set(s)
        for i in range(len(s)):
            if s[i].swapcase() not in S:
                front = self.longestNiceSubstring(s[:i])
                tail = self.longestNiceSubstring(s[i+1:])
                return max(front, tail, key=len)
        return s
```
---

#### [1812. Determine Color of a Chessboard Square](https://leetcode.com/problems/determine-color-of-a-chessboard-square/description/)

```java
public class JavaLeetcode {
    public static boolean determinecolor(String coordinates) {
        char char1 = coordinates.charAt(0);
        int char2 = coordinates.charAt(1);
        int sum = (int) char1 + char2;
        return sum % 2 != 0;
    }

    public static void main(String[] args) {
        String coordinates = "a2";
        System.out.println(determinecolor(coordinates));

    }
}
```


---
#### [1869. Longer Contiguous Segments of Ones than Zeros](https://leetcode.com/problems/longer-contiguous-segments-of-ones-than-zeros/description/)
```JAVA
public class JavaLeetcode {
    static boolean solution(String s) {
        int maxZeroSegment = findMaxSegment(s, '0');
        int maxOneSegment = findMaxSegment(s, '1');
        return maxOneSegment > maxZeroSegment;
    }

    // Helper method to find the maximum length of contiguous characters
    private static int findMaxSegment(String s, char targetChar) {
        int maxSegmentLength = 0;
        int currentSegmentLength = 0;

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == targetChar) {
                currentSegmentLength++;
                maxSegmentLength = Math.max(maxSegmentLength, currentSegmentLength);
            } else {
                currentSegmentLength = 0; // Reset count when the character changes
            }
        }
        return maxSegmentLength;
    }

    public static void main(String[] args) {
        String s = "1101";
        System.out.println(solution(s)); // Output: true
    }
}
```

---

#### [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

```

