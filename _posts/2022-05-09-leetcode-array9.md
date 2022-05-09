---
layout: post
title:  "[leetcode] Array - 9. Valid Mountain Array"
date:   2022-05-09
category: leetcode
---

## Problem 9. Valid Mountain Array
Given an array of integers `arr`, return *`true`* *if and only if it is a valid mountain array.*

<br>

Recall that `arr` is a mountain array if and only if:

* `arr.length >= 3`
* There exists some `i` with `0 < i < arr.length - 1` such that:
    * `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    * `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`


### Example 1:
```yaml
Input: arr = [2,1]
Output: false
```

### Example 2:
```yaml
Input: arr = [3,5,5]
Output: false
```

### Example 3:
```yaml
Input: arr = [0,3,2,1]
Output: true
```

### Constraints:
* `1 <= arr.length <= 10^4`
* `0 <= arr[i] <= 10^4`

<br>

---
## My solution 1

임의의 배열 `arr`가 있을 때, 그 값이 커졌다가, 작아지면 `true`, 그 외는 `false`를 나타내는 문제이다.

1. 올라가는 값 또는 작아지는 값이 하나도 없다면 `false`
2. 같은 값이 하나라도 있다면 `false`
3. 작아지다가 커진다면 `false`
4. 그 외는 `true`??

정도로 생각하고 접근해보자.

```cpp
class Solution {
public:
    bool validMountainArray(std::vector<int>& arr)
    {
        int ascend = 0, descend = 0;
        for (int i = 0; i < arr.size(); i++) {
            if (i == arr.size() - 1) {
                break;
            }
            if (arr[i] == arr[i + 1]) {
                return false;
            }
            else if (arr[i] < arr[i + 1]) {
                ascend++;
            }
            else if (arr[i] > arr[i + 1]) {
                for (int j = i + 1; j < arr.size(); j++) {
                    if (j == arr.size() - 1) {
                        break;
                    }
                    if (arr[j] < arr[j + 1]) {
                        return false;
                    }
                }
                descend++;
            }
        }

        if (ascend == 0 || descend == 0) {
            return false;
        }
        else if (ascend + descend == arr.size() - 1) {
            return true;
        }
        else {
            return false;
        }
    }
};
```

결과는 `Runtime` 은 매우 느린데, `memory usage`는 매우 좋았다.

![alt text](/public/img/leetcode/leetcode-array-9-1.png)

나중에 좋은 생각이 나면 알고리즘을 수정해 봐야겠다. 지금 당장은 떠오르지가 않네.

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F09%2Fleetcode-array9.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)