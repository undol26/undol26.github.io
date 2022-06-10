---
layout: post
title:  "[leetcode] Array - 1089. Duplicate Zeros (Easy)"
date:   2022-05-03
category: leetcode
---

## Problem 1089. Duplicate Zeros (Easy)
Given a fixed-length integer array `arr`, duplicate each occurrence of zero, shifting the remaining elements to the right.

<br>
**Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

### Example 1:
```yaml
Input: arr = [1,0,2,3,0,4,5,0]
Output: [1,0,0,2,3,0,0,4]
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
```


### Example 2:
```yaml
Input: arr = [1,2,3]
Output: [1,2,3]
Explanation: After calling your function, the input array is modified to: [1,2,3]
```

### Constraints:
* `1 <= arr.length <= 10^4`
* `0 <= arr[i] <= 9`

<br>

---
## My solution

입력된 배열의 원소에서 0이 있다면, 오른쪽에 `0`을 하나 넣고, 나머지 값은 하나씩 `shift`한다. 이 때 배열의 최대 길이는 유지한다. 

1. 앞에서부터 `arr[i]=0`인 값을 찾고
2. `0`이라면
    1. 뒤에서부터 `i+2`까지 하나씩 shift
    2. 그리고 `i+1`의 값에 `0`을 넣고, i를 하나 더 키워줌 (어차피 여기서 `i+1`을 처리했으므로)
    3. 적절히 경계에 걸리지 않게 조건문 삽입
3. `0`이 아니라면 `continue`로 넘어가기


```cpp
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] == 0) {
                for (int j = arr.size() - 1; j > i + 1; j--) {
                    arr[j] = arr[j - 1];
                }
                if (i != arr.size() - 1) {
                    arr[i + 1] = 0;
                    i++;
                }
                continue;
            }
            else {
                continue;
            }
        }
    }
};
```

하지만 그 결과는 별로 좋지 않네 ㅠㅠ

![alt text](/public/img/leetcode/leetcode-array-4.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F03%2Fleetcode-array4.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)