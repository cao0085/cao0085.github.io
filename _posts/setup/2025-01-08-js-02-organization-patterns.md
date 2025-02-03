---
title: "JavaScript（二）：資料加載處理"
date: 2025-01-03
categories: 
# algorithm-leetcode
# code-notes
# setup-basics
---
<!-- 大綱引言 -->
###### 如何提升使用者體驗


#### 無限滾動
哨兵元素是一種隱藏或視覺上不重要的 HTML 元素，它通常放置在滾動容器的末端 (或是在需要觸發載入資料的位置)。用於偵測是否與 viewport (可視區域) 產生交叉。

假設要實現 Facebook 的貼文無限滾動，可能會是設定當前頁面初次加載10筆貼文，然後在底端放入哨兵。當被觸發時呼叫 api 載薪資料到前端，再render到哨兵元素前面

``` js
    return (
        <div>
            <ul>
                {posts.map(post => (
                    <li key={post.id}>{post.title}</li>
                ))}
                {isLoading && <div>Loading...</div>}
                <li ref={sentinelRef}></li>
                {!hasNextPage && <div>No More Data</div> }
            </ul>
        </div>
    );
``` 

#### 資料分頁

初次就加載所有資料，第一次render取1-20筆資料，之後換頁就是選取不同區間的資料
``` js
    const [initialData, setInitialData] = useState([]);
    const [remainingData, setRemainingData] = useState([]);
    const [loading, setLoading] = useState(true);
    const [currentPage, setCurrentPage] = useState(1);
    const [pageSize, setPageSize] = useState(10);
    const [totalPage, setTotalPage] = useState(0)

    useEffect(()=>{
        fetchInitData();
    },[fetchInitData])

  return (
    <div>
      <ul>
        {initialData.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
        <select value={pageSize} onChange={handleChangeSize}>
            <option value={10}>10 per page</option>
            <option value={20}>20 per page</option>
            <option value={50}>50 per page</option>
        </select>
      <button disabled={currentPage === 1} onClick={()=> handlePageChange(currentPage -1 )}>Prev Page</button>
        <span>{currentPage} / {totalPage}</span>
      <button disabled={currentPage === totalPage} onClick={() => handlePageChange(currentPage + 1)}>Next Page</button>
    </div>
  );
```

另一種是後端有設計邏輯的話，就直接指定請求的資料區間，然後替換掉。
