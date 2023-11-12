# 0000 Array String

---

在 LeetCode 或其他算法平台中，"Array" 和 "String" 是两种基本的数据结构类型，通常都有专门的题型分类。

**数组（Array）：**

- 数组是一种线性数据结构，它可以存储一定数量的元素，这些元素的类型都是相同的。
- 在算法问题中，数组可以用来存储数字、字符或其他类型的数据。
- 数组的元素可以通过索引（通常从 0 开始）来快速访问和修改。

**字符串（String）：**

- 字符串是一系列字符的集合，通常被视为字符数组。
- 在算法问题中，字符串处理是一个常见的主题，涉及到搜索、排序、转换、字符串匹配等操作。

**Array 和 String 题型适用场景：**

1. **查找和排序**：如在数组中查找给定值，或对数组进行排序。
2. **动态规划**：许多涉及到字符串和数组的动态规划问题，如最长公共子序列、最长递增子序列等。
3. **双指针技巧**：在数组和字符串问题中常用，如反转数组、快慢指针找环等。
4. **滑动窗口**：在处理连续数据或需要考虑连续子序列问题时，如找出无重复字符的最长子串。
5. **哈希表**：用于快速查找、去重或统计频率等，如在字符串中统计每个字符出现的次数。

在编程中，数组和字符串是最基础的数据结构之一，几乎所有的程序都会用到它们。由于它们的普遍性和重要性，LeetCode 等平台会有大量的问题专门针对这两种数据结构，以帮助开发者熟练掌握相关的算法和技巧。

# 1768 Merge Strings Alternately

---

**题目描述**

给定两个字符串 `word1` 和 `word2`。你需要通过交替添加字母的方式合并这两个字符串，首字母从 `word1` 开始。如果一个字符串比另一个字符串长，那么将多余的字母追加到合并后的字符串末尾。

返回合并后的字符串。

题目涉及的知识点：
1. 字符串的操作，如：遍历、连接等。
2. 条件判断。

使用Python解决这个问题的方法：

我们可以使用双指针方法，分别遍历两个字符串，直到其中一个字符串遍历完为止。在此过程中，我们将交替地从两个字符串中取出字符并添加到结果字符串中。最后，如果两个字符串中的一个还有剩余字符，则直接将其添加到结果字符串的末尾。

具体代码如下：

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        # 定义两个指针，分别指向两个字符串的开始
        i, j = 0, 0
        # 初始化结果字符串
        result = ""
        
        # 当两个字符串都未遍历完时，交替添加字符到结果字符串中
        while i < len(word1) and j < len(word2):
            # 先从word1添加字符
            result += word1[i]
            i += 1
            # 然后从word2添加字符
            result += word2[j]
            j += 1
        
        # 如果word1还有剩余字符，直接添加到结果字符串的末尾
        while i < len(word1):
            result += word1[i]
            i += 1
            
        # 如果word2还有剩余字符，直接添加到结果字符串的末尾
        while j < len(word2):
            result += word2[j]
            j += 1
        
        # 返回合并后的字符串
        return result
```

时间复杂度：O(n)，其中 n 是两个字符串中较长的那个的长度。

空间复杂度：O(n)，用于存储合并后的字符串。

# 1071 Greatest Common Divisor of Strings

---

这个问题的关键是找到两个字符串的最大公约数字符串。这实际上与找数字的最大公约数是相似的。对于数字，我们有欧几里得算法，我们也可以使用这种方法来找字符串的最大公约数。

考虑字符串的结构，如果 `str1` 是 `str2` 的多倍数，那么 `str1` 中去掉 `str2` 的部分（从前面开始），剩下的部分仍然是 `str2` 的倍数。所以，我们可以通过不断地减去较小字符串的方式，来寻找两个字符串的最大公约数字符串。

具体算法如下：
1. 如果 `str1 + str2` 与 `str2 + str1` 不相等，那么返回空字符串。这是因为如果两个字符串有公约数字符串，它们拼接起来的结果应该是相同的。
2. 使用欧几里得算法：假设 `str1` 是长度较大的字符串，`str2` 是长度较小的字符串，不断地用 `str1` 去减去 `str2`，直到其中一个字符串变为空为止。剩下的那个字符串就是我们要找的最大公约数字符串。

以下是相应的Python代码：

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        # 如果str1和str2有最大公约数字符串，它们拼接的结果应该是相同的
        if str1 + str2 != str2 + str1:
            return ""

        # 使用欧几里得算法
        while str2:
            str1, str2 = str2, str1.replace(str2, '', 1)
            # str1.replace(str2,'', 1): 
                # 这会在str1中查找str2的首次出现，
                # 并将它替换为一个空字符串（即删除它）。
                # 1是一个可选参数，表示最大替换次数，这里意味着仅替换首次出现。

        return str1
```

