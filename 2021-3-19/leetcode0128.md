####  [最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

***

**题目描述**

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

进阶：你可以设计并实现时间复杂度为 `O(n)` 的解决方案吗？

示例 1：

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

示例 2：

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

示例 3：

```
输入：nums = [1,3,0,2,1]
输出：4
```

提示：

- `0 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

***

**题意分析**

1. `nums`是未排序的
2. 要查找的连续序列在`nums`的位置并不连续, 在排序后连续, 但在排序后连续
3. `nums`可能为空

***

**我的解法**

最直接的解法

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if(nums.empty()) return 0;
        sort(nums.begin(), nums.end());
        int curCounter=1;
        int maxcounter=1;
        int len = nums.size();
        for(int i=1; i< len; i++) {
            if(nums[i] == nums[i-1]+1) {
                curCounter++;
            } else if(nums[i] != nums[i-1]){
                curCounter = 1;
            }
            maxcounter = max(curCounter, maxcounter);
        }
        return maxcounter;
    }
};
```

先对`nums`进行排序, 然后遍历`nums`. 在遍历时，需要实时的记录当前的最大连续序列`curCounter`，同时需要记录最大连续序列`maxcounter`，上述解法的时间复杂度要大于 O(n)

解法二：

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        set<int> sortNums;
        for (auto& elem : nums) {
            sortNums.insert(elem);
        }
        auto iter = sortNums.begin();
        auto endIter = sortNums.end();
        int preVal = *iter++;
        int curCounter = 1;
        int maxCounter = 1;
        while (iter != endIter) {
            if (*iter == preVal + 1) {
                curCounter++;
            }
            else {
                curCounter = 1;
            }
            preVal = *iter++;
            maxCounter = max(curCounter, maxCounter);
        }
        return maxCounter;
    }
};
```

与解法一类似，主要利用`set`的底层红黑树排序功能。

进阶解法：
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        unordered_set<int> hashSet;
        for (auto& elem : nums) {
            hashSet.insert(elem);
        }
        auto iter = hashSet.begin();
        auto endIter = hashSet.end();
        int curCounter = 0;
        int maxCounter = 0;
        while (iter != endIter) {
            curCounter = 0;
            for (int i = 0; hashSet.count(*iter + i)!=0; i++) {
                curCounter++;
            }
            maxCounter = max(maxCounter, curCounter);
            iter++;
        }
        return maxCounter;
    }
};
```

使用`unordered_set`的对象`hashSet`进行数据存储，因为unordered_map插入时的时间复杂度为O(1)。然后对`hashSet`进行遍历，每读取一个元素，然后检查元素的临近的逐步递增的元素是否在`hashSet`中，并记录存在的以当前元素为起始的连续序列的值。此种解法的时间复杂度为O(n), 但是在`leetcode`的运行结果是执行用时: **1444 ms**，内存消耗: **10.4 MB**。而前面两种解法的时间复杂度都是**8 ms**,这就有问题了，需要优化。

**优化后的代码**

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> hashSet;
        for (const int& elem : nums) {
            hashSet.insert(elem);
        }
        int maxCounter = 0;
        int curCounter = 0;
        for(auto elem : hashSet) {
            if(hashSet.count(elem-1)){ //主要优化的地方
                continue;
            }
            curCounter =0;
            while(hashSet.count(elem)) {
                curCounter++;
                elem++;
            }
            maxCounter=max(maxCounter, curCounter);
        }
        return maxCounter;
    }
};
```

主要优化的地方在于已经遍历过的元素，无需查找以它为为起始的连续序列。优化思路来自官方题解。优化了半天没想出来要优化这里。