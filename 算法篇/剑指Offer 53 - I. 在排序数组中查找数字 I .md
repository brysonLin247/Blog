## 在排序数组中查找数字 |

> 剑指Offer 53 - |.在排序数组中查找数字 |
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/

统计一个数字在排序数组中出现的次数。

示例1：

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
```

示例2：

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: 0
```

提示：

- 0 <= nuts.length <= 10^5
- -10^9 <= nuns[i] <= 10^9
- nums是一个非递减数组
- -10^9 <= target <= 10^9

## 题解

###  法一 暴力法

遇到这道题，直观的思路就是用两个变量记录第一次和最后一次遇见 target 的下标，但这个方法的时间复杂度为 O(n)，没有利用数组升序的条件。

步骤：

- 从数组左侧和右侧均往数组中间遍历，遇到目标targer则停止，记录下标left和right
- 如果right > left，返回rigth-left+1；否则说明没有出现target，返回0；

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
    if (!nums.length) return 0;
    let len = nums.length;
    let left = 0,
        right = len - 1;
    while (nums[left] !== target && left < len) {
        left++;
    }
    while (nums[right] !== target && right >= 0) {
        right--;
    }
    return left <= right ? right - left + 1 : 0;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

### 法二 二分查找法

利用数组单调递增的特性，可以使用二分法来加速查找的过程。

排序数组nums中所有数字target形成一个窗口，记窗口的**左/右边界**，索引分别为left和right，分别对应窗口左边和右边的首个元素。因此题目可以使用二分法分别找到left和right，数量res = right - left - 1。步骤如下：

- 设左边界i = 0，右边界j = num.length - 1
- 循环条件是 i  <= j
  - 计算中点mid
  - 若nums[mid] < target，则target在闭区间[m + 1, j]，因此执行i = m + 1；同理，若nums[mid] > target，则执行j = m - 1;若两者相等，说明左边界在mid左侧，右边界在mid右侧（也可能在mid位置），则：
    - 查找右边界，则i = m + 1
    - 查找左边界，则j = m - 1;
- 使用两次二分，查出left和right，最终再返回right - left - 1。

```javascript
var search = function (nums, target) {
    // 搜索右边界
    let i = 0,
        j = nums.length - 1;
    while (i <= j) {
        let mid = Math.floor((i + j) / 2);
        if (nums[mid] <= target) {
            i = mid + 1;
        } else {
            j = mid - 1;;
        }
    }
    let right = i;
    // 若数组中无 target ，则提前返回
    if (j >= 0 && nums[j] !== target) return 0;
    // 搜索左边界
    i = 0;
    j = nums.length - 1;
    while (i <= j) {
        let mid = Math.floor((i + j) / 2);
        if (nums[mid] < target) {
            i = mid + 1;
        } else {
            j = mid - 1;;
        }
    }
    let left = j;
    return right - left - 1;
};
```

- 时间复杂度：O($logN$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