时间复杂度：O(n)，其中 n 是两个字符串的总长度。

空间复杂度：O(n)。

# 1431 Kids With the Greatest Number of Candies

---

**题目翻译**

题目：1431. 拥有最多糖果的孩子

有 n 个孩子有糖果。你得到了一个整数数组 candies，其中每个 candies[i] 代表第 i 个孩子拥有的糖果数量，还有一个整数 extraCandies，表示你有的额外糖果数量。

返回一个长度为 n 的布尔数组 result，其中 result[i] 为真，表示如果给第 i 个孩子所有额外的糖果，他们将拥有所有孩子中最多的糖果，否则为假。

注意，多个孩子可以拥有最多的糖果。

示例：
1. 输入：candies = [2,3,5,1,3], extraCandies = 3 输出：[true,true,true,false,true]
2. 输入：candies = [4,2,1,1,2], extraCandies = 1 输出：[true,false,false,false,false]
3. 输入：candies = [12,1,12], extraCandies = 10 输出：[true,false,true]

约束条件：
- n == candies.length
- 2 <= n <= 100
- 1 <= candies[i] <= 100
- 1 <= extraCandies <= 50

**知识点**

这道题考查了数组的基本操作和逻辑判断。

**解题方法**

为了判断每个孩子给了额外的糖果后是否会有最多的糖果，我们只需要查看他加上额外糖果后的总数是否大于等于当前所有孩子中的最大糖果数。

详细步骤：
1. 首先，找出数组中的最大值。
2. 遍历数组，检查每个孩子的糖果数加上额外糖果是否大于等于最大值。
3. 根据上一步的判断结果，构建并返回结果数组。

Python代码解答：
```python
class Solution:
    def kidsWithCandies(self, candies: List[int], extraCandies: int) -> List[bool]:
        # 找到当前孩子中拥有的最大糖果数
        maxCandies = max(candies)
        # 根据每个孩子的糖果数加上额外糖果后的总数是否大于等于最大值，来构建结果列表
        return [candy + extraCandies >= maxCandies for candy in candies]
```

时间复杂度：O(n) - n 是 candies 的长度。我们进行了一次遍历找最大值和一次遍历构建结果列表。
空间复杂度：O(1) - 除了结果列表外，我们只使用了常数空间。

# 605 Can Place Flowers

---

**题目翻译**

题目：605. 种花问题

你有一个长的花坛，其中某些地块已经种植了花，而有些地块则没有。但是，花不能种植在相邻的地块上。

给定一个整数数组 `flowerbed`，其中 `0` 表示空地，`1` 表示已种植，并给定一个整数 `n`，如果在不违反不种植相邻花的规则的情况下，能在花坛中种植 `n` 朵新花，则返回 `true`，否则返回 `false`。

示例：
1. 输入：flowerbed = [1,0,0,0,1], n = 1 输出：true
2. 输入：flowerbed = [1,0,0,0,1], n = 2 输出：false

约束条件：
- `1 <= flowerbed.length <= 2 * 10^4`
- `flowerbed[i]` 是` 0` 或 `1`。
- 在 `flowerbed` 中没有两朵相邻的花。
- `0 <= n <= flowerbed.length`

