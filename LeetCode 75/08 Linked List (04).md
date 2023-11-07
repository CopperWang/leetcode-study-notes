# 0000 Linked List

---

在计算机科学中，链表（Linked List）是一种线性数据结构，其中的元素（称为节点）不必在内存中连续存储。每个节点都包含两部分：

- 一是存储数据的值，
- 二是指向列表中下一个节点的指针（或引用）。

在双向链表中，每个节点还包含指向前一个节点的指针。

链表在以下情况下非常有用：

1. **动态大小**：与数组不同，链表的大小可以在运行时动态增长或缩减，无需预先指定大小。

2. **插入和删除操作频繁**：在链表中插入或删除节点可以在 O(1) 的时间内完成（如果已经有了节点的引用），相对于数组或动态数组的 `O(n)` 时间复杂度要快得多。

3. **内存利用**：由于链表不需要连续内存空间，它可以有效地利用分散在内存中的零碎空间。

4. **实现其他数据结构**：链表常被用来实现栈、队列和其他复杂的数据结构。

然而，链表也有一些缺点：

- **随机访问慢**：相比于数组，链表难以快速访问特定索引的元素，通常需要从头开始遍历链表直到所需的位置。

- **内存开销**：每个节点额外需要存储指针信息，这意味着链表的内存占用相对较高。

- **缓存不友好**：链表的元素分布在内存中的不同位置，可能会导致较差的缓存性能，进而影响整体性能。

在LeetCode等算法题目中，链表问题经常会要求考虑这些操作特性，并利用链表的动态和指针特性解决问题。例如，反转链表、检测链表中的环、合并两个排序的链表等，都是常见的链表相关题目。

以下是一个简单的链表节点定义和使用示例（在LeetCode中经常见到的形式）：

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# 创建链表 1 -> 2 -> 3
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)

# 遍历链表
current = head
while current:
    print(current.val)
    current = current.next
```

在这个例子中，我们定义了一个 `ListNode` 类来表示链表的节点，并创建了一个简单的三元素链表。我们还演示了如何遍历链表并打印每个节点的值。在链表的实际应用中，你需要熟悉指针操作，因为它们对于链表问题至关重要。

# 2095 Delete the Middle Node of a Linked List

---

**题目翻译：**

给定一个链表的头节点，删除链表的中间节点，并返回修改后的链表的头节点。

链表大小为`n`的中间节点是从开始使用`0`为基础索引的`⌊n / 2⌋`个节点，这里`⌊x⌋`表示小于或等于`x`的最大整数。

对于n = 1, 2, 3, 4, 和 5，中间节点分别是 0, 1, 1, 2, 和 2。

示例 1:
输入：head = [1,3,4,7,1,2,6]
输出：[1,3,4,1,2,6]
解释：
上图表示给定的链表。节点的索引写在下面。
由于 n = 7，节点3的值为7是中间节点，它在图中用红色标记。
删除这个节点后，我们返回新的列表。

示例 2:
输入：head = [1,2,3,4]
输出：[1,2,4]
解释：
上图表示给定的链表。
对于 n = 4，节点2的值为3是中间节点，它在图中用红色标记。

示例 3:
输入：head = [2,1]
输出：[2]
解释：
上图表示给定的链表。
对于 n = 2，节点1的值为1是中间节点，它在图中用红色标记。
删除节点1后，节点0的值为2是唯一剩下的节点。

约束条件：
- 链表中节点的数量在`[1, 10^5]`范围内。
- `1 <= Node.val <= 10^5`

**解题思路和代码实现：**

这道题目可以通过快慢指针的方法来解决。快指针一次走两步，慢指针一次走一步，当快指针走到尽头时，慢指针就会指向链表的中间节点。之后我们只需要调整慢指针前一个节点的next指针，使其跳过慢指针指向的节点即可删除中间节点。

以下是Python代码实现，包括详细的中文注释：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # 如果链表为空或只有一个节点，删除中间节点后链表为空
        if not head or not head.next:
            return None
        
        # 初始化快慢指针，都指向头节点
        slow = head
        fast = head
        # 初始化一个前继节点指针，用于之后删除中间节点
        prev = None
        
        # 快指针一次走两步，慢指针一次走一步
        while fast and fast.next:
            fast = fast.next.next
            prev = slow
            slow = slow.next
        
        # 慢指针指向的即为中间节点，将前继节点的next指向慢指针的next，删除中间节点
        prev.next = slow.next
        
        return head
```

