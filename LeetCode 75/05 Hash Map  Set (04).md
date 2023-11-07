# 0000 Hash Map / Set

---

在 LeetCode 等编程问题中提到的 "Hash Map" 和 "Hash Set" 是两种基于哈希表的数据结构。

**Hash Map (哈希映射)：**

- Hash Map 是一种存储键值对的数据结构，其中键是唯一的。
- 在 Python 中，Hash Map 对应的是 `dict` 类型。
- 适用情况：当你需要快速检索、插入和删除键值对时，Hash Map 是很有用的。例如，在频率计数、双数问题（Two Sum），或者需要快速访问数据时。

**Hash Set (哈希集合)：**

- Hash Set 是一组不重复元素的集合，它不存储任何键值对，只存储单一元素。
- 在 Python 中，Hash Set 对应的是 `set` 类型。
- 适用情况：当你需要存储一组不重复元素并且需要快速检查一个元素是否已经存在于集合中时，Hash Set 是有用的。例如，在去除重复元素、检查集合交集或并集时非常有用。

**哈希表的优点：**

- 时间复杂度：对于插入、删除和获取操作，哈希表通常提供平均情况下的常数时间复杂度 O(1)。
- 代码简洁：在很多编程语言中，Hash Map 和 Hash Set 的使用通常非常简洁直观。

**哈希表的缺点：**

- 空间复杂度：哈希表需要额外的内存来存储表结构。
- 最坏情况下的性能：尽管平均情况下是常数时间，但在最坏情况下（例如，所有元素都映射到同一哈希值时），性能可能会降到线性时间 O(n)。

在选择使用哈希表的数据结构时，要考虑到问题的需求，以及哈希表带来的时间效率和空间消耗之间的权衡。

# 2215 Find the Difference of Two Arrays

---

**题目翻译：**

给定两个0索引的整数数组`nums1`和`nums2`，返回一个大小为2的列表`answer`，其中：
- `answer[0]`是一个列表，其中包含所有在`nums1`中但不在`nums2`中的不同整数。
- `answer[1]`是一个列表，其中包含所有在`nums2`中但不在`nums1`中的不同整数。
注意，列表中的整数可以按任意顺序返回。

**知识点与解题思路：**

这道题的关键是找到两个数组中的差异。考虑到题目要求我们返回的整数应该是独特的，我们可以使用集合来解决这个问题。

具体步骤如下：
1. 将两个数组`nums1`和`nums2`转换为集合`set1`和`set2`。
2. 使用集合的差集运算找到`set1`中存在但`set2`中不存在的元素，这就是`answer[0]`。
3. 同理，找到`set2`中存在但`set1`中不存在的元素，这就是`answer[1]`。
4. 返回这两个列表作为结果。

Python代码解答：
```python
class Solution:
    def findDifference(self, nums1: List[int], nums2: List[int]) -> List[List[int]]:
        set1, set2 = set(nums1), set(nums2)
        return [list(set1 - set2), list(set2 - set1)]
```

时间复杂度：O(n + m) - n是`nums1`的长度，m是`nums2`的长度。我们需要将两个数组转换为集合，然后进行差集运算。
空间复杂度：O(n + m) - 我们创建了两个集合来存储`nums1`和`nums2`中的独特值。



# 1207 Unique Number of Occurrences

---

**题目翻译：**

给定一个整数数组`arr`，如果数组中每个值的出现次数是唯一的，则返回`true`，否则返回`false`。

**知识点与解题思路：**

为了解决这个问题，我们可以使用两个哈希表（或字典）。

1. 第一个字典用于计算数组中每个数字的出现次数。
2. 第二个字典用于检查出现次数的唯一性。

如果第二个字典中的任何值大于1，则返回`false`，因为这意味着存在两个不同的数，它们的出现次数是相同的。否则，返回`true`。

Python代码解答：
```python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        # 使用字典来计算每个数字的出现次数
        count = {}
        for num in arr:
            count[num] = count.get(num, 0) + 1
        
        # 使用另一个字典来检查出现次数的唯一性
        unique_counts = {}
        for key, value in count.items():
            if value in unique_counts:
                return False
            unique_counts[value] = 1
        
        return True
```