**知识点：**

这道题考查了数组的基本操作和逻辑判断。

**解题方法：**

为了判断能否在花坛中种植 `n` 朵新花，我们需要遍历整个花坛并查找可以种植的位置。

详细步骤：
1. 遍历花坛，当我们发现一个空的地块 (即 `flowerbed[i] == 0`) 时，我们检查其前后是否也是空地。
2. 如果前后都是空地，或者是花坛的边界，并且该地块是空地，那么我们就在这个地块种植一朵花。
3. 当遍历完花坛后，比较我们种植的花的数量与 `n`，如果种植的花的数量大于等于 `n`，则返回 `true`，否则返回 `false`。

Python代码解答：
```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        count = 0 # 用于记录种植的花的数量
        i = 0
        length = len(flowerbed)

        while i < length:
            # 检查当前地块是否是空的，并且前后都是空地或者是花坛的边界
            if flowerbed[i] == 0 and (i == 0 or flowerbed[i-1] == 0) and (i == length-1 or flowerbed[i+1] == 0):
                flowerbed[i] = 1 # 种植一朵花
                count += 1 # 更新种植的花的数量
                i += 2 # 由于不能种植相邻的花，所以跳过下一个地块
            else:
                i += 1

        # 检查种植的花的数量是否大于等于 n
        return count >= n
```

时间复杂度：O(length) - length 是 flowerbed 的长度。我们进行了一次遍历。
空间复杂度：O(1) - 我们只使用了常数空间。

# 345 Reverse Vowels of a String

---

**题目翻译：**

题目：345. 反转字符串中的元音字母

给定一个字符串 `s`，只反转字符串中的所有元音字母并返回它。

元音字母为 `'a'`、`'e'`、`'i'`、`'o'` 和 `'u'`，它们可以以小写或大写的形式出现，而且可以出现多次。

示例：
1. 输入：`s = "hello"` 输出：`"holle"`
2. 输入：`s = "leetcode"` 输出：`"leotcede"`

约束条件：
- `1 <= s.length <= 3 * 10^5`
- `s` 由可打印的ASCII字符组成。

**知识点：**

这道题考查了字符串的基本操作和双指针技巧。

**解题方法：**

为了只反转字符串中的元音字母，我们可以使用双指针的技巧。

详细步骤：
1. 定义两个指针：一个从头开始，另一个从尾部开始。
2. 当两个指针没有交叉时，继续以下操作：
    - 如果左指针指向的字符不是元音字母，就向右移动左指针。
    - 如果右指针指向的字符不是元音字母，就向左移动右指针。
    - 当左右指针都指向元音字母时，交换这两个元音字母的位置，然后移动这两个指针。
3. 当两个指针交叉时，停止操作并返回字符串。

Python代码解答：
```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        # 定义元音字母集合，用于快速判断一个字母是否为元音字母
        vowels = set("aeiouAEIOU")
        s = list(s) # 转换字符串为列表，以便进行修改
        left, right = 0, len(s) - 1 # 初始化左右指针

        # 当左指针没有超过右指针时
        while left < right:
            # 移动左指针直到它指向一个元音字母
            while left < right and s[left] not in vowels:
                left += 1

            # 移动右指针直到它指向一个元音字母
            while left < right and s[right] not in vowels:
                right -= 1

            # 交换两个元音字母的位置
            s[left], s[right] = s[right], s[left]

            # 移动左右指针
            left += 1
            right -= 1

        # 返回修改后的字符串
        return ''.join(s)
```

时间复杂度：O(n) - n 是 s 的长度。我们最多进行一次完整的遍历。
空间复杂度：O(n) - 我们将字符串转换为列表，需要额外的空间。

# 151 Reverse Words in a String

---

**题目翻译：**

题目：151. 翻转字符串里的单词

给定一个输入字符串 `s`，反转其中的单词顺序。

