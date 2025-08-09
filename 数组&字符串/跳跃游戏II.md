# 跳跃游戏II

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向后跳转的最大长度。换句话说，如果你在索引 `i` 处，你可以跳转到任意 `(i + j)` 处：

- `0 <= j <= nums[i]` 且
- `i + j < n`

返回到达 `n - 1` 的最小跳跃次数。测试用例保证可以到达 `n - 1`。

 

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2
```

 

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `n - 1`



```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        if(n <= 1)
            return 0;
        int cur_reach = 0;
        int next_reach = 0;
        int jump = 0;
        for(int i=0;i<n;i++){
            // 计算出在当前的跳跃范围内能跳出下一跳的最大范围
            next_reach = max(next_reach, nums[i]+i);
            // 到达当前跳跃边界
            if(i == cur_reach){
                jump++;
                cur_reach = next_reach;
                if(cur_reach >= n-1)
                    break;
            }
        }
        return jump;
    }
};
```