时间复杂度：O(n) - n是`arr`的长度。我们遍历整个数组来填充`count`字典，然后遍历`count`字典来填充`unique_counts`字典。
空间复杂度：O(n) - 在最坏的情况下，我们可能需要存储所有不同的数字和它们的出现次数。

# 1657 Determine if Two Strings Are Close

**题目翻译：**

两个字符串如果可以通过以下操作使一个变成另一个，则被认为是接近的：

操作1：交换任意两个现有字符。
例如，`abcde -> aecdb`

操作2：将一个现有字符的所有出现转换为另一个现有字符，并对另一个字符执行相同操作。
例如，`aacabb -> bbcbaa`（所有的a都变成了b，所有的b都变成了a）

你可以在任意一个字符串上使用这些操作任意次数。

给定两个字符串`word1`和`word2`，如果`word1`和`word2`是接近的，则返回`true`，否则返回`false`。

**知识点与解题思路：**

此题的核心在于理解两个字符串能够通过特定的操作变换来相互匹配，这意味着它们应当满足以下条件：

1. 两个字符串必须具有相同的字符集合。
2. 两个字符串中每个字符出现的次数必须相同，尽管这些字符出现的顺序可以不同。

具体步骤如下：

1. 检查两个字符串是否含有相同的独特字符。
2. 如果字符集合相同，进一步比较两个字符串中每个独特字符出现的次数是否一致。
3. 为此，我们需要统计每个字符串中每个字符出现的次数，并将这些次数排序后比较。

Python代码解答：
```python
class Solution:
    def closeStrings(self, word1: str, word2: str) -> bool:
        # 检查两个字符串是否含有相同的独特字符
        if set(word1) != set(word2):
            return False

        # 统计word1中每个字符的出现次数
        count1 = {}
        for char in word1:
            count1[char] = count1.get(char, 0) + 1
        
        # 统计word2中每个字符的出现次数
        count2 = {}
        for char in word2:
            count2[char] = count2.get(char, 0) + 1
        
        # 如果两个字符串中每个独特字符出现的次数一样，则它们是接近的
        # 将出现次数进行排序后比较
        return sorted(count1.values()) == sorted(count2.values())
```

时间复杂度：O(n log n) - n是字符串的长度。这是因为我们需要对字符出现次数的列表进行排序。
空间复杂度：O(n) - 我们需要存储每个字符串中每个字符出现的次数。

# 2352 Equal Row and Column Pairs

---

**题目翻译：**

给定一个0索引的`n x n`整数矩阵`grid`，返回一对`(ri, cj)`的数量，使得第`ri`行和第`cj`列相等。

如果一对行和列包含相同顺序的相同元素（即，相等的数组），则认为这对行和列是相等的。

例子 1：
输入: grid = [[3,2,1],[1,7,6],[2,7,7]]
输出: 1
解释: 有1对相等的行和列：

- （第2行，第1列）：[2,7,7]

例子 2：
输入: grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
输出: 3
解释: 有3对相等的行和列：

- （第0行，第0列）：[3,1,2,2]
- （第2行，第2列）：[2,4,2,2]
- （第3行，第2列）：[2,4,2,2]

约束条件：
- n == grid.length == grid[i].length
- 1 <= n <= 200
- 1 <= grid[i][j] <= 10^5

**知识点与解题思路：**

这个问题需要我们检查矩阵中任意一行和任意一列是否相同。为了解决这个问题，我们可以遍历矩阵的每一行和每一列，比较它们是否完全相同。

一个有效的解决方案是：
1. 遍历矩阵中的每一行。
2. 对于每一行，遍历矩阵的每一列。
3. 比较当前行和当前列的每个元素是否相等。
4. 如果所有元素都相等，则将相等对的计数增加1。

Python代码实现：
```python
class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        n = len(grid) # 矩阵的大小
        equal_pairs_count = 0 # 相等对的计数

        # 遍历矩阵的每一行
        for r in range(n):
            # 对于每一行，遍历矩阵的每一列
            for c in range(n):
                # 如果当前行和当前列的每个元素都相等
                if all(grid[r][i] == grid[i][c] for i in range(n)):
                    # 增加相等对的计数
                    equal_pairs_count += 1

        return equal_pairs_count
```

时间复杂度：O(n^3) - 我们需要遍历矩阵的每一行和每一列，并比较它们的元素。
空间复杂度：O(1) - 我们没有使用额外的空间，除了几个用于计数和迭代的变量。