单词定义为一系列非空格字符。`s` 中的单词将由至少一个空格分隔。

返回由单个空格连接的反转顺序的单词组成的字符串。

注意，`s` 可能包含前导或尾部空格，或者两个单词之间的多个空格。返回的字符串应该只有一个空格分隔单词，不要包含任何额外的空格。

示例：
1. 输入：`s = "the sky is blue"` 输出：`"blue is sky the"`
2. 输入：`s = "  hello world  "` 输出：`"world hello"` 
解释：您反转的字符串不应该包含前导或尾部空格。
3. 输入：`s = "a good   example"` 输出：`"example good a"` 
解释：在反转的字符串中，您需要将两个单词之间的多个空格减少到一个空格。

约束条件：
- 1 <= s.length <= 104
- s 包含英文字母（大写和小写）、数字和空格 ' '。
- s 中至少有一个单词。

后续问题：如果在您的语言中字符串数据类型是可变的，您能否用 O(1) 的额外空间解决它？

**知识点：**

这道题主要考查了字符串的操作。

**解题方法：**

1. 为了翻转字符串中的单词，首先我们可以通过 Python 的内建 `split()` 方法来按空格分割字符串，得到一个单词列表。
2. 使用 `reversed` 函数或列表的反转方法，来反转该单词列表。
3. 使用 `join` 方法，用空格来连接反转后的单词列表。

Python代码解答：
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 1. 使用 split() 分割字符串得到单词列表
        # 2. 使用 reversed 函数反转单词列表
        # 3. 使用 join 方法用空格连接反转后的单词列表
        return ' '.join(reversed(s.split()))
```

时间复杂度：O(n) - n 是 s 的长度。我们进行了一次分割和一次反转操作。
空间复杂度：O(n) - 存储分割后的单词列表需要额外的空间。

对于后续问题，Python 的字符串是不可变的，因此在 Python 中不能直接在原地使用 O(1) 额外空间来解决。但在其他支持可变字符串的语言中，这个问题可以通过双指针技巧在原地解决。

# 238 Product of Array Except Self

---

**题目翻译：**

题目：238. 除自身以外数组的乘积

给定一个整数数组 `nums`，返回一个数组 `answer`，使得 `answer[i]` 等于 `nums` 中除了 `nums[i]` 之外的所有元素的乘积。

`nums` 的任何前缀或后缀的乘积保证在 32 位整数中适应。

你必须写一个算法，运行时间为 `O(n)`，且不使用除法操作。

示例：
1. 输入：`nums = [1,2,3,4]` 输出：`[24,12,8,6]`
2. 输入：`nums = [-1,1,0,-3,3]` 输出：`[0,0,9,0,0]`

约束条件：
- `2 <= nums.length <= 10^5`
- `-30 <= nums[i] <= 30`
- `nums` 的任何前缀或后缀的乘积保证在 32 位整数中适应。

后续问题：你能在 `O(1)` 的额外空间复杂度下解决这个问题吗？（输出数组不计算为空间复杂度分析的额外空间。）

**知识点：**

这道题测试了数组操作的能力。

**解题方法：**

1. 创建一个数组 `left`，使得 `left[i]` 等于 `nums[0]` 到 `nums[i-1]` 的乘积。
2. 创建一个数组 `right`，使得 `right[i]` 等于 `nums[i+1]` 到 `nums[n-1]` 的乘积。
3. 为了得到最终结果，`answer[i] = left[i] * right[i]`。

但上述方法使用了 `O(n)` 的额外空间。为了达到 `O(1)` 的额外空间复杂度，我们可以：
1. 首先计算 `left` 数组，并将其存储在结果数组 answer 中。
2. 之后，我们可以用一个变量来计算 `right` 数组，并同时更新 `answer` 数组。

Python代码解答：
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        answer = [1] * n
        
        # 计算 left 乘积
        left_product = 1
        for i in range(1, n):
            left_product *= nums[i-1]
            answer[i] *= left_product
        
        # 计算 right 乘积并更新 answer
        right_product = 1
        for i in range(n-2, -1, -1):
            right_product *= nums[i+1]
            answer[i] *= right_product
        
        return answer
```

