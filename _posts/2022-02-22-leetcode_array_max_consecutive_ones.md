---
layout: post
title:  "[leetcode] Array - max consecutive ones"
date:   2022-02-22
category: leetcode
---
# Arrays

## Problem 1. Max consecutive ones.
> Given a binary array nums, return the maximum number of consecutive 1's in the array.

#### Example 1:
> Input: nums = [1,1,0,1,1,1] <br>
> Output: 3 <br>
> Explanation: The first two digits or the last three digits are consecutive 1s. <br>
> The maximum number of consecutive 1s is 3.

#### Example 2:
> Input: nums = [1,0,1,1,0,1] <br>
> Output: 2

#### Constraints:
> 1 <= nums.length <= 105 <br>
> nums[i] is either 0 or 1.

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
