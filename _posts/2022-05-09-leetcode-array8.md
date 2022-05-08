---
layout: post
title:  "[leetcode] Array - 8. Check If N and Its Double Exist"
date:   2022-05-09
category: leetcode
---

## Problem 8. Check If N and Its Double Exist
Given an array `arr` of integers, check if there exists two integers `N` and `M` such that `N` is the double of `M` ( i.e. `N = 2 * M`).

<br>

More formally check if there exists two indices `i` and `j` such that :

* `i != j`
* `0 <= i, j < arr.length`
* `arr[i] == 2 * arr[j]`

### Example 1:
```yaml
Input: arr = [10,2,5,3]
Output: true
Explanation: N = 10 is the double of M = 5,that is, 10 = 2 * 5.
```

### Example 2:
```yaml
Input: arr = [7,1,14,11]
Output: true
Explanation: N = 14 is the double of M = 7,that is, 14 = 2 * 7.
```

### Example 3:
```yaml
Input: arr = [3,1,7,11]
Output: false
Explanation: In this case does not exist N and M, such that N = 2 * M.
```

### Constraints:
* `2 <= arr.length <= 500`
* `-10^3 <= arr[i] <= 10^3`

<br>

---
## My solution

임의의 배열 `arr`가 있을 때, 임의의 한 값이 다른 값의 두 배가 되는 것이 하나라도 있다면 `true`, 이를 만족하는 것이 하나도 없다면 `false`를 반환하는 문제이다.

```cpp
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        for (int i = 0; i < arr.size(); i++) {
            for (int j = 0; j < arr.size(); j++) {
                if (i == j) {
                    continue;
                }
                else if (arr[i] == arr[j] * 2) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

결과는 이렇다.

![alt text](/public/img/leetcode/leetcode-array-8.png)
