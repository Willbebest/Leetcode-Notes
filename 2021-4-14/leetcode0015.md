####  [三数之和](https://leetcode-cn.com/problems/3sum/)

***

**题目描述**

给你一个包含 n 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

> 注意：答案中不可以包含重复的三元组。

示例1：

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

示例 2：

```
输入：nums = []
输出：[]
```

**提示：**

- `0 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

****

**超时暴力解法**

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size()<=2) return {};
        vector<vector<int>> result;
        int len = nums.size();
        
        for(int i=0; i<len; i++) {
            for(int j=i+1; j<len; j++) {
                for(int k = j+1; k<len; k++) {
                    if(nums[i]+nums[j]+nums[k]==0) {
                        vector<int> vct{nums[i], nums[j], nums[k]};
                        sort(vct.begin(), vct.end());
                        if(find(result.begin(), result.end(), vct) == result.end()) {
                            result.push_back(vct);
                        }
                    }
                }
            }
        }

        return result;
    }
};
```

使用三次循环，进行遍历。时间复杂度为O($n^3$)。结果超时了

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result; 
        
        int len = nums.size();
        if(len < 3) return ressult;         
        std::sort(nums.begin(), nums.end());  // 排序

        for(int i=0; i< len; i++) {            // 固定第一个数
            if(nums[i]>0) return result; 
            if(i>0&&nums[i]==nums[i-1]) continue;
			// 双指针在i后面的区间中寻找和为0-nums[i]的另外两个数
            int left = i+1;
            int right = len -1;
            while(left < right) {
                if(nums[left] + nums[right] > -nums[i]) {
                    right--; // 两数之和太大，右指针左移
                } else if(nums[left] + nums[right] < -nums[i]) {
                    left++;  // 两数之和太小，左指针右移
                } else {
                    result.push_back(vector<int>{nums[i], nums[left++], nums[right--]});
                    // 去重：第二个数和第三个数也不重复选取
                    while(left < right && nums[left]==nums[left-1]) left++;
                    while(left < right && nums[right]==nums[right+1]) right--;
                }
            }
        }

        return result;
    }
};
```

时间复杂度O($n^2$)，空间复杂度`O(log N)`.

先进行排序。然后进行遍历，然后利用双指针查找剩余两个元素。去重的步骤比较关键

> ① 先检查元素个数是否大于三，满足要求，不满足返回
>
> ② 将`nums`中的元素排序
>
> ③ 进行遍历，以索引为`i`的元素为第一个元素，在`i+1`~`len`中查找剩下的两个元素。因为要和为0，所以第一个元素一定小于，因为 已经是有序数组
>
> ④ 使用双指针，以`i+1`第二个元素的起始点，以`len-1`为第三个元素的起始点，进项查找两个元素和为 `-nums`的元素：
>
> - 当两个元素的和为`-nums[i]`时，添加到结果序列，并过滤重复元素
> - 当两个元素的和小于`-nums[i]`时，第二个元素向后移
> - 当两个元素的和大于`-nums[i]`时，第三个元素向前移