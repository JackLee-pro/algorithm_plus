# [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

## 题目解析

给定一个链表，判断链表中是否有环。

链表有环的意思是遍历链表到尾节点时，该节点的下一个节点不是 null，而是它前面的某个节点，所以遍历环形链表时，会一直循环下去。

## 思路

我们试着用一个指针来解决，因为环形链表的 next 永远不为 null，所以它可以一直遍历下去，那么我们用一个 Map 或 Set 来保存每个节点并判断是否出现过，如果出现过，说明链表中有环。

这种思路可以实现，但需要额外的存储空间，空间复杂度较高。接着寻找更好的解法。

刚才用了一个指针，一个指针没有相遇这一说，只能是通过计数来判断，那两个指针呢？

两个指针可以一前一后，一快一慢，能相遇说明链表有环，这就是快慢指针的思想。

1、定义一个快指针，一个慢指针，分别指向头节点或者头节点的下一个节点。

2、慢指针每次走一步，快指针每次走两步。

3、当慢指针和快指针相遇时，说明链表有环。

## 代码

    public boolean hasCycle(ListNode head) {
            if (head == null) {
                return false;
            }

            ListNode slowNode = head.next; //慢指针，每次走一步
            if (slowNode == null) {
                return false;
            }

            ListNode fastNode = slowNode.next; //快指针，每次走两步

            while (fastNode != null && slowNode != null) {
                // 当慢指针和快指针相遇时，说明链表有环
                if (slowNode == fastNode) {
                    return true;
                }

                slowNode = slowNode.next;
                fastNode = fastNode.next;
                if (fastNode != null) {
                    fastNode = fastNode.next;
                }
            }

            return false;
    }

## 复杂度分析

时间复杂度为 O(n)，空间复杂度为 O(1)。