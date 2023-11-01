# 04 Prefix Sum

## 1732 Find the Highest Altitude

---

**题目翻译：**

有一个骑手正在进行公路旅行。这次旅行由`n + 1`个点组成，每个点的海拔都不同。骑手在海拔为`0`的点`0`开始他的旅行。

你被给定了一个长度为`n`的整数数组`gain`，其中`gain[i]`是点`i`和点`i + 1`之间的净海拔增益，对于所有的`(0 <= i < n)`。返回某个点的最高海拔。

**知识点与解题思路：**

我们需要计算每一个点的海拔高度，然后找到其中的最大值。为了计算每一个点的海拔，我们可以初始化一个变量`altitude`为0，然后逐步加上`gain`数组中的每一个值。这样，`altitude`在每一步都会表示当前点的海拔高度。

具体步骤如下：

1. 初始化一个变量`altitude`为0，表示当前海拔高度。
2. 初始化一个变量`max_altitude`为0，表示最大海拔高度。
3. 遍历`gain`数组中的每一个值，更新`altitude`并检查是否需要更新`max_altitude`。
4. 返回`max_altitude`。

Python代码解答：
```python
class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        altitude = 0
        max_altitude = 0
        
        for g in gain:
            altitude += g
            max_altitude = max(max_altitude, altitude)
            
        return max_altitude
```

时间复杂度：O(n) - n是数组`gain`的长度，我们遍历一次`gain`。
空间复杂度：O(1) - 使用了常数级别的额外空间。

## 0724 Find Pivot Index

---

**题目翻译：**

给定一个整数数组`nums`，计算这个数组的枢轴索引。

枢轴索引是指数组中的一个位置，其左边的所有数字之和严格等于其右边的所有数字之和。

如果该索引位于数组的左边界，则左边的和为0，因为左边没有元素。这也适用于数组的右边界。

返回最左边的枢轴索引。如果不存在这样的索引，返回-1。

**知识点与解题思路：**

1. **前缀和**：在解决此类问题时，前缀和是一个非常有用的技巧。它可以在O(1)的时间内得到某个范围的和。
2. **双重遍历**：一次遍历计算总和，另一次遍历找枢轴。
3. **数学技巧**：对于任意位置i，其左边的和可以通过前缀和直接得到，而其右边的和则可以通过`总和 - 左边的和 - nums[i]`得到。

具体步骤如下：

1. 首先，计算`nums`的总和。
2. 初始化一个变量`leftSum`为0，表示当前索引左边的所有数字之和。
3. 遍历`nums`的每一个数字，对于当前的数字`nums[i]`，如果`leftSum == 总和 - leftSum - nums[i]`，则返回i。
4. 每次遍历后，更新`leftSum += nums[i]`。
5. 如果遍历完都没找到，则返回-1。

Python代码解答：
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        totalSum = sum(nums)  # 计算总和
        leftSum = 0  # 初始化左边的和为0

        for i, num in enumerate(nums):  # 遍历数组
            # 如果左边的和等于右边的和，返回当前索引
            if leftSum == totalSum - leftSum - num:
                return i
            # 更新左边的和
            leftSum += num

        # 如果没找到符合条件的索引，返回-1
        return -1
```

时间复杂度：O(n) - n是数组`nums`的长度，我们遍历一次`nums`计算总和，再遍历一次找枢轴。
空间复杂度：O(1) - 使用了常数级别的额外空间。