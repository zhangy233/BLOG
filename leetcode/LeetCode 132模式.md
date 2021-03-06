## LeetCode 456题 132模式

### 题目
给定一个整数序列：a1, a2, ..., an，一个132模式的子序列 ai, aj, ak 被定义为：当 i < j < k 时，ai < ak < aj。设计一个算法，当给定有 n 个数字的序列时，验证这个序列中是否含有132模式的子序列。

注意：n 的值小于15000。

示例1:

输入: [1, 2, 3, 4]

输出: False

解释: 序列中不存在132模式的子序列。
示例 2:

输入: [3, 1, 4, 2]

输出: True

解释: 序列中有 1 个132模式的子序列： [1, 4, 2].
示例 3:

输入: [-1, 3, 2, 0]

输出: True

解释: 序列中有 3 个132模式的的子序列: [-1, 3, 2], [-1, 3, 0] 和 [-1, 2, 0].

### 解法思路
&emsp;&emsp;我们的解法思路就是从后往前记录最大值 和 次大值, 然后如果前面有小于次大值的数就说明符合该模式

* 1 从后往前, 将每个值存入一个栈中, 如果出现一个较大值(图中的上升), 将小于该值的数弹出并记录最小值

![1571838471782](E:\BLOG\算法题解\1571838471782.png)

* 2 当继续往前是, 如果没有遇到小于次大值的数, 但是又碰到较大值呢?(上升区)

![1571838692758](E:\BLOG\算法题解\1571838692758.png)

  我们每次遇到上升时都需要重新刷新次大值, 因为如图中的最小值必然大于上一次的次大值(否则我们就已经匹配成功了); 最大值的大小我们无需节意, 但是次大值必须要刷新为最大的一个.
否则会出现如图, 大于第一次次大值但是小于第二次次大值的情况

![1571838907869](E:\BLOG\算法题解\1571838907869.png)

```
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        if(nums.size() < 3) return false;

        int len = nums.size();
        int third = INT_MIN;
        stack<int> max;
        for(int i = len - 1; i >= 0; i--)
        {
        	if(nums[i] < third) return true;

        	while(!max.empty() && nums[i] > max.top())
        	{
                third = max.top();
        		max.pop();
        	}
        	max.push(nums[i]);
        }

        return false;
    }
};
```