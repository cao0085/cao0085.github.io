---
title: "Graph with LeetCode（1）"
date: 2024-09-30
categories: algorithm-leetcode
---

###### Graph (圖) 是一種用來表示一組物件及其之間關係的數據結構。圖由節點 (Nodes) 和邊 (Edges) 組成，邊用來連接節點。圖可以是有向的 (Directed) 或無向的 (Undirected)，在有向圖中，邊有方向性，而無向圖中的邊則沒有方向性。圖的特性使它非常適合用來解決各種複雜的問題，例如路徑尋找、社交網絡分析、網絡流量優化等。

無向圖，有向圖，權重圖

有向圖實現方法：矩陣 or 列表

用列表實現有向圖
```java
import java.util.ArrayList;

public class ListGraph {
    // 宣告一個內含 Integer ArrayList 的 ArrayList，稱為 graphs
    ArrayList<ArrayList<Integer>> graphs;

    // 建構一個包含 v 個節點的 graphs，並且為每個節點創建一個 ArrayList 來存儲相鄰的節點
    public ListGraph(int v) {
        graphs = new ArrayList<>(v);
        for (int i = 0; i < v; i++) {
            graphs.add(new ArrayList<>());
        }
    }

    //  start to end line
    public void addEdge(int start, int end) {
        graphs.get(start).add(end);
    }

    // delete to end line , ((Integer)end) 是要找出對象，不加 Integer 會被判定成 index
    public void removeEdge(int start, int end) {
        graphs.get(start).remove((Integer) end);
    }

} 
```

深度優先 DFS 並不是優先走最深的路徑，依照相鄰的節點向下遞歸遍歷  

實現 DFS  
```JAVA
public class GraphTraversal {
    ListGraph graph;  // 儲存圖的結構
    boolean[] visited;  // 記錄每個節點是否被訪問

    // 構造函數，用來初始化圖和 visited 數組
    public GraphTraversal(ListGraph listGraph) {
        this.graph = listGraph;
        visited = new boolean[listGraph.graphs.size()];
    }

    // 深度優先搜尋 (DFS) 的遞歸實現
    public void DFSTraversal(int v) {
        // 如果當前節點已經被訪問，則返回
        if (visited[v]) return;
        
        // 標記當前節點已被訪問
        visited[v] = true;
        System.out.print(v + " -> ");  // 輸出當前節點

        // 取得當前節點的所有鄰居
        Iterator<Integer> neighbors = graph.graphs.get(v).listIterator();
        // 遍歷每個鄰居節點，並進行遞歸訪問
        while (neighbors.hasNext()) {
            int nextNode = neighbors.next();
            if (!visited[nextNode]) {
                DFSTraversal(nextNode);
            }
        }
    }

    // 遍歷整個圖，執行 DFS
    public void DFS() {
        // 對於圖中每個節點，檢查是否已被訪問，若沒有則執行 DFS
        for (int i = 0; i < graph.graphs.size(); i++) {
            if (!visited[i]) {
                DFSTraversal(i);
            }
        }
    }

    // 主程序示例
    public static void main(String[] args) {
        // 建立一個有 5 個節點的圖
        ListGraph graph = new ListGraph(5);
        
        // 添加邊
        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 3);
        graph.addEdge(2, 4);

        // 創建一個圖遍歷器，並執行 DFS
        GraphTraversal traversal = new GraphTraversal(graph);
        traversal.DFS();  // 進行深度優先遍歷
    }
}
```

