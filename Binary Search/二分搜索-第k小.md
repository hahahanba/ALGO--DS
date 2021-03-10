# Leetcode 题解 - 二分搜索-第K小
<!-- GFM-TOC -->
* [Leetcode 题解 - 二分搜索-第K小](#leetcode-题解---二分搜索-第k小)
  * [二分搜索-第K小](#二分搜索-第K小)
    * [1. 有序矩阵中第K小的元素](#1-有序矩阵中第K小的元素)
    * [2. 乘法表中第k小的数](#2-乘法表中第k小的数)
    * [3. 找出第k小的距离对](#3-找出第k小的距离对)
    * [4. 第k个最小的素数分数](#4-第k个最小的素数分数)
<!-- GFM-TOC -->

## 二分搜索-第K小

### 1. 有序矩阵中第K小的元素

378\. Kth Smallest Element in a Sorted Matrix (Medium)

[参考题解](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/solution/you-xu-ju-zhen-zhong-di-kxiao-de-yuan-su-by-leetco/)

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

```python
def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
    n = len(matrix)
    def check(mid):
        i, j = n - 1, 0
        count = 0
        while i >= 0 and j < n:
            if matrix[i][j] <= mid:
                count += i + 1
                j += 1
            else:
                i -= 1
        return count
    # 标记搜索的上限及下限，此题的上限和下限和数值的上下限不一致
    left, right = matrix[0][0], matrix[-1][-1]
    while left < right:
        mid = left + (right - left) // 2
        count_k = check(mid)
        if count_k >= k:
            right = mid
        else:
            left = mid + 1
    return left
```

### 2. 乘法表中第k小的数

668\. Kth Smallest Number in Multiplication Table (Hard)

[Leetcode](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-number-in-multiplication-table/)

```python
def findKthNumber(self, m: int, n: int, k: int) -> int:
    def check(x):
        count = 0
        for i in range(1, m+1):
            count += min(x // i, n)
        return count
    # 对比上一题，此题的上限和下限和数值的上下限一致
    left, right = 1, m * n
    while left < right:
        mid = left + (right - left) // 2
        count_k = check(mid)
        if count_k >= k:
            right = mid
        else:
            left = mid + 1
    return left
```

### 3. 找出第k小的距离对

719\. Find K-th Smallest Pair Distance (Hard)

* 我们可以使用双指针来计算出所有小于等于 mid 的距离对数目。我们维护 left 和 right，其中 right 通过循环逐渐递增，left 在每次循环中被维护，使得它满足 nums[right] - nums[left] <= mid 且最小。这样对于 nums[right]，以它为右端的满足距离小于等于 guess 的距离对数目即为 right - left。我们在循环中对这些 right - left 进行累加，就得到了所有小于等于 mid 的距离对数目。


[Leetcode](https://leetcode.com/problems/find-k-th-smallest-pair-distance/) / [力扣](https://leetcode-cn.com/problems/find-k-th-smallest-pair-distance/)

```python
def smallestDistancePair(self, nums: List[int], k: int) -> int:
    def distance(mid):
        count = left = 0
        for right, num in enumerate(nums):
            while num - nums[left] > mid:
                left += 1
            count += right - left
        return count
    # 1.减小混乱度，方便定义距离对的上限和下限以及查找
    nums.sort()
    # 距离对的上限与下限
    left, right = 0, nums[-1] - nums[0]
    while left < right:
        mid = left + (right - left) // 2
        ksmallest = distance(mid)
        if ksmallest >= k:
            right = mid
        else:
            left = mid + 1

    return left 
```

### 4. 第k个最小的素数分数

786\. K-th Smallest Prime Fraction (Hard)

[Leetcode](https://leetcode.com/problems/k-th-smallest-prime-fraction/) / [力扣](https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/)

```python
def kthSmallestPrimeFraction(self, arr: List[int], k: int) -> List[int]:
    def frac(mid):
        count = 0
        i = 0
        for j in range(1, len(arr)):
            while i < j and arr[i] / arr[j] <= mid:
                i += 1
            count += i
        return count
    
    EPS = 1e-9
    left, right = arr[0] / arr[-1], 1
    while right - left > EPS:
        mid = (right + left) / 2
        count = frac(mid)
        if count >= k:
            right = mid
        else:
            left = mid 
    
    for b in arr:
        a = round(left * b)
        if abs(a - left * b) < b * EPS:
            return a, b
```
