---
layout: post
title:  "[leetcode] Array - 1295. Find Numbers with Even Number of Digits (Easy)"
date:   2022-02-22
category: leetcode
---

## Problem 1295. Find Numbers with Even Number of Digits (Easy)
```yaml
Given an array nums of integers, return how many of them contain an even number of digits.
```

### Example 1:
```yaml
Input: nums = [12,345,2,6,7896]
Output: 2
Explanation:
12 contains 2 digits (even number of digits).
345 contains 3 digits (odd number of digits).
2 contains 1 digit (odd number of digits).
6 contains 1 digit (odd number of digits).
7896 contains 4 digits (even number of digits).
Therefore only 12 and 7896 contain an even number of digits.
```


### Example 2:
```yaml
Input: nums = [555,901,482,1771]
Output: 1 
Explanation: 
```

### Constraints:
* `1 <= nums.length <= 500`
* `1 <= nums[i] <= 10^5`
<br>

---
## My solution

입력된 배열에서 짝수 자리수의 개수를 구하는 문제이다.

자리수를 나타내는 것은 결국 10진법이니까, 10의 지수승으로 생각해서, 이 지수가 홀수일 때가 원하는 자리수가 짝수인 경우이다. (ex. $1787 / 10^3 > 1$)

그래서 지수가 홀수일 때 개수를 더했다.

```cpp
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        int out = 0;
        int digit = 0;
        
        for (int i = 0; i < nums.size(); i++) {
            for (int j = 5; j > 0; j--) {
                digit = nums[i] / pow(10,j);
                
                if (digit > 0) {
                    if ((j % 2) == 1) {
                        out++;
                        break;
                    }
                    break;
                }
            }
        }
        
        return out;
    }
};
```

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F02%2F22%2Fleetcode-array2.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)