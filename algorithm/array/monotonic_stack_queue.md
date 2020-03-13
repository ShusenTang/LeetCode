# 单调数据结构

## 1. 单调栈

### 1.1 原理

顾名思义，单调栈即元素单调递增或递减排列的栈。我们可以**通过单调栈求元素的左（右）边第一个比他大（小）的元素**。

例如我们想求数组里每个元素的左边第一个比它小的元素，我们可以维护一个严格单调增的栈，我们从前往后遍历原数组，设当前元素为num:

* 我们不断去掉栈顶元素直到栈空或者栈顶元素小于num，此时栈顶元素就是num的左边第一个比它小的元素，记录结果，然后将num入栈。

整个过程的复杂度为 O(n) ，因为所有元素只会进栈出栈一次。

### 1.2 代码

``` C++
vector<int> PreviousSmallerElement(vector<int>& nums) {
    /*
        求数组里每个元素的左边第一个比它小的元素，若找不到用-1填充
    */
    vector<int> res(nums.size());
    stack<int> monoStack;
    for (int i = 0; i < nums.size(); i++) {
        while (!monoStack.empty() && monoStack.top() >= nums[i])
            monoStack.pop();

        res[i] = monoStack.empty() ? -1 : monoStack.top();
        monoStack.push(nums[i]);
    }
    return res;
}
```

### 1.3 相关题目

LeetCode上面可以用单调栈解决的题还挺多的：
* [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
* [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
* [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
* [456. 132 Pattern](https://leetcode.com/problems/132-pattern/)
* [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)
* [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
* [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
* [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)


## 2. 单调队列

### 2.1 原理

与单调栈类似，单调队列即元素单调递增或递减排列的队列。我们可以**通过单调队列求滑动窗口的最值问题**。一般用双向队列`deque`实现。


例如我们想求大小为k的滑动窗口内部的最大值（LeetCode239），那么核心思路就是用一个`deque`存放有可能成为最大值的元素（的**下标**）。从左往右滑动窗口的过程中，若窗口即将把`nums[i]`包含进来，

1. 首先，若队首元素下标小于`i - k`，即队首在窗口之外了，所以应该删除队首元素；
2. 然后，由于我们仅保留有可能成为最大值的元素（的下标），所以我们应该从队尾开始不断去掉比`nums[i]`小的那些元素（的下标），因为只要窗口内有`nums[i]`，那么去掉的这些元素就不可能成为最大值。
3. 最后，我们将`nums[i]`（的下标）送入队尾。

因此，按照上述过程维护的队列里面的元素是单调递减的，队首的元素即每次窗口内的最大值。

每个元素入队出队一次，时间复杂度O(n)，且仅遍历一次数组。

> 滑动窗口最值问题还有一个同样O(n)的思路，具体见[239题解](../../solutions/239.%20Sliding%20Window%20Maximum.md)思路二。

### 2.2 代码
``` C++
// Leetcode 239
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int>res;
    deque<int>win;

    for(int i = 0; i < nums.size(); i++){
        if (!win.empty() && win.front() == i - k) win.pop_front();
        
        while(!win.empty() && nums[win.back()] <= nums[i]) win.pop_back();
        
        win.push_back(i);
        
        if(i >= k - 1) res.push_back(nums[win.front()]);
    }
    return res;
}
```

### 2.3 相关题目

* [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
 


