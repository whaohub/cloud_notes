[toc]

# leetcode

## 1. 俩数之和

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



## 2. 爬楼梯

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



## 3. 买卖股票的最佳时机

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

```

```cpp
 int maxProfit(vector<int>& prices) 
 {
     int minprice = 1e9;
     int maxprofit = 0;
     for(auto price:prices)
     {
         maxprofit = max(maxprofit,price - minprice);
         minprice = min(minprice, price);
     }
     return maxprofit;
 }
```

思路:遍历一次在历史价格中记录最小的价格，遍历过程中计算当前最大的利润和历史最大利润作比较，最后返回计算结果

- 时间复杂度：O(n)，只需要遍历一次。
- 空间复杂度：O(1)，只使用了常数个变量。

## 4. 环形链表

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

```cpp
bool hasCycle(ListNode *head) 
    {
        if(head == nullptr)
            return false;

        ListNode *slow = head;
        ListNode *fast = head->next;

        while(slow != fast)
        {
            if(fast==nullptr || fast->next == nullptr)
            {
                return false;
            }
            slow = slow->next;
            fast = fast->next->next;
        }
        return true;
    }
```

> 思路:快慢指针

时间复杂度：O(N)其中 N 是链表中的节点数。

当链表中不存在环时，快指针将先于慢指针到达链表尾部，链表中每个节点至多被访问两次。

当链表中存在环时，每一轮移动后，快慢指针的距离将减小一。而初始距离为环的长度，因此至多移动 N 轮。

空间复杂度：O(1)。我们只使用了两个指针的额外空间

## 5. 罗马数字转整数

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 `2` 写做 `II` ，即为两个并列的 1 。`12` 写做 `XII` ，即为 `X` + `II` 。 `27` 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。

**示例 1:**

```
输入: s = "III"
输出: 3
```

```cpp
class Solution 
{
private:
    unordered_map<char, int> symbolValues = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000},
    };

public:
    int romanToInt(string s) 
    {
        int n = s.length();
        int ans = 0;
        for(int i = 0; i < n; ++i)
        {
            int value = symbolValues[s[i]];
            if(i < n - 1 && value < symbolValues[s[i + 1]])
            {
                ans -= value;
            }
            else
            {
                ans += value;
            }

        }
        return ans;
    }

};
```

思路:遍历罗马hashmap,比较相邻的俩个值，小于则相减否则相加

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是字符串 s 的长度。
- 空间复杂度：O(1)。



##  6. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```



```cpp
string longestCommonPrefix(vector<string>& strs) 
{
    std::string res = strs.empty() ? "":strs[0];

    /*
            在字符串s中查找res并返回首字母的位置（find函数）
            如果首地址不为零，每次令res-1以缩短公共前缀
            比如说再flow中查找flower，没有找到，返回值为迭代器结尾（非0）
            公共前缀会减掉最后一个字母，为flowe。继续循环直到为flow

            如果是首字母不一样则公共前缀会清空,只有其实位置是0算是前缀
            */ 
    for(const std::string& str: strs)
    {
        while(str.find(res) != 0)
        {
            res = res.substr(0, res.length() - 1);
        }
    }
    return res;
}
```



##  7. 删除有序数组中的重复项

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 `k` 个元素，那么 `nums` 的前 `k` 个元素应该保存最终结果。

将最终结果插入 `nums` 的前 `k` 个位置后返回 `k` 。

```cpp
 int removeDuplicates(vector<int>& nums) 
 {
     if(nums.size() == 0)
         return 0;

     int slow = 1;
     int fast = 1;

     while(fast < nums.size())
     {
             if(nums[fast] != nums[fast - 1])
         {
             nums[slow] = nums[fast];
             ++slow;
         }
         ++fast;
     }
     return slow;

 }
```

思路:使用快慢指针，从下标1开始，快指针负责比较相邻的元素是否相同，不相同时慢指针负责保存非重复值，





##  8. 无重复字符的最长子串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

answer:

```cpp
class Solution 
{
public:
    int lengthOfLongestSubstring(string s) 
    {
        if(s.empty())
            return 0;

        std::unordered_set<char>  sliding_window;
        int left = 0;
        int maxstr = 0;

        for(int i = 0; i < s.size(); ++i)
        {
            while(sliding_window.find(s[i]) != sliding_window.end())
            {
                sliding_window.erase(s[left]);
                left++;
            } //找到滑动窗口中重复字符，将滑动窗口至重复字符的前的字符移除
            maxstr = std::max(maxstr, i - left + 1);
            sliding_window.insert(s[i]);
        }
        return maxstr;
    }
};
```

思路:维持无重复的滑动窗口，将重复字符左侧移除维持队列长度

- 时间复杂度：O(n)

##  9. 验证回文串

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。 

**示例 1：**

```
输入: s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。
```

```cpp
class Solution 
{
public:
    bool isPalindrome(string s) 
    {
        std::string str;
        for(const auto& ch: s)
        {
            if(isalnum(ch))
            {
                str +=std::tolower(ch);
            }
        }
        std::string rev_str(str.rbegin(), str.rend());
        return str==rev_str;
    }
};
```

思路: 对字符进行小写转换，对转换后字符串reverse,比较反转后字符串与原字符串是否相等

复杂度分析

时间复杂度：O(∣s∣)，其中 ∣s∣ 是字符串 s 的长度。

空间复杂度：O(∣s∣)。由于我们需要将所有的字母和数字字符存放在另一个字符串中，在最坏情况下，新的字符串 sgood与原字符串 s 完全相同，因此需要使用 O(∣s∣) 的空间。

## 10 整数的反转

给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−231, 231 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

 

**示例 1：**

```
输入：x = 123
输出：321
```



```cpp
class Solution {
public:
    int reverse(int x) 
    {
        int res = 0;

        while(x != 0)
        {
            int tmp = x % 10;

            if(res > INT_MAX/10 || (res == INT_MAX/10 && tmp > (INT_MAX % 10)))
            {
                return 0;
            }
            if(res < INT_MIN/10 || (res == INT_MIN/10 && tmp < (INT_MIN %10)))
            {
                return 0;
            }
            res = res*10 + tmp;
            x/=10;
        }
        return res;
    }
};
```

思路:

1. 将12345 % 10 得到5，之后将12345 / 10
2. 将1234 % 10 得到4，再将1234 / 10
3. 将123 % 10 得到3，再将123 / 10
4. 将12 % 10 得到2，再将12 / 10
5. 将1 % 10 得到1，再将1 / 10

这么看起来，一个循环就搞定了，循环的判断条件是x>0
但这样不对，因为忽略了 负数
循环的判断条件应该是while(x!=0)，无论正数还是负数，按照上面不断的/10这样的操作，最后都会变成0，所以判断终止条件就是!=0
有了取模和除法操作，对于像12300这样的数字，也可以完美的解决掉了。

看起来这道题就这么解决了，但请注意，题目上还有这么一句

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。

也就是说我们不能用long存储最终结果，而且有些数字可能是合法范围内的数字，但是反转过来就超过范围了。
假设有1147483649这个数字，它是小于最大的32位整数2147483647的，但是将这个数字反转过来后就变成了9463847411，这就比最大的32位整数还要大了，这样的数字是没法存到int里面的，所以肯定要返回0(溢出了)。 甚至，我们还需要提前判断

如果某个数字**等于** `214748364`呢，这对应到上图中第三、第四、第五排的数字，需要要跟最大数的**末尾数字**比较，如果这个数字比`7`还大，说明溢出了。

如果某个数字**等于** `-214748364`，还需要跟最小数的末尾比较，即看它是否小于`8`

