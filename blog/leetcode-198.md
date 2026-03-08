# leetcode 198. 打家劫舍 刷题笔记
**文章标签：** leetcode、笔记、算法

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

**示例 1：**
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。

**示例 2：**
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。偷窃到的最高金额 = 2 + 9 + 1 = 12 。

**提示：**
- 1 <= nums.length <= 100
- 0 <= nums[i] <= 400

---

### 题目分析
从前看，第i个房子偷了之后那么就不能选第i-1个房子，因为是相连的，这个时候只能去偷第i-2个房子，当然也可以选择不偷第i个，这个时候也就是取了前i-1个的最大值，即：
f[i] = max(f[i-2]+nums[i], f[i-1]);

DP动态规划还有一个需要注意的是初始化，在这个题里面观察这个公式，发现应该有 i >= 2，也就是需要给 f[0] 和 f[1] 初始化。

### C++ 代码实现

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if(n==1)return nums[0];
        vector<int>f(n+1);
        f[0]=nums[0];
        f[1]=max(nums[0],nums[1]);
        int ans=f[1];
        for(int i=2;i<n;i++){
            f[i]=max(f[i-2]+nums[i],f[i-1]);
            ans=max(ans,f[i]);
        }
        return ans;
    }
};

```

**空间优化版本：**

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        int cur=0,pre1=0,pre2=0;
        for(int i=0;i<n;i++){
            cur=max(pre1,pre2+nums[i]);
            pre2=pre1;
            pre1=cur;
        }
        return cur;
    }
};

```