BFS 廣度優先實現
```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Iterator;

public class ListGraph {
    // 宣告一個內含 Integer ArrayList 的 ArrayList，稱為 graphs
    ArrayList<ArrayList<Integer>> graphs;

    // 建構一個包含 v 個節點的 graphs，並且為每個節點創建一個 ArrayList 來存儲相鄰的節點
    public ListGraph(int v) {
        graphs = new ArrayList<>(v);
        for (int i = 0; i < v; i++) {
            graphs.add(new ArrayList<>());
        }
    }

    // 添加從 start 到 end 的邊
    public void addEdge(int start, int end) {
        graphs.get(start).add(end);
    }
}

public class GraphTraversal {
    ListGraph graph;  // 儲存圖的結構
    boolean[] visited;  // 記錄每個節點是否被訪問

    // 構造函數，用來初始化圖和 visited 數組
    public GraphTraversal(ListGraph listGraph) {
        this.graph = listGraph;
        visited = new boolean[listGraph.graphs.size()];
    }

    // 廣度優先搜尋 (BFS) 的實現
    public void BFSTraversal(int v) {
        Deque<Integer> queue = new ArrayDeque<>();  // 初始化隊列
        visited[v] = true;  // 標記當前節點已訪問
        queue.offerFirst(v);  // 將起始節點加入隊列

        while (!queue.isEmpty()) {  // 當隊列不為空
            Integer cur = queue.pollFirst();  // 取出隊列中的第一個節點
            System.out.print(cur + " -> ");  // 輸出當前節點

            // 取得當前節點的所有鄰居
            Iterator<Integer> neighbors = graph.graphs.get(cur).listIterator();
            while (neighbors.hasNext()) {
                int nextNode = neighbors.next();
                // 如果鄰居節點尚未被訪問，則將其加入隊列並標記為已訪問
                if (!visited[nextNode]) {
                    queue.offerLast(nextNode);
                    visited[nextNode] = true;
                }
            }
        }
    }

    // 遍歷整個圖，執行 BFS
    public void BFS() {
        // 對於圖中每個節點，檢查是否已被訪問，若沒有則執行 BFS
        for (int i = 0; i < graph.graphs.size(); i++) {
            if (!visited[i]) {
                BFSTraversal(i);
            }
        }
    }

    // 主程序示例
    public static void main(String[] args) {
        // 建立一個有 5 個節點的圖
        ListGraph graph = new ListGraph(5);
        
        // 添加邊
        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 3);
        graph.addEdge(2, 4);

        // 創建一個圖遍歷器，並執行 BFS
        GraphTraversal traversal = new GraphTraversal(graph);
        traversal.BFS();  // 進行廣度優先遍歷
    }
}
```
<!-- #### []()


*** -->

#### [133. Clone Graph](https://leetcode.com/problems/clone-graph/description/)
練習複製一個框架

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val  # Initialize the node's value
        # Initialize the node's neighbors; if not provided, set it to an empty list
        self.neighbors = neighbors if neighbors is not None else []


def cloneGraph(node):
    if not node:
        return None  # If the input node is None, return None

    # Dictionary to store the mapping from original nodes to cloned nodes
    clones = {}

    # Define a recursive function to perform Depth First Search (DFS)
    def dfs(n):
        if n in clones:
            # Return the cloned node if it has already been cloned
            return clones[n]

        # Clone the current node
        clone_node = Node(n.val)
        clones[n] = clone_node  # Map the original node to the cloned node

        # Iterate through all neighbors, clone them and add to the cloned node's neighbors
        for neighbor in n.neighbors:
            clone_node.neighbors.append(dfs(neighbor))

        return clone_node  # Return the cloned node

    # Start the DFS from the given node
    return dfs(node)


# Test case
if __name__ == "__main__":
    # Create the original graph
    n1 = Node(1)  # Create node 1
    n2 = Node(2)  # Create node 2
    n3 = Node(3)  # Create node 3
    n4 = Node(4)  # Create node 4

    n1.neighbors = [n2, n3]  # Node 1's neighbors are Node 2 and Node 3
    n2.neighbors = [n1, n4]  # Node 2's neighbors are Node 1 and Node 4
    n3.neighbors = [n1]      # Node 3's neighbor is Node 1
    n4.neighbors = [n2]      # Node 4's neighbor is Node 2

    # Clone the graph
    cloned_graph = cloneGraph(n1)

    # Function to output the adjacency list of the cloned graph
    def print_graph(node):
        visited = set()  # Set to keep track of visited nodes
        result = []      # List to store the adjacency list representation

        def dfs(n):
            if n in visited:
                return  # Return if the node has already been visited
            visited.add(n)  # Mark the node as visited
            # Append the node's value and its neighbors' values to the result
            result.append((n.val, [neighbor.val for neighbor in n.neighbors]))
            for neighbor in n.neighbors:
                dfs(neighbor)  # Recursive call for each neighbor

        dfs(node)  # Start DFS from the given node
        return result  # Return the adjacency list

    # Output the adjacency list of the cloned graph
    print(print_graph(cloned_graph))
```

