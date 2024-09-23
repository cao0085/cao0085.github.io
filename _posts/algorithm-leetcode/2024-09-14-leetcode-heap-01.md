---
title: "Heap with LeetCode（1）"
date: 2024-09-13
categories: algorithm-leetcode
---

###### Heap (堆積)最常用來實現 Priority Queue (優先級佇列)，它會根據插入順序來處理資料、依照優先順序來進行處理， Heap 的特性使它非常適合用來解決需要進行排序或優先級管理的問題。


<!-- #### []()


*** -->

### 完全二元樹

Heap 是一種完全二元樹，透過最大堆或最小堆（ parent node is always greater/smaller than or equal to child nodes ）來實現優先級。其中的兩大核心概念：Bubble Up ＆ Bubble Down 是維持堆結構的兩個重要機制，當元素被插入或移除時，這些機制用來確保堆頂保持最大值。

<br>


##### Push & Bubble Up
堆中插入一個新元素會先被放尾部，然後需要通過上浮（bubble up）將它移動到正確的位置，確保父節點大於子節點。

```js
function insertIntoMaxHeap(heap, value) {
    heap.push(value); // Insert new value at the end of the heap
    let childIndex = heap.length - 1; // Index of the newly inserted element
    
    // Bubble up: compare the newly inserted value with its parent
    while (childIndex > 0) {
        let parentIndex = Math.floor((childIndex - 1) / 2);

        // If ... Swap Index or not 
        if (heap[childIndex] > heap[parentIndex]){
            [heap[childIndex], heap[parentIndex]] = [heap[parentIndex], heap[childIndex]]; 
            childIndex = parentIndex;// Move up to the parent position
        } 
        else {
            break;
        }
    }
}
```
<br>

##### Pop & Bubble Down  
當我們從堆中移除最大值（堆頂元素）時，最後一個元素會被移動到堆頂，然後需要通過下沉（bubble down）來將這個元素放到正確的位置，以維持堆的結構。
```js
// HeapifyDown: Maintain the max-heap property by "bubbling down" from the given index
function heapifyDown(array, index, length) {
    let largest = index;
    const left = 2 * index + 1;  // Left child index
    const right = 2 * index + 2; // Right child index

    // Find the largest value among the current node and its children
    if (left < length && array[left] > array[largest]) {
        largest = left;
    }
    if (right < length && array[right] > array[largest]) {
        largest = right;
    }

    // If the largest is not the current node, swap them and continue bubbling down
    if (largest !== index) {
        [array[index], array[largest]] = [array[largest], array[index]];
        heapifyDown(array, largest, length); // Recursively heapify down
    }
}

// Extract the max (root) from the heap and restore max-heap property
function extractMax(array) {
    // The max value is always at the root (index 0)
    const max = array[0];

    // Move the last element to the root and reduce the array size
    array[0] = array.pop();

    // Perform bubble down from the root to restore heap property (use reduced length)
    heapifyDown(array, 0, array.length);  // Use the new length of the array after pop

    return max; // Return the removed max value
}
```

<br>

以上的都是在整理好的 heap 取出或插入，如果要把一個 array 整理成 heap ，最快的方法是『從最後一個父節點開始進行 bubble down 』這是將一個無序的陣列整理成堆的最有效方法之一。
```js
// Heapify: Turn an unsorted array into a max-heap
function heapify(array) {
    const length = array.length;
    const startIdx = Math.floor((length - 2) / 2); // Find the index of the last parent node

    // Start from the last parent node and perform "bubble down" for each node
    for (let i = startIdx; i >= 0; i--) {
        heapifyDown(array, i, length);  // Use the length of the full array
    }
}

```

<br>

補充 : heapifyDown(array, index, length) 的 length 用變數傳入是個安全的做法。



---


#### [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
這題在當 Kth 最小建立好後，剩下的數值題目沒有要求做成完全二元樹列，所以用過的數字是可以丟掉的。

```python
import heapq

class KthLargest(object):

    def __init__(self, k, nums):
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

#### [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/description/)

```python
import collections
import heapq
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        counter = collections.Counter(words)
        top_k = heapq.nsmallest(k, counter.items(),
                                key=lambda x: (-x[1], x[0]))
        return [word[0] for word in top_k]
```

***

#### [1353. Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/description/)  
