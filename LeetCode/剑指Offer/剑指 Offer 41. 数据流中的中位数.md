### 剑指 Offer 41. 数据流中的中位数
如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
例如，
[2,3,4] 的中位数是 3
[2,3] 的中位数是 (2 + 3) / 2 = 2.5
设计一个支持以下两种操作的数据结构：
* `void addNum(int num) - 从数据流中添加一个整数到数据结构中。`
* `double findMedian() - 返回目前所有元素的中位数。`
**示例 1：**
```
输入： ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"] [[],[1],[2],[],[3],[]] 
输出：[null,null,null,1.50000,null,2.00000]
```
**示例 2：**
```
输入： ["MedianFinder","addNum","findMedian","addNum","findMedian"] [[],[2],[],[3],[]] 
输出：[null,null,2.00000,null,2.50000]
```
限制：
* `最多会对 addNum、findMedia进行 50000 次调用。`

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    vector<int> ve;
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        ve.insert(lower_bound(ve.begin(),ve.end(),num),num);
    }
    
    double findMedian() {
        int len = ve.size();
        if (len % 2 == 0) return (ve[len/2-1] + ve[len/2]) / 2.0; 
        else return ve[len/2];
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