在这段代码中，`ListNode`是链表节点的定义，`Solution`类包含一个名为`deleteMiddle`的方法，这个方法接收一个可能为空的`ListNode`类型的头节点`head`作为输入，返回一个可能为空的`ListNode`类型的头节点。我们使用两个指针`slow`和`fast`来找到中间节点，`prev`用来记录`slow`的前一个节点。当`fast`指针无法继续前进时，`slow`就指向了中间节点

# 0328 Odd Even Linked List

---

**题目翻译：**

给定一个单链表的头节点`head`，请将所有奇数位置的节点放在一起，然后是所有偶数位置的节点，并返回重排后的列表。

第一个节点被认为是奇数位置，第二个节点是偶数位置，依此类推。

请注意，在奇数组和偶数组内部，节点的相对顺序应该保持输入时的顺序。

你必须在`O(1)`额外空间复杂度和`O(n)`时间复杂度的条件下解决问题。

示例 1:
输入: head = [1,2,3,4,5]
输出: [1,3,5,2,4]

示例 2:
输入: head = [2,1,3,5,6,4,7]
输出: [2,3,6,7,1,5,4]

约束条件：
- 链表中节点的数量在`[0, 10^4]`的范围内。
- `-10^6 <= Node.val <= 10^6`

**解题思路和代码实现：**

这道题目的关键是将链表的节点根据其位置的奇偶性分开，同时不能改变各自奇数位置节点和偶数位置节点内部的相对顺序，并且要求空间复杂度为`O(1)`、时间复杂度为`O(n)`。

我们可以使用两个指针，一个用来链接所有奇数位置的节点，另一个用来链接所有偶数位置的节点。遍历链表一次，将两种节点分别链接起来，最后将偶数位置节点链接到奇数位置节点的尾部。

以下是Python代码实现，包括详细的中文注释：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head
        
        # odd用来链接奇数位置的节点，even用来链接偶数位置的节点
        # evenHead用来保存偶数位置链表的头节点，以便后续将其链接到奇数链表后面
        odd = head
        even = head.next
        evenHead = even
        
        # 遍历链表，注意要检查even和even.next是否为None
        while even and even.next:
            odd.next = even.next
            odd = odd.next
            even.next = odd.next
            even = even.next
        
        # 将偶数位置链表链接到奇数位置链表的尾部
        odd.next = evenHead
        return head

# 使用示例
# sol = Solution()
# head = 构造链表[1,2,3,4,5]
# print(sol.oddEvenList(head))  # 应输出构造的链表[1,3,5,2,4]的头节点
```

在这段代码中，`ListNode`是链表节点的定义，`Solution`类包含一个名为`oddEvenList`的方法，这个方法接收一个可能为空的`ListNode`类型的头节点`head`作为输入，返回一个可能为空的`ListNode`类型的头节点。我们首先初始化两个指针`odd`和`even`来分别处理奇数位置和偶数位置的节点，然后通过一次遍历将节点分类，并在遍历结束后将偶数链表链接到奇数链表的末尾。这样就在O(n)的时间复杂度和O(1)的空间复杂度内完成了链表的重排。

# 0206 Reverse Linked List

---

**题目翻译：**

给定一个单链表的头节点`head`，请反转该链表，并返回反转后的链表的头节点。

示例 1:
输入: head = [1,2,3,4,5]
输出: [5,4,3,2,1]

示例 2:
输入: head = [1,2]
输出: [2,1]

示例 3:
输入: head = []
输出: []

约束条件：
- 链表中节点的数量范围是 [0, 5000]。
- 节点值的范围是 [-5000, 5000]。

进阶：链表可以通过迭代或递归的方式来反转。你能实现它们吗？

**解题思路和代码实现：**

这道题目要求我们反转一个单链表。我们可以通过迭代或递归的方式来实现。这里我们先介绍迭代的方法，因为它的空间复杂度较低。

迭代方法的基本思想是，遍历链表，逐个节点地改变节点的`next`指针的方向。我们需要一个额外的指针来保存先前节点的信息，以便我们可以改变当前节点的`next`指针。

以下是迭代方法的Python代码实现，包括详细的中文注释：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # 初始化三个指针，prev指向已经反转部分的最后一个节点，初始化为None
        # curr指向当前处理的节点，初始化为head
        # next用来暂存curr的下一个节点
        prev = None
        curr = head
        
        # 遍历链表
        while curr:
            next_temp = curr.next  # 暂存当前节点的下一个节点
            curr.next = prev       # 反转当前节点的指针方向
            prev = curr            # prev移动到当前节点
            curr = next_temp       # curr移动到下一个节点
        
        # 最后prev将会指向新的头节点
        return prev
```