时间复杂度：O(n) - 我们遍历了 nums 两次。
空间复杂度：O(1) - 我们只使用了常数级别的额外空间，不包括输出数组。

# 334 Increasing Triplet Subsequence

---

**题目翻译：**

题目：存在递增的三元子序列

给定一个整数数组 `nums`，如果存在三个索引的三元组 `(i, j, k)` 使得 `i < j < k` 且 `nums[i] < nums[j] < nums[k]`，则返回 `true`。如果不存在这样的索引，返回 `false`。

示例：
1. 输入：`nums = [1,2,3,4,5]` 输出：`true`。任何满足 `i < j < k` 的三元组都是有效的。
2. 输入：`nums = [5,4,3,2,1]` 输出：`false`。不存在这样的三元组。
3. 输入：`nums = [2,1,5,0,4,6]` 输出：`true`。三元组 `(3, 4, 5)` 是有效的，因为 `nums[3] == 0 < nums[4] == 4 < nums[5] == 6`。

约束条件：
- `1 <= nums.length <= 5 * 10^5`
- `-2^31 <= nums[i] <= 2^31 - 1`

后续问题：你能实现一个时间复杂度为 `O(n)` 和空间复杂度为 `O(1)` 的解决方案吗？

**解题方法：**

我们可以用两个变量 `first` 和 `second` 来保存目前为止找到的最小值和次小值。我们遍历数组一次，首先看是否可以更新 `first`，否则看是否可以更新 `second`，否则返回 `true`。

Python代码解答：
```python
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        first, second = float('inf'), float('inf')
        
        for num in nums:
            if num <= first:
                first = num
            elif num <= second:
                second = num
            else:
                return True
        
        return False
```

时间复杂度：O(n) - 我们遍历了数组一次。
空间复杂度：O(1) - 只使用了常数级别的额外空间。

# 443 String Compression

---

**题目翻译：**

给定一个字符数组 `chars`，使用以下算法对其进行压缩：

从一个空字符串 `s` 开始。对于 `chars` 中的每组连续重复字符：

- 如果组的长度为1，将字符附加到 `s`。
- 否则，附加字符后跟该组的长度。
  

压缩后的字符串 `s` 不应单独返回，而应存储在输入字符数组 `chars` 中。注意，长度为10或更长的组会在 `chars` 中分为多个字符。

完成修改输入数组后，返回数组的新长度。

您必须编写一个只使用常量额外空间的算法。

**知识点与解题思路：**

这道题测试的是字符串的处理和数组的原地操作。我们需要在原始数组上进行修改，不能使用额外的空间。

步骤如下：

1. 定义两个指针，一个用于遍历数组（读指针），另一个用于写入字符和计数（写指针）。
2. 遍历字符数组，对于每个字符，计算其连续重复的次数。
3. 根据连续重复的次数和字符修改原数组，并移动写指针。
4. 最后返回写指针的位置，即为新数组的长度。

Python代码解答：
```python
class Solution:
    def compress(self, chars: List[str]) -> int:
        read_pointer = 0  # 读指针
        write_pointer = 0  # 写指针

        while read_pointer < len(chars):
            char = chars[read_pointer]
            count = 0
            
            # 计算字符连续重复的次数
            while read_pointer < len(chars) and chars[read_pointer] == char:
                read_pointer += 1
                count += 1
            
            # 写入字符
            chars[write_pointer] = char
            write_pointer += 1
            
            # 如果字符连续重复次数大于1，将次数写入数组
            if count > 1:
                for ch in str(count):
                    chars[write_pointer] = ch
                    write_pointer += 1
        
        # 返回新数组的长度
        return write_pointer
```

时间复杂度：`O(n)` - 我们遍历了数组一次。
空间复杂度：`O(1)` - 只使用了常数级别的额外空间。