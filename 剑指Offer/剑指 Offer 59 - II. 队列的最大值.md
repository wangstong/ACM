### 剑指 Offer 59 - II. 队列的最大值
请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。
若队列为空，pop_front 和 max_value 需要返回 -1
**示例 1：**
```
输入: ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"] 
[[],[1],[2],[],[],[]] 
输出: [null,null,null,2,1,2]
```
**示例 2：**
```
输入: ["MaxQueue","pop_front","max_value"] [[],[],[]] 
输出: [null,-1,-1]
```
**限制：**
* `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
* `1 <= value <= 10^5`
```cpp
class MaxQueue {
public:
    queue<int> que;
    deque<int> mx_que;
    MaxQueue() {

    }
    
    int max_value() {
        if (que.empty()) return -1;
        return mx_que.front();
    }
    
    void push_back(int value) {
        que.push(value);

        while (!mx_que.empty() && mx_que.back() < value) {
            mx_que.pop_back();
        }
        mx_que.push_back(value);
    }
    
    int pop_front() {
        if (que.empty()) return -1;
        int res = que.front(); que.pop();
        if (res == mx_que.front()) mx_que.pop_front();
        return res;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```