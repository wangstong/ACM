### 剑指 Offer 51. 数组中的逆序对
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
**示例 1:**
```
输入: [7,5,6,4] 
输出: 5
```
**限制：**
`0 <= 数组长度 <= 50000`
```cpp
class Solution {
public:
    int ans;
    void merge_sort(vector<int>& a, int l, int r) {
        if (l >= r) return;
        int mid = l+r>>1;
        merge_sort(a,l,mid);
        merge_sort(a,mid+1,r);

        vector<int> ve; 
        int i = l, j = mid+1;
        while (i <= mid && j <= r) {
            if (a[i] <= a[j]) ve.push_back(a[i++]);
            else {
                ans += (mid-i+1);
                ve.push_back(a[j++]);
            }
        }
        while (i <= mid) ve.push_back(a[i++]);
        while (j <= r) ve.push_back(a[j++]);
        
        for (int i = 0; i < ve.size(); ++i) {
            a[l+i] = ve[i];
        }
    }
    int reversePairs(vector<int>& nums) {
        ans = 0;
        merge_sort(nums,0,nums.size()-1);
        return ans;
    }
};
```

