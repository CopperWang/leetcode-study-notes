# 1768 Merge Strings Alternately

题目描述：

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



# Can Place Flowers

# Reverse Vowels of a String

# Reverse Words in a String

# Product of Array Except Self

# Increasing Triplet Subsequence

# String Compression