这段代码中，`ListNode`是链表节点的定义，`Solution`类包含一个名为`reverseList`的方法，这个方法接收一个可能为空的`ListNode`类型的头节点`head`作为输入，返回一个可能为空的`ListNode`类型的头节点。我们使用三个指针`prev`、`curr`和`next_temp`来反转链表，最终`prev`将指向反转后的头节点。

该方法的时间复杂度是O(n)，因为我们需要遍历一遍链表。空间复杂度是O(1)，因为我们只用了固定数量的额外空间。

对于递归方法，递归的过程本质上是一个栈，因此空间复杂度会是O(n)，这里不再赘述。

# 2130 Maximum Twin Sum of a Linked List

---

**题目翻译：**

在一个大小为`n`（`n`为偶数）的链表中，如果`0 <= i <= (n / 2) - 1`，那么链表的第i个节点（从0开始索引）被认为是第(n-1-i)个节点的孪生节点。

例如，如果n = 4，那么节点0是节点3的孪生节点，节点1是节点2的孪生节点。这些是n = 4时唯一具有孪生节点的节点。
孪生和被定义为一个节点及其孪生节点的值之和。

给定一个偶数长度链表的头节点head，请返回该链表的最大孪生和。

示例 1:
输入: head = [5,4,2,1]
输出: 6
解释:
节点0和节点1分别是节点3和节点2的孪生节点，所有的孪生和都是6。
链表中没有其他具有孪生节点的节点。
因此，链表的最大孪生和是6。

示例 2:
输入: head = [4,2,2,3]
输出: 7
解释:
该链表中存在具有孪生关系的节点是：
- 节点0是节点3的孪生节点，孪生和为4 + 3 = 7。
- 节点1是节点2的孪生节点，孪生和为2 + 2 = 4。
因此，链表的最大孪生和是max(7, 4) = 7。

示例 3:
输入: head = [1,100000]
输出: 100001
解释:
链表中只有一个孪生节点，其孪生和为1 + 100000 = 100001。

约束条件：
- 链表中节点的数目是范围 [2, 10^5] 内的偶数。
- 1 <= Node.val <= 10^5

**解题思路和代码实现：**

要解决这个问题，我们可以将链表分成两部分，然后将后半部分反转，最后遍历前半部分和反转后的后半部分，计算最大孪生和。

以下是具体的Python代码实现，包括详细的中文注释：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        # 快慢指针找到中点
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        # 反转后半部分链表
        prev = None
        while slow:
            next_temp = slow.next
            slow.next = prev
            prev = slow
            slow = next_temp
        
        # 计算最大孪生和
        max_twin_sum = 0
        while prev: # 此时prev指向反转后的后半部分链表的头节点
            max_twin_sum = max(max_twin_sum, head.val + prev.val)
            head = head.next
            prev = prev.next
        
        return max_twin_sum
```

在这段代码中，我们首先使用快慢指针找到链表的中点，然后反转中点之后的链表部分，最后同步遍历链表的前半部分和反转后的后半部分，计算出每对孪生节点的和，取最大值即为结果。

该方法的时间复杂度是O(n)，因为我们只遍历了链表两次。空间复杂度是O(1)，因为我们只使用了有限的额外空间。