---
layout: post
title:  "[leetcode] Array - 905. Sort Array By Parity (Easy)"
date:   2022-05-13
category: leetcode
---

## Problem 905. Sort Array By Parity (Easy)
Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.

<br>

Return ***any array*** *that satisfies this condition.*

### Example 1:
```yaml
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```

### Example 2:
```yaml
Input: nums = [0]
Output: [0]
```

### Constraints:
* `1 <= nums.length <= 5000`
* `0 <= nums[i] <= 5000`

---
## My solution

임의의 배열 `nums`가 있을 때 그 값이 짝수라면, 맨 앞으로 옮기는 문제이다.

새 배열을 하나 만들어서, `nums`가 짝수일 때 먼저 `push_back()`을 통해 값을 넣어주고, 다시 포문을 만들어서 `nums`가 홀수일 때 그 값들을 `push_back()`으로 넣었다. `O(2N)`

```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        std::vector<int> output;
        for (std::vector<int>::iterator it = nums.begin(); it < nums.end(); it++) {
            if (*it % 2 == 0) {
                output.push_back(*it);
            }
        }
        for (std::vector<int>::iterator it = nums.begin(); it < nums.end(); it++) {
            if (*it % 2 != 0) {
                output.push_back(*it);
            }
        }
        return output;        
    }
};
```

결과는 오랜만에 적절하게 나왔다. 

![alt text](/public/img/leetcode/leetcode-array-12.png)

가장 빠른 알고리즘을 봤더니 앞뒤를 동시에 건드려서 이중 포문이 아닌 포문 하나로 끝내버리네. 

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F13%2Fleetcode-array12.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)