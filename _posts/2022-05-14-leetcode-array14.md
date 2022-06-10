---
layout: post
title:  "[leetcode] Array - 1004. Max Consecutive Ones II (Medium)"
date:   2022-05-14
category: leetcode
---

## Problem 1004. Max Consecutive Ones II
Given a binary array `nums`, return *the maximum number of consecutive `1`'s in the array if you can flip at most one `0`*.

### Example 1:
```yaml
Input: nums = [1,0,1,1,0]
Output: 4
Explanation: Flip the first zero will get the maximum number of consecutive 1s. After flipping, the maximum number of consecutive 1s is 4.
```

### Example 2:
```yaml
Input: nums = [1,0,1,1,0,1]
Output: 4
```

### Constraints:
* `1 <= nums.length <= 10^5`
* `nums[i]` is either `0` or `1`.

---
## My solution

임의의 binary 배열 `nums`가 있을 때 최대 하나의 `0`은 flip (`1`로 뒤집을 수 있음) 할 수 있다. 이 때 1이 연속되는 최대 개수를 구해라.

* 일단 binary이고 1이 연속되는 것이기 때문에 합을 생각하면 된다.
* 앞이 0이고 뒤가 0인것 까지의 인덱스를 찾은뒤, 그 인덱스 간의 차이를 이용해서 구하였다.
* 시작점과 끝점을 표시하기 위해 `zeroIndex`의 첫 시작과 끝을 각각 `-1`, `nums.size()`를 넣었다.

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        std::vector<int> zeroIndex;
        zeroIndex.push_back(-1);
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0) {
                zeroIndex.push_back(i);
            }
        }
        zeroIndex.push_back(nums.size());

        if (zeroIndex.size() == 2) {
            return nums.size();
        }
        else {
            int max = -9999;
            for (int i = 0; i < zeroIndex.size(); i++) {
                if (i + 2 == zeroIndex.size()) {
                    break;
                }
                max = std::max(max, zeroIndex[i + 2] - zeroIndex[i] - 1);
            }
            return max;
        }
    }
};
```

결과는 그냥 무난한 수준. 이중 포문을 쓰고 싶었는데 잘 참았다..

빠른 결과들을 보니, 결국 좋은 알고리즘을 생각해서 구현해야 빨라지는 것 같다. 즉, 내가 생각한 앞뒤의 0을 보고 찾는 것보다 더 좋은..

결과를 내는 것은 하겠으나 빠르고 좋은 것을 내는건 쉽지 않구나.


![alt text](/public/img/leetcode/leetcode-array-14.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F14%2Fleetcode-array14.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)