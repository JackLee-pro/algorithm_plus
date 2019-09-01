# 反转链表 II
[相关Issue](https://github.com/JackLee-pro/algorithm_plus/issues/18)

## 题目解析
这个题是[反转链表]()的升级版，升级的地方在于，需要考虑各种状态。假设链表的长度为l，要反转从[m,n]之间的链表，要考虑，
* 情况一，m=0,n<0；
* 情况二，m>0,n=l;
* 情况三，m>0,n<l;
* 情况四，m=0,n=l

针对于情况四，就是[反转链表]()这道题了

## 思路
对于这种情况复杂的链表问题，如果强行把这四种情况融合到一个算法中来做，肯定是能做出来，但是增加了条件判断的复杂性；
针对这种问题可以采用“哨兵节点”来简化代码逻辑，也就是说，创建一个新的ListNode作为哨兵节点，对当前链表进行遍历并反转
[m,n]的部分，然后维护ListNode的当前指针并添加到尾部，这样对原链表遍历完成后，直接返回哨兵节点的下一个节点，就是题目
需要返回的结果了

## 代码
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (m==n)return head;
        ListNode res = new ListNode(-1);
        ListNode currRes = res;
        ListNode curr = head;
        ListNode mNode = null;
        ListNode pre = null;
        int i = 1;
        while(curr!=null){
            ListNode tmp = curr.next;
            if(i<m){ // m范围之前,直接添加到哨兵节点后，并记录最后一个添加的节点
                currRes.next = curr; //直接放到新链表尾部
                currRes = curr; //更新新链表的指针
            }else if(i>=m && i<=n){ // [m,n]
                if(i==m){ //进入需要反转的范围，记录他的前一个指针，等反转的范围结束后，把它作为链表的尾部
                    mNode = curr;
                    curr.next = pre;
                    pre = curr;
                }else if(i==n){ //反转的最后一个节点，把它放到新链表的尾部
                    curr.next = pre;
                    currRes.next = curr;
                    currRes = mNode;
                }else{ //中间范围，更新next指针来反转
                    curr.next = pre;
                    pre = curr;
                }
            }else{ // n 范围之后
                currRes.next = curr;
                currRes = curr;
            }
            i++;
            curr = tmp;
        }
        return res.next;
    }
}
```

## 复杂度分析
时间复杂度: O(n)
空间复杂度: O(1)

