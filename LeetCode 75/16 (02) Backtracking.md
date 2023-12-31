# 0000 Backtracking

---

在 LeetCode 及其他编程平台中，**Backtracking（回溯法）**是一种算法技术，用于解决那些需要通过尝试各种可能性来找到所有解的问题，特别是在搜索和约束满足问题中。它通常用于解决诸如排列、组合、棋盘问题（如八皇后问题）和分区问题（如图着色问题）等。

回溯法的工作原理是递归地构建解决方案的候选解，一旦发现候选解不是有效解或者不可能进一步构成有效解时，就“回溯”到上一步进行其他选择。这个过程就像在决策树上探索路径，当路径达到一个“死端”时，退回一步或多步尝试其他可能的选项。

回溯法适用的情况包括：

1. **组合问题**：如找出一组元素所有可能的组合方式。
2. **排列问题**：如找出一组元素所有可能的排列方式。
3. **分割问题**：如将集合分割成满足特定条件的多个子集。
4. **棋盘问题**：如八皇后问题，即在棋盘上放置棋子，使它们互不攻击。
5. **图的遍历**：在图中寻找路径或者解决某些约束满足问题。
6. **子集问题**：找出一个集合的所有子集。

回溯法通常与深度优先搜索（DFS）结合使用，因为它在尝试所有可能性的过程中深入到解空间的底部。这种方法的关键是剪枝，即及时停止对无效解或无法产生有效解的路径的进一步探索，从而减少计算量。

# 0017 Letter Combinations of a Phone Number

---

**题目**

《17. 电话号码的字母组合》是一个中等难度的回溯算法问题。

题目描述：给定一个包含数字`2-9`的字符串，返回该数字可能代表的所有可能的字母组合。答案可以以任何顺序返回。
![https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png](https://)

数字到字母的映射如下所示（就像电话按钮上的那样）。注意1不映射到任何字母。

示例 1：

输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
示例 2：

输入：digits = ""
输出：[]
示例 3：

输入：digits = "2"
输出：["a","b","c"]

限制条件：

- 0 <= digits.length <= 4
- digits[i] 是范围 ['2', '9'] 内的一个数字。

**思路**

涉及的知识点包括回溯算法。对于输入的每个数字，我们查找其可能代表的所有字母，并对每个可能的字母递归地继续查找下一个数字所代表的字母，直到处理完所有输入的数字。我们使用回溯算法来生成所有可能的字母组合。

**解法**

以下是该题的 Python 解法，包括详细的中文注释，说明了其空间复杂度和时间复杂度：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        
        # 数字到字母的映射
        d = ["", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"]
        n = len(digits)  # 输入数字的长度
        a = []  # 存储所有组合的结果列表
        
        # 回溯函数
        def backtrack(i, s):
            if len(s) == n:  # 如果当前字符串长度等于输入数字的长度，将其添加到结果列表
                a.append("".join(s))
                return
            if i == n:  # 如果当前索引等于输入数字的长度，返回
                return
            for j in d[int(digits[i])]:  # 遍历当前数字对应的所有字母
                s.append(j)  # 将字母添加到当前字符串
                backtrack(i + 1, s)  # 递归处理下一个数字
                s.pop()  # 回溯，移除当前字符串的最后一个字母
        
        backtrack(0, [])  # 从第一个数字开始回溯
        return a
```

空间复杂度：O(3^N * 4^M)，其中 N 是映射到3个字母的数字数量，M 是映射到4个字母的数字数量，这是因为最坏情况下递归调用的栈空间和存储结果的空间。

时间复杂度：O(3^N * 4^M)，每个生成的字符串长度都是 N+M，这是每个组合的生成时间，总共有 3^N * 4^M 个组合。


这种解法已经是这个问题的最优解法，因为它能够遍历生成所有可能的字母组合，没有更高效的方法能够减少所需的时间复杂度。在这个问题中，我们无法使用比回溯更高效的算法来降低时间复杂度。

# 0216 Combination Sum III

---

**题目翻译**

题目《216. 组合总和 III》要求我们找到所有唯一的 k 个数字组合，这些数字的和为 n，并且满足以下条件：

- 仅使用数字 1 到 9。
- 每个数字最多使用一次。

返回所有可能的有效组合的列表。列表不能包含相同的组合两次，组合可以以任何顺序返回。

示例 1：

输入：k = 3, n = 7
输出：[[1,2,4]]
解释：1 + 2 + 4 = 7，没有其他有效的组合。

示例 2：

输入：k = 3, n = 9
输出：[[1,2,6],[1,3,5],[2,3,4]]
解释：1 + 2 + 6 = 9，1 + 3 + 5 = 9，2 + 3 + 4 = 9，没有其他有效的组合。

示例 3：

输入：k = 4, n = 1
输出：[]
解释：没有有效的组合。使用范围 [1,9] 内的 4 个不同数字，我们能得到的最小和是 1+2+3+4 = 10，因为 10 > 1，所以没有有效的组合。

限制条件：

- 2 <= k <= 9
- 1 <= n <= 60

**解题思路**

解题思路涉及到回溯算法，我们需要遍历所有可能的数字组合，并且记录下来满足和为 n 的组合。这个过程中我们要确保每个数字只使用一次，并且组合中的数字个数为 k。

**Python 解法**

以下是 Python 解法，包含详细的中文注释：

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        # 初始解集为空
        res = []
        
        # 使用回溯算法来构建解
        def backtrack(start, combination, target):
            # 如果组合中数字的数量达到 k 并且目标和为 0，则找到一个解
            if len(combination) == k and target == 0:
                res.append(list(combination))
                return
            # 遍历从 start 到 9 的数字
            for i in range(start, 10):
                # 选择当前数字 i
                combination.append(i)
                # 回溯并更新目标和为 target - i
                backtrack(i + 1, combination, target - i)
                # 撤销选择
                combination.pop()
        
        backtrack(1, [], n)
        return res
```

空间复杂度：O(k)，k 为递归栈的深度。

时间复杂度：O(C(9, k))，即从 9 个数字中选择 k 个进行组合的总可能数。

在这段代码中，我们定义了一个回溯函数 `backtrack`，它会递归地构建每个可能的组合，并在找到一个有效的组合时将其添加到解集中。这种方法在时间和空间复杂度上都是最优的，因为它能够在保证不重复的前提下遍历所有可能的组合。由于题目的限制（使用数字 1-9，组合长度为 k），没有更快的算法能够降低时间复杂度。