# 0000 Queue

---

在编程中，队列（Queue）是一种先进先出（First-In-First-Out, FIFO）的数据结构。队列支持两种主要操作：入队（enqueue），将一个元素放到队列的尾部；和出队（dequeue），从队列的头部移除一个元素。

在LeetCode或其他编程问题中，队列经常用在以下场景：

1. **广度优先搜索（BFS）**：当你使用广度优先搜索算法遍历图或树时，队列用来保存待处理的节点，确保它们的处理顺序符合BFS的要求。

2. **缓存和调度**：在操作系统或网络请求处理等场景中，队列可以用于任务调度，确保请求按接收顺序处理。

3. **同步**：在多线程程序中，队列可以用于线程之间的数据传递和同步。

4. **实时计算**：在需要按顺序处理数据流的场合，队列可以存储一系列的事件或计算结果。

5. **击键事件处理**：在用户界面编程中，队列用来管理用户的击键或其他事件，确保它们按照发生顺序被处理。

Python内置了一个名为 `queue.Queue` 的线程安全队列实现，它非常适合在多线程程序中用于线程间的信息传递。对于LeetCode这类算法问题，你通常会使用集合（collections）模块中的 `deque` 类来模拟队列，因为它提供了快速的从两端操作元素的方法，适合实现标准队列和双端队列（deque）的行为。

以下是一个使用 `collections.deque` 实现的队列的简单例子：

```python
from collections import deque

# 创建一个队列
queue = deque()

# 入队
queue.append('a')
queue.append('b')
queue.append('c')

# 出队
print(queue.popleft())  # 输出: 'a'
print(queue.popleft())  # 输出: 'b'

# 查看队列中剩余的元素
print(queue)  # 输出: deque(['c'])
```

在这个例子中，`append` 方法将元素添加到队列的尾部，而 `popleft` 方法从队列的头部移除元素。这模拟了队列的先进先出行为。

# 0933 Number of Recent Calls

---

**题目翻译：**

你有一个`RecentCounter`类，它用来计数在一定时间范围内的最近请求的数量。

实现`RecentCounter`类：

- `RecentCounter()` 初始化计数器，最近的请求次数为零。
- `int ping(int t)` 在时间`t`添加一个新的请求，其中`t`表示以毫秒为单位的某个时间，并返回过去3000毫秒内（包括新请求）发生的请求次数。具体来说，返回在闭区间`[t - 3000, t]`内发生的请求次数。

保证每次调用`ping`方法时，传入的`t`都比之前的调用要大。

示例 1:
输入
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]
输出
[null, 1, 2, 3, 3]

解释
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // 请求 = [1], 范围是 [-2999,1], 返回 1
recentCounter.ping(100);   // 请求 = [1, 100], 范围是 [-2900,100], 返回 2
recentCounter.ping(3001);  // 请求 = [1, 100, 3001], 范围是 [1,3001], 返回 3
recentCounter.ping(3002);  // 请求 = [1, 100, 3001, 3002], 范围是 [2,3002], 返回 3

约束条件:

- 1 <= t <= 10^9
- 每个测试案例都会用严格递增的`t`值来调用`ping`。
- 最多会调用104次`ping`。

**解题思路：**

这道题目可以通过使用队列的数据结构来维护一个滑动窗口。队列存储的是在时间范围`[t-3000, t]`内的所有请求时间戳。当调用`ping(t)`时，我们应该把时间`t`加入队列，并移除队列中所有小于`t-3000`的时间戳，因为它们不再属于最近3000毫秒内的请求。之后，队列的大小即为在时间范围`[t-3000, t]`内的请求数量。

由于我们每次只添加一个元素，并移除不满足条件的元素，所以每个元素只会被加入和移除队列一次，因此时间复杂度是O(1)。队列中最多存储3000毫秒内的请求，空间复杂度取决于这个范围内的请求数量，最坏情况是O(n)，其中n是3000毫秒内可能的最大请求数量。

以下是Python代码实现，包括详细的中文注释：

```python
class RecentCounter:
    def __init__(self):
        self.requests = []  # 初始化一个空队列来存储请求时间

    def ping(self, t: int) -> int:
        # 添加新的请求时间
        self.requests.append(t)
        # 移除不在[t-3000, t]范围内的请求时间
        while self.requests[0] < t - 3000:
            self.requests.pop(0)
        # 队列的长度即为范围内的请求数量
        return len(self.requests)
```

