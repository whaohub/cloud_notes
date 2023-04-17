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
空间复杂度：这里只用了常数个变量作为辅助空间，故渐进空间复杂度为 O(1)。



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



## 11 删除链表的倒数第 N 个结点

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```



```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution 
{
public:
    unsigned int get_length(const ListNode*  head)
    {
        unsigned int length = 0;
        const ListNode* travel = head;
        while(travel)
        {
            length++;
            travel = travel->next;
        }
        return length;
    }
    ListNode* removeNthFromEnd(ListNode* head, int n) 
    {
        if(!head)
        {
            return nullptr;
        }
        unsigned int length = get_length(head);
        ListNode* dummy = new ListNode(0, head);
        ListNode* cur = dummy;
        for(int i = 0;i < length - n ; ++i)
        {
            cur = cur->next;
        }
        cur -> next= cur->next->next;
        ListNode* ans =  dummy->next;
        delete dummy;
        return ans;
    }
};
```

- 思路:获取链表长度，添加一个哑节点（便于删除头节点），遍历链表查找要删除的前一个节点
- 

## 12  找出字符串中第一个匹配项的下标

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

 

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

```cpp

class Solution 
{
public:
    int strStr(string haystack, string needle)
    {
        size_t hay_size = haystack.size();
        size_t needle_size = needle.size();
        
        for(size_t i = 0 ; i + needle_size <= hay_size;++i)
        {
            bool flag = true;
            for(size_t j = 0;j < needle_size; ++j)
            {
                if(haystack[i + j] != needle[j])
                {
                    flag = false;
                }
            }
            if(flag)
            {
                return i;
            }
        }
        return -1;
    }
};
```

- 思路：暴力遍历从长字符串中查找字串，第一次找到后返回
- 算法复杂度：O(m*n) 最坏字符串遍历结束
- 空间复杂度：O(1)

## 13  快乐数

编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
- 如果这个过程 **结果为** 1，那么这个数就是快乐数。

如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

**示例 1：**

```
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**示例 2：**

```
输入：n = 2
输出：false
```



```cpp
class Solution 
{
public:
    int bitSquareSum(int n)
    {
        int sum = 0;
        while(n)
        {
            int num = n % 10;
            sum += num * num;
            n /= 10;
        }
        return sum;
    }

    bool isHappy(int n) 
    {
        int slow = n;
        int fast = n;
        do
        {
            slow = bitSquareSum(slow);
            fast = bitSquareSum(fast);
            fast = bitSquareSum(fast);
        }while(slow != fast);

        return slow==1;
    }
};
```

- 思路：如何破循环，如果不是快乐数，就会无限循环，也就是会陷入链表环，等同于查找链表是否有环，如果是快乐数，那么快慢指针结果都将是1,则退出循环



## 14 回文数

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

- 例如，`121` 是回文，而 `123` 不是。

**示例 1：**

```
输入：x = 121
输出：true
```

```cpp
class Solution 
{
public:
    bool isPalindrome(int x) 
    {
        if(x < 0 || (x % 10 == 0 && x != 0))
        {
            return false;
        }

        int revertnum = 0;
        
        while(x > revertnum)
        {
            revertnum = revertnum * 10 + x % 10;
            x /= 10;
        }
        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除
        return x == revertnum || x == revertnum/10;
    }
};
```

- 思路：前置条件: 负数末尾为0的数不可能是回文数; 对数进行半反转比较(easy)



## 15环形链表 II

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution 
{
public:
    ListNode *detectCycle(ListNode *head) 
    {
        if(!head)
            return nullptr;

        std::unordered_set<ListNode *> visited;

        while(head)
        {
            if(visited.count(head))
            {
                return head;
            }
            visited.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```

思路:使用hash表遍历一次链表，如果重复就是有环，且返回重复节点即满足要求

- 时间复杂度:O(n)

- 空间复杂度:O(n)
