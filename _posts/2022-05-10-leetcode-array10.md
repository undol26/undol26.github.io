---
layout: post
title:  "[leetcode] Array - 10. Replace Elements with Greatest Element on Right Side"
date:   2022-05-09
category: leetcode
---

## Problem 10. Replace Elements with Greatest Element on Right Side
Given an array `arr`, replace every element in that array with the greatest element among the elements to its right, and replace the last element with `-1`.


<br>

After doing so, return the array.

### Example 1:
```yaml
Input: arr = [17,18,5,4,6,1]
Output: [18,6,6,6,1,-1]
Explanation: 
- index 0 --> the greatest element to the right of index 0 is index 1 (18).
- index 1 --> the greatest element to the right of index 1 is index 4 (6).
- index 2 --> the greatest element to the right of index 2 is index 4 (6).
- index 3 --> the greatest element to the right of index 3 is index 4 (6).
- index 4 --> the greatest element to the right of index 4 is index 5 (1).
- index 5 --> there are no elements to the right of index 5, so we put -1.
```

### Example 2:
```yaml
Input: arr = [400]
Output: [-1]
Explanation: There are no elements to the right of index 0.
```

### Constraints:
* `1 <= arr.length <= 10^4`
* `1 <= arr[i] <= 10^5`

<br>

---
## My solution

임의의 배열 `arr`가 있을 때, 매 `element` 의 오른쪽에서 가장 큰 값으로 그 `element` 의 값을 대체하고, 마지막엔 `-1`을 입력하는 문제이다.

아 지금까지는 단순히 귀찮아서 `edge case`를 모두 생략하고 있는데, `constraints`의 조건으로 사실 체크해줘야 하는게 맞다. 

하지만 계속 생략하겠다.. 테스트 케이스를 내가 제출할 것도 아니고 일로서 하는 게 아니므로 ㅎㅎ

```cpp
class Solution {
public:
    vector<int> replaceElements(vector<int>& arr) {
        std::vector<int> output = arr;
        int max = -9999;
        for (int i = 0; i < arr.size(); i++) {
            max = -9999;
            if (i == arr.size() - 1) {
                output[i] = -1;
                break;
            }

            for (int j = i + 1; j < arr.size(); j++) {
                if (max < arr[j]) {
                    max = arr[j];
                }
            }
            output[i] = max;
        }
        return output;        
    }
};
```

결과는 이번에도 `Runtime`이 매우 느렸다. 아무래도 이중포문이 문제인 것 같다.

![alt text](/public/img/leetcode/leetcode-array-10.png)

가장 빠른 알고리즘을 봤더니 `max`도 함수가 있고, 값을 넣는 것도 `exchange`라는 함수가 있네..

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F09%2Fleetcode-array10.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)