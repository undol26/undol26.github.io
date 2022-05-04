---
layout: post
title:  "[leetcode] Array - 5. Merge Sorted Array"
date:   2022-05-04
category: leetcode
---

## Problem 5. Merge Sorted Array
You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1 `and `nums2` respectively.

<br>
**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order.**

<br>
The final sorted array should not be returned by the function, but instead be *stored inside the array* `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first m elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

<br>
**Note** that elements beyond the length of the original array are not written. Do the above modifications to the input array in place and do not return anything.

### Example 1:
```yaml
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

### Example 2:
```yaml
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

### Example 3:
```yaml
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

### Constraints:
* `nums1.length == m + n`
* `nums2.length == n`
* `0 <= m, n <= 200`
* `1 <= m + n <= 200`
* `-10^9 <= nums1[i], nums2[j] <= 10^9`

<br>

---
## My solution 1

배열1, 배열2의 값 들 중 `0`이 아닌 것들을 오름차순으로 합치는 문제이다. 

constraints의 조건을 잘 이용하면 효과적인 알고리즘이 될 것 같다.

첫 번째 `nums1.length == m + n` 조건은 필요한건가? 아직 잘 모르겠다.

1. 각 배열에서 `0`인 값들을 제거한다. (`new_nums1.size()=m, new_nums2.size()=n`)
2. 제거한 각 배열을 하나로 합친다. (`output.size()=m+n`)
3. 오름차순으로 `sorting`한다.

매우 간단하게 정리된다. 전에 저 `sorting`을 직접 이중 `for`문으로 구현했다가 속도가 느리다고 답 채택이 안됐으니.. 이번엔 `sort`함수를 써보려고 한다. 

```cpp
class Solution
{
public:
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n)
    {
        std::vector<int> new_nums1, new_nums2, output;
        for (auto& num : nums1) {
            if (num != 0) {
                new_nums1.push_back(num);
            }
        }

        for (auto& num : nums2) {
            if (num != 0) {
                new_nums2.push_back(num);
            }
        }

        int index = 0;
        for (int i = 0; i < m + n; i++) {
            if (i < m) {
                output.push_back(new_nums1[index]);
                index++;
                if (i == m - 1) {
                    index = 0;
                }
            }
            else {
                output.push_back(new_nums2[index]);
                index++;
            }
        }

        sort(output.begin(), output.end());
        nums1 = output;
    }
};
```

아니 근데 결과가 실패.. 어떤 케이스에서 실패했는지 확인해봤더니
```yaml
Input: nums1 = [-1,0,0,3,3,3,0,0,0], m = 6, nums2 = [1,2,2], n = 3
```
아니 `nums1`은 **non-decreasing order** 라고 했으면, `nums1`의 3뒤에 0이 오는게 올바른 테스트케이스인가?? 킹받네...

다시 알고리즘을 수정해야겠다.

## My solution 2
위의 test case를 봤을 때, `nums1`의 element인 `m`이 6이 나오려면 `{-1, 0, 0, 3, 3, 3}` 까지를 count했다는 말이고, 3뒤에 non-decreasing order를 만족하지 않는 녀석 (여기선 0을 제외한 것 같다.

1. 각 배열에서 non-decreasing order를 깨는 element를 제거한다. (`new_nums1.size()=m, new_nums2.size()=n`)
2. 제거한 각 배열을 하나로 합친다. (`output.size()=m+n`)
3. 오름차순으로 `sorting`한다.

이렇게 해서 여차저차 풀었더니 또 안되는 케이스 발견...
```yaml
Input: nums1 = [-1,-1,0,0,0,0], m = 4, nums2 = [-1,0], n = 2
```

아니 -1뒤에 0이 오는것 까지는 이해하겠는데, 그 0이 어디까지가 m인지 내가 어떻게 알아??

**이쯤되니 문제를 잘못이해한 것 같다.**

## My solution 3
문제를 다시 정의하자.
1. `nums1`은 향후 merging된 array를 return해야 하므로 뒤에 `dummy` 값이 붙는다. 
    1. 그 `dummy` 값을 `0`으로 입력한다. (ex, `nums1 = [-1,0,0,3,3,3,0,0,0]`)
    2. `dummy` 값을 제외한 `element` 를 `m`으로 표시한다. (ex `m=6`)
    3. 즉, `dummy` 값 0이 3개이므로, `nums2`는 `n`이 3이어야 한다.
2. `nums2`는 non-decreasing order의 배열이다. 
    1. 역시 `element` 의 개수를 `n`으로 표시한다.

아. 문제를 잘못 이해해서 엄청 복잡하게 짜고 있었다... ㅠㅠ
```cpp
class Solution
{
public:
    void printVec(std::vector<int>& output)
    {
        std::cout << "{";
        for (int i = 0; i < output.size(); i++) {
            if (i == output.size() - 1) {
                std::cout << output[i] << "}" << std::endl << std::endl;
            }
            else {
                std::cout << output[i] << ", ";
            }
        }
    }
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n)
    {
        std::vector<int> output;

        if (m == 0) {
            nums1 = nums2;
            std::cout << "output: ";
            printVec(nums1);
        }
        else if (n == 0) {
            // do nothing
            std::cout << "output: ";
            printVec(nums1);
        }
        else {
            int index = 0;
            for (int i = 0; i < m + n; i++) {
                if (i < m) {
                    output.push_back(nums1[index]);
                    index++;
                    if (i == m - 1) {
                        index = 0;
                    }
                }
                else {
                    output.push_back(nums2[index]);
                    index++;
                }
            }

            sort(output.begin(), output.end());
            nums1 = output;

            std::cout << "nums1: ";
            printVec(nums1);
        }
    }
};
```

이번 건 괜찮은 결과가 나왔군. 

![alt text](/public/img/leetcode/leetcode-array-5.png)

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fundol26.github.io%2Fleetcode%2F2022%2F05%2F04%2Fleetcode-array5.html&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)