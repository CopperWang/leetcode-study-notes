# 03 Sliding Window

## 0643 Maximum Average Subarray I

---

**题目翻译：**

给定一个由`n`个元素组成的整数数组`nums`和一个整数`k`。

找到一个连续的子数组，其长度等于`k`且具有最大的平均值，并返回该值。任何计算误差小于`10^-5`的答案都将被接受。

**知识点与解题思路：**

我们可以使用滑动窗口来解决这个问题。先计算前k个数字的总和作为初始值，然后向右移动窗口，减去最左边的数字并加上右边新进入窗口的数字来更新子数组的总和。记录下最大的子数组总和，最后将其除以k得到最大的平均值。

Python代码解答：
```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # 计算前k个数字的总和作为初始值
        current_sum = sum(nums[:k])
        max_sum = current_sum
        
        # 使用滑动窗口的方法更新子数组的总和并记录最大值
        for i in range(1, len(nums) - k + 1):
            current_sum = current_sum - nums[i - 1] + nums[i + k - 1]
            max_sum = max(max_sum, current_sum)
        
        # 返回最大的平均值
        return max_sum / k
```

时间复杂度：O(n) - 我们只遍历数组一次。

空间复杂度：O(1) - 我们只使用了常数级别的额外空间。

## 1456 Maximum Number of Vowels in a Substring of Given Length

---

**题目翻译：**

给定一个字符串`s`和一个整数`k`，返回长度为`k`的`s`的任何子串中的元音字母的最大数量。

英文中的元音字母是'a', 'e', 'i', 'o'和'u'。

**知识点与解题思路：**

此题可以利用滑动窗口的方法来寻找最大的元音字母数。首先，计算长度为k的窗口内的元音字母数，然后将窗口向右移动，并相应地增加或减少元音字母的计数。在每一步中，都记录下元音字母的最大数量。

Python代码解答：
```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        # 初始化元音字母的集合
        vowels = set("aeiou")
        
        # 计算窗口的初始值
        current_count = sum(1 for char in s[:k] if char in vowels)
        max_count = current_count
        
        # 使用滑动窗口的方法更新元音字母的计数并记录最大值
        for i in range(1, len(s) - k + 1):
            if s[i - 1] in vowels:
                current_count -= 1
            if s[i + k - 1] in vowels:
                current_count += 1
            max_count = max(max_count, current_count)
        
        return max_count
```

时间复杂度：O(n) - n是字符串s的长度，我们只遍历一次字符串s。

空间复杂度：O(1) - 我们使用了常数级别的额外空间。

## 1004 Max Consecutive Ones III

---

**题目翻译：**

给定一个二进制数组`nums`和一个整数`k`，如果你最多可以翻转`k`个0，则返回数组中连续1的最大数量。

**知识点与解题思路：**

这道题目可以使用滑动窗口的方法来解决。思路如下：
1. 使用两个指针，`left`和`right`，表示窗口的左右边界。
2. 移动`right`指针遍历整个数组，并计算窗口中的0的数量。
3. 如果窗口中0的数量超过了`k`，则移动`left`指针并相应地调整0的数量，直到窗口中0的数量小于或等于`k`。
4. 在整个过程中，记录窗口的最大长度，该长度即为连续1的最大数量。

Python代码解答：
```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        left, right = 0, 0
        max_length = 0
        zeros = 0   # 窗口内0的数量
        
        while right < len(nums):
            # 如果当前数字是0，更新0的数量
            if nums[right] == 0:
                zeros += 1
            
            # 如果窗口内0的数量超过了k，移动左指针
            while zeros > k:
                if nums[left] == 0:
                    zeros -= 1
                left += 1
            
            # 更新最大长度
            max_length = max(max_length, right - left + 1)
            right += 1
        
        return max_length
```

时间复杂度：O(n) - n是数组`nums`的长度，我们遍历一次`nums`。
空间复杂度：O(1) - 使用了常数级别的额外空间。

## 1493 Longest Subarray of 1's After Deleting One Element

**题目翻译：**

给定一个二进制数组`nums`，你应该从中删除一个元素。

返回结果数组中只包含`1`的最长的非空子数组的大小。如果没有这样的子数组，则返回`0`。

**知识点与解题思路：**

我们可以使用滑动窗口的方法来解决这个问题。我们的目标是找到只包含一个0和其余为1的最长子数组，因为我们只能删除一个0，这意味着在最优的情况下，我们应该删除那个0来获得最长的全1子数组。

1. 使用两个指针，`left`和`right`，表示窗口的左右边界。
2. 移动`right`指针遍历整个数组，并计算窗口中的0的数量。
3. 如果窗口中0的数量超过了1，就移动`left`指针并相应地调整0的数量，直到窗口中0的数量小于或等于1。
4. 在整个过程中，记录窗口的最大长度。
5. 最后的结果应该是`max_length - 1`，因为我们必须删除一个元素。

Python代码解答：
```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        left, right = 0, 0
        max_length = 0
        zeros = 0   # 窗口内0的数量
        
        while right < len(nums):
            # 如果当前数字是0，更新0的数量
            if nums[right] == 0:
                zeros += 1
            
            # 如果窗口内0的数量超过了1，移动左指针
            while zeros > 1:
                if nums[left] == 0:
                    zeros -= 1
                left += 1
            
            # 更新最大长度
            max_length = max(max_length, right - left + 1)
            right += 1
        
        # 最后的结果应该减去1，因为我们必须删除一个元素
        return max_length - 1 if max_length > 0 else 0
```

时间复杂度：O(n) - n是数组`nums`的长度，我们遍历一次`nums`。
空间复杂度：O(1) - 使用了常数级别的额外空间。