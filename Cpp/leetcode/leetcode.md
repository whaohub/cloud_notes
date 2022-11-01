# leetcode

## 俩数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

```cpp
vector<int> twoSum(vector<int>& nums, int target) 
 {
        std::unordered_map<int, int> hash_table;

        for(int i = 0; i < nums.size(); ++i)
        {
            if(hash_table.find(target - nums[i])!= hash_table.end())
            {
                return {i, hash_table[target - nums[i]]};
            }
            hash_table[nums[i]] = i;
        }
        return {};
}
```

  思路:使用hashtable 保存target - x的值,若hash中不存在则将值存入,继续遍历

## 

## 爬楼梯

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

**输入：**n = 2
**输出：**2
**解释：**有两种方法可以爬到楼顶。

1. 1 阶 + 1 阶
2. 2 阶



```cpp
int climbStairs(int n) 
    {
        int first = 0;
        int second = 0;
        int ret = 1;

        for(int i = 1; i<= n; ++i)
        {
            first = second;
            second = ret;
            ret = first + second;
        }
        return ret;
    }
```

思路:使用动态规划第n阶台阶的前一次可能是爬了1或2,以此类推f(x)=f(x-1)+f(x-2);第n阶台阶是前一次爬了1阶或俩阶方法的和，没阶台阶皆是如此，所以设ret第一阶台阶为1,记录f(x-1)和f(x-2)的方法和。

时间复杂度：循环执行 n 次，每次花费常数的时间代价，故渐进时间复杂度为 O(n)O(n)。
空间复杂度：这里只用了常数个变量作为辅助空间，故渐进空间复杂度为 O(1)O(1)。


