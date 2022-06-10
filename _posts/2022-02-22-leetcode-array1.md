---
layout: post
title:  "[leetcode] Array - 485. Max consecutive ones (easy)"
date:   2022-02-22
category: leetcode
---
## Problem 485. Max consecutive ones (easy)
```yaml
Given a binary array nums, return the maximum number of consecutive 1's in the array.
```

### Example 1:
```yaml
Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
The maximum number of consecutive 1s is 3.
```

### Example 2:
```yaml
Input: nums = [1,0,1,1,0,1]
Output: 2
```

### Constraints:
* `1 <= nums.length <= 10^5`
* `nums[i]` is either `0` or `1`.
<br>

---
## My solution

1이 연속되는 개수를 구하라는 문제이다.

어차피 input은 0 아니면 1이기 때문에, 0이 아닐때까지 더해주고 (count), 

input이 0이되면 더 이상 연속되지 않으므로 지금까지 저장한 가장 큰 값(out)과 더해준 값(sum)과 비교하여 최대값 (out)을 업데이트 하는 생각으로 접근하였다.

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int out = 0;
        int sum = 0;
        
        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];
            if (nums[i] == 0) {
                if (out < sum) {
                    out = sum;
                }
                sum = 0;
            }
            else if (i == nums.size() - 1) {
                if (out < sum) {
                    out = sum;
                }
            }
        }
        
        return out;
    }
};
```

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F02%2F22%2Fleetcode-array1.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)