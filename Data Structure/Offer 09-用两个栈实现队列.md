# Offer 09 : 用两个栈实现队列

### 题目链接

[力扣](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

Offer 09\. 用两个栈实现队列 (Easy)

* 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

示例1:

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

示例2:

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

### 算法

* 加入队尾 appendTail()函数： 将数字 val 加入栈  即可。
* 删除队首deleteHead()函数： 有以下三种情况。
    1. 当栈 stack2 不为空： B中仍有已完成倒序的元素，因此直接返回 stack2 的栈顶元素。
    2. 否则，当 stack1 为空： 即两个栈都为空，无元素，因此返回 -1。
    3. 否则： 将栈 stack1 元素全部转移至栈 stack2 中，实现元素倒序，并返回栈 stack2 的栈顶元素。

```python
class CQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def appendTail(self, value: int) -> None:
        self.stack1.append(value)

    def deleteHead(self) -> int:
        if not self.stack2:
            if not self.stack1:
                return -1
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()
```
