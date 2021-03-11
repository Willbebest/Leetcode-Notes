##### 两数之和

题目描述

```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
你可以按任意顺序返回答案。
示例：
    输入：nums = [2,7,11,15], target = 9
    输出：[0,1]
    解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
提示：
    2 <= nums.length <= 103
    -109 <= nums[i] <= 109
    -109 <= target <= 109
    只会存在一个有效答案
```

我的题解：

```cpp
class Solution {
public:
  vector<int> twoSum(vector<int>& nums, int target) {
     map<int, int> valueToIndice;
     int length=nums.size();
     for(int i=0; i<length&&length>0; i++){
       if(valueToIndice.count(target - nums[i])){
         return vector<int>{valueToIndice[target-nums[i]], i};
       }
       valueToIndice[nums[i]] = i;
      }
     return vector<int>();
  }
};
```

>使用map建立索引关系，在遍历时，如果对应的值已经在map中则返回两个索引；如果不在，则将索引与值的关系添加到map中。

网上的解法：

``` cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> brother_set;
        for(int i = nums.size() - 1; i >= 0; --i){
            auto iter = brother_set.find(nums[i]);
            if(iter != brother_set.end())
                return {i, iter->second};
            int number_brother = target - nums[i];
            brother_set[number_brother] = i;
        }
        return {};
    }
};
```
>1. 循环从后往前，会得到更好的速度收益，从最后向前遍历，这算是利用数据特性吧
>2. 使用unordered_map，对差值进行存储，效率比map高
>3. 对于unordered_map的查找，不宜使用[key]的方式，这样会进行两次查找浪费时间，而应该使用iter，通过!=end()判断是否找到，通过iter->second直接拿到数据，一次查找操作
>4. 我们存储到map中的数据是diff（差值）。这样尽可能的减少计算diff的操作（比如iter != end满足条件时，diff是不需要计算的）
>5. 返回的是{}，而不是vector。减少调用 vector 的过程。

优化后的代码：

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> valToIdx;
        for(int i=nums.size()-1; i>=0; i--) {
            if(valToIdx.count(nums[i])){
                 return {valToIdx[nums[i]], i};
            }
            valToIdx[target-nums[i]]=i;
        }   
        return {};
    }
};
```
总结：
使用unordered_map的查找数据快于map
我们存储到map中的数据是diff（差值）。这样尽可能的减少计算diff的操作（比如iter != end满足条件时，diff是不需要计算的）
从后向前遍历，减少size的调用