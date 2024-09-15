---
title: "Heap with LeetCode（1）"
date: 2024-09-13
categories: algorithm-leetcode
---

###### 紀錄與 AI 協作 LeetCode 當下的思考模式，主要使用Python，並同步理解與 Java、JS 差異和語感。<br> Heap 最常用來實現優先級佇列（Priority Queue），這是一種在許多應用中非常有用的資料結構。優先級佇列不僅根據插入順序來處理資料，還會依照優先順序（例如大小、重要性）來進行處理。堆的這種特性使它非常適合用來解決實時需要進行排序或優先級管理的問題。


<!-- #### []()


*** -->

- 完全二元樹的陣列表示，對於一個節點 i：
  - 左子節點的索引是 2 * i + 1
  - 右子節點的索引是 2 * i + 2
  - 父節點的索引是 (i - 1) // 2

#### [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
Python有內建 heapq 可以直接做最大/最小堆，還是先用手寫的了解概念。  
這題卡在當 Kth 最小建立好後，剩下的數值題目沒有要求做成完全二元樹列，所以用過的數字是可以丟掉的。

```python
import heapq

class KthLargest(object):

    def __init__(self, k, nums):
        """
        初始化 KthLargest 類別
        :type k: int
        :type nums: List[int]
        """
        self.k = k
        self.min_heap = []  # 使用一個最小堆來維持第 k 大的元素
        
        # 將 nums 中的元素加入到堆中
        for num in nums:
            self.add(num)  # 使用 add 函數將元素添加到堆

    def add(self, val):
        """
        添加一個新的測試分數，並返回當前的第 k 大的數字
        :type val: int
        :rtype: int
        """
        heapq.heappush(self.min_heap, val)  # 將新值加入最小堆
        if len(self.min_heap) > self.k:  # 如果堆的大小超過 k
            heapq.heappop(self.min_heap)  # 移除最小的元素
        
        return self.min_heap[0]  # 返回堆頂元素，即第 k 大的元素
```


***

#### [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/description/)  
做一個 Max Heap 恆取最大二個數字，比較後放回去直到 array 為 1 or 0。

```java
import java.util.PriorityQueue;

public class StoneSmashGame {
    public static int lastStoneWeight(int[] stones) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

        for (int stone : stones) {
            maxHeap.add(stone);
        }

        while (maxHeap.size() > 1) {
            int stone1 = maxHeap.poll();
            int stone2 = maxHeap.poll();

            if (stone1 != stone2) {
                maxHeap.add(stone1 - stone2);
            }
        }

        return maxHeap.isEmpty() ? 0 : maxHeap.poll();
    }

    public static void main(String[] args) {
        int[] stones = { 2, 7, 4, 1, 8, 1 };
        System.out.println("最後剩下的石頭重量: " + lastStoneWeight(stones));
    }
}
```

***

#### []()


***