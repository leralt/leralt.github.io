---
tags:
  - 中等
---
<iframe width=100%
  height="360" border=none scrolling = "auto" src="https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked"></iframe>
  
思路：维护一个动态窗口，窗口内的数之和大于K，则减少前面的数字，小于K，则增加后面的数字，等于K，则计数加1

存在num和K小于0的情况，上述思路不成立

正确思路：遍历计算前缀和pre，将其用哈希表存下来，key=前缀和， value=前缀和出现次数，遍历过程中，查找哈希表中是否存在(pre-k)，如有，则结果增加value值
```cpp
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        // mp[0] = 1;  
        int count = 0, pre = 0;
        for (auto& x:nums) {
	        mp[pre]++; // 防止前缀和即为k 将mp[0] = 1;
            pre += x;
            if (mp.find(pre - k) != mp.end()) {
                count += mp[pre - k];
            }
            // mp[pre]++;
        }
        return count;
    }
```