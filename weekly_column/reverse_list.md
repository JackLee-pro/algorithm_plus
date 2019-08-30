# 链表
链表是一种线性结构，只是它用指针来表示结点之间“除首元素外其余元素都有唯一前驱结点，除尾元素外其余元素都有唯一后继结点”这种逻辑关系，同时，因为链表的这种实现机制导致链表不具有随机访问性，要访问链表中的任意元素都必须使用顺序访问（就是每一次都从头结点开始遍历），所以链表访问的时间复杂度是O(n)。

# 链表的实现
## C++版单链表


# 链表相关的算法题
## 反转链表
[反转链表](https://leetcode.com/problems/reverse-linked-list/)

这个题目起初是我想的太复杂了，不应该去考虑头结点的问题，而是应该按照输入建模，认为此链表就是一个没有头结点的链表。以本题的输入（1->2->3->4->5->NULL）为例，这个链表的样子是：

| 1 | next指向2 | 2 |next 指向3 | 3 | next 指向 4 | 4 | next 指向5 | 5 |next（指针域）为空，表示结尾|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

要达到反转链表的目的，只需要改变每一个结点指针域指针的指向即可，这里特殊结点是首节点（请务必区分头结点和首节点，首节点是链表真正的第一个有数据的结点，而头结点则不存储数据，其指针域指向首节点），反转后，头节点要变成尾结点，尾结点的特别之处是其指针域为空。对于中间结点，算法是一致的：改变指针域指针的指向，原本单链表的结点的指针域是指向该结点的直接后继结点，要达到反转的目的则需要它的指针域指向其直接前驱结点。例如数据元素是2的结点，其直接后继结点是数据为3的结点，直接前驱结点是数据为1的结点，原本它的next指针域指向的是3，现在要把它变为指向1。对除首节点外每一个结点执行改变指针域指向的操作，即可得到如下结果：

| 1 | next为nullptr | 2 |next 指向1 | 3 | next 指向 2 | 4 | next 指向3 | 5 |next指向4|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

但是单链表的问题是，单独拿出来一个结点，我们只知道它的直接后继结点是什么，而不能知道它的直接前驱结点是哪一个，但对于这道题目来说，这个信息也就够了，我们从首节点出发开始遍历每一个结点，当前结点就是其后继结点的前驱结点。

代码实现：

C++实现：
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

//题目的意思是反转一个单链表
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *pre = nullptr;    //保存某个结点的前驱结点的地址
        ListNode *currentNode = head;   //当前正在处理的结点
        ListNode *temp = nullptr;       //这个是做指针指向改变时临时保存
        while(currentNode != nullptr)   //判断是否到达了链表的尾结点
        {
            temp = currentNode->next;   //获得当前结点的直接后继结点
            currentNode->next = pre;    //令当前结点的指针域指向保存下的该结点的前驱结点
            pre = currentNode;
            currentNode = temp;         //跳转到下一个结点
        }
        return pre;     //返回的是pre而不是currentNode，因为遍历到链表结束的时候currentNode是nullptr
    }
};
```

Java代码：
```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode currentNode = head;
        ListNode pre = null;
        ListNode temp = null;
        while(currentNode != null)
        {
            temp = currentNode.next;
            currentNode.next = pre;
            pre = currentNode;
            currentNode = temp;
        }
        return pre;
    }
}
```
Python3解法：
```Python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        currentNode = head
        pre = None
        temp = None
        while currentNode != None :
            temp = currentNode.next
            currentNode.next = pre
            pre = currentNode
            currentNode = temp
        return pre
```

上面三种语言的解答方法其实本质上都是利用链表的存储结构来实现的，其时间复杂度是O(n)，空间复杂度是O(1)。更加详细的介绍请看C++版本的注释。
