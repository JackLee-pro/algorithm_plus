# 反转链表
# 反转链表II
# 环形链表
# [环形链表Ⅱ](https://github.com/JackLee-pro/algorithm_plus/wiki/%E9%93%BE%E8%A1%A8%E4%B8%93%E9%A2%98%EF%BC%88%E7%AC%AC%E4%B8%80%E6%9C%9F%EF%BC%89%E2%80%94%E2%80%94%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II)
# [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists)

## 题目解析：
题目中给出两个链表的头节点，需要寻找两个链表是否有「交点」并且反回这个「交点」。
> 所谓「交点」就是两个链表在其某个节点之后同时指向了同一个节点对象「而不是同一个数值节点，两个相同数值的节点并不是同一个对象，如**示例 1**中的两个①并不是同一个对象，虽然他们的数值相同，两个①之后的⑧才是相交的地方」，换句话说，他们“合并”了，两个不同的 Node 的 next 指向了相同的 Node,找到这个相同的 Node 返回即可。

测试用例中给出的输入有些同学看不懂，我在这里解释一下：
![](http://img.artaris.cn/15668333954866.jpg)

**示例 1**
```
8
[4,1,8,4,5]
[5,0,1,8,4,5]
2
3
```
第一个 8,代表两个链表总共有八个节点「咦？这不是总共输入了 5 + 6 = 11 个节点么？因为两个链表后面的节点是重复的」，随后输入了这两个链表，后面的两个数代表他们相交的节点的序号「从0开始」，第一个2即代表了第一个链表的第三个节点即⑧；第二个3即代表了第二个链表的第四个节点即也是⑧；
用这五个值可以唯一确定两个链表的的相交状况。「不信你可以动手画画试试还有没有其他情况。我不要我觉得，我要你觉得」

---

## 思路一
从对象重复的角度考虑，如果两个链表的某个节点是同一个对象「而不是链表的值相等」，说明这两个链表此刻指向了同一个对象，则它们相交了。
这时，可以考虑使用集合 Set 的方式来存放第一个链表的所有节点，然后再顺序遍历第二个链表，判断第二个链表的节点是否存在于 Set 中，如果存在，说明这个点就是那个重复点。

```Java
public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    Set<ListNode> result = new HashSet<>();
    while (headA != null){
        result.add(headA);
        headA = headA.next;
    }
    while (headB != null){
        if(result.contains(headB)){
            return headB;
        }
        headB = headB.next;
    }
    return null;
}
```

此方法并不是最优解，因为 Set 要处理碰撞，时间消耗很大。

## 思路二
能不能让两个链表自然向后遍历，找到那个相交点呢？
从头开始是不行的，因为两个链表的长度是不一样的，直接从头遍历我们不知道它们会在何处相遇。
思考相等的特性，如果两个链表从某处相交了，那么从此处之后它们就完全一样了，包括剩余长度和后面的对象。
那么把两个链表尾部对齐，以从尾到头的倒序走向考虑，如果有交点的话两个链表末尾的一些节点肯定是相同的，然后从某处开始不等，然后短的那个链表到头部位置，然后长的链表到头部的位置。
这样的摆放方式，两个相交链表最早的相交点也只可能是短的链表的头部位置「此时短链表是长链表的一部分」，故可以以这种思路，先将其尾部对齐，长的链表走两个链表相差的步数，然后两个链表同时开始继续往后走，边走边判断链表的节点是否相等，直到最后一个节点。

**归纳我们的方法：**
1. 计算两个链表的长度
2. 确定两个链表的长度的差值
3. 让较长的那个链表开始向后走差值的步数
4. 让短的链表从头、长度的链表从差值所在位置，同时开始向后走，边走边判断节点相等
5. 找到想等的节点或走到两个链表的尾部

```Java
public static ListNode getIntersectionNode1(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) return null;
    int lengthA = 0, lengthB = 0;
    ListNode curA = headA, curB = headB;
    while (curA != null) {
        lengthA++;
        curA = curA.next;
    }
    while (curB != null) {
        lengthB++;
        curB = curB.next;
    }
    ListNode first = headA, second = headB;
    int step = Math.abs(lengthA - lengthB);
    if (lengthA > lengthB) {
        for (int i = 0; i < step; i++) {
            first = first.next;
        }
    } else {
        for (int i = 0; i < step; i++) {
            second = second.next;
        }
    }
    while (first != second) {
        first = first.next;
        second = second.next;
    }
    return first;
}
```
**复杂度分析**
时间复杂度O(n)
空间复杂度O(1)