在这段代码中，我们创建了一个RecentCounter类，它有一个名为requests的列表来存储请求时间。每次调用`ping`函数时，我们都会把新的请求时间`t`加入这个列表，并移除所有小于`t-3000`的时间戳。然后返回列表的长度，也就是在时间

# 0649 Dota2 Senate

---

**题目翻译：**

在Dota2的世界里，有两个派系：Radiant（光明）和Dire（黑暗）。

Dota2的参议院由两个派系的参议员组成。现在参议院想要就Dota2游戏中的一个变更进行表决。这个表决是一个基于回合的程序。在每个回合中，每个参议员可以行使以下两项权利之一：

1. 禁止一个参议员的权利：一个参议员可以让另一个参议员在这个和所有随后的回合中失去所有权利。
2. 宣布胜利：如果这个参议员发现仍有投票权的参议员都来自同一个派系，他可以宣布胜利并决定游戏的变更。

给定一个代表每个参议员所属派系的字符串`senate`。字符`'R'`和`'D'`分别代表`Radiant`派系和`Dire`派系。然后如果有`n`个参议员，给定字符串的大小将为`n`。

基于回合的程序从给定顺序的第一个参议员开始到最后一个参议员。这个程序将持续到表决结束。在程序中将跳过所有失去权利的参议员。

假设每个参议员都足够聪明，并且会为他自己的派系采取最佳策略。预测最终哪个派系将宣布胜利并改变Dota2游戏。输出应为`"Radiant"`或`"Dire"`。

示例 1:
输入: senate = "RD"
输出: "Radiant"
解释: 
第一个参议员来自Radiant，他可以在第1轮就禁止下一个参议员的权利。
第二个参议员因为权利被禁止，所以不能行使任何权利了。
在第2轮，第一个参议员可以宣布胜利，因为他是参议院中唯一一个可以投票的人。

示例 2:
输入: senate = "RDD"
输出: "Dire"
解释: 
第一个参议员来自Radiant，他可以在第1轮就禁止下一个参议员的权利。
第二个参议员因为权利被禁止，所以不能行使任何权利了。
第三个参议员来自Dire，他可以在第1轮禁止第一个参议员的权利。
在第2轮，第三个参议员可以宣布胜利，因为他是参议院中唯一一个可以投票的人。

约束条件:
- n == senate.length
- 1 <= n <= 10^4
- senate[i] 是 'R' 或 'D'。

**解题思路和代码实现：**

这道题目可以通过贪心算法来解决。每个参议员都会禁止下一个异派系的参议员的权利，所以我们可以使用一个队列来模拟参议员的投票过程。遍历给定的字符串，将'R'和'D'的索引分别放入两个队列中。每一轮中，我们从两个队列的队首取出索引，比较它们的先后顺序，索引较小的参议员将会禁止索引较大的参议员的权利，并将该参议员的索引加上`n`后重新放入队列，以模拟下一轮的投票过程。这样一来，每轮结束后，至少有一个异派系的参议员会被禁止投票权，直到一个派系的参议员被完全禁止为止。

以下是Python代码实现，包括详细的中文注释：

```python
class Solution:
    def predictPartyVictory(self, senate: str) -> str:
        # 初始化两个队列，分别存储'R'和'D'的索引
        radiant = []


        dire = []

        for i, s in enumerate(senate):
            if s == 'R':
                radiant.append(i)
            else:
                dire.append(i)

        while radiant and dire:
            # 从两个队列的队首取出索引
            r = radiant.pop(0)
            d = dire.pop(0)
            # 比较索引大小，较小的索引表示该参议员行动更快，可以禁止对方的权利
            if r < d:
                # 禁止了'D'的参议员的权利，将'R'的参议员索引加上n后放回队列
                radiant.append(r + len(senate))
            else:
                # 禁止了'R'的参议员的权利，将'D'的参议员索引加上n后放回队列
                dire.append(d + len(senate))

        # 判断哪个队列还有成员，就是哪个派系胜出
        return "Radiant" if radiant else "Dire"
```

在这段代码中，我们创建了一个`Solution`类，它有一个名为`predictPartyVictory`的方法，这个方法接收一个字符串`senate`作为输入，返回一个字符串代表胜出的派系。通过两个队列`radiant`和`dire`来分别存储属于Radiant派系和Dire派系的参议员的索引。在每一轮中，我们比较两个队列队首的索引，索引较小的参议员会禁止另一个参议员的权利，然后将自己的索引加上字符串的长度再放回队列中，以模拟下一轮的投票。

这个解决方案的时间复杂度是O(n)，因为每个参议员只会入队和出队一次。空间复杂度是O(n)，因为需要存储所有参议员的索引。