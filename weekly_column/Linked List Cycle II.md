
# [142. 环形链表II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

## 题前思考
* 如果你已经学会【快慢指针】这种思想，那么验证一个链表是否成环相比，想必就得心应手了；

* 当你解决了[141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)，是否有思考过快慢指针最终相遇在哪个点？

* 在现实中如果你不确定自己是否在绕圈，我们可以在所有经过的地方做上记号，之后第一次遇见记号的地方就是圈的起点了，可是这种做法抽象成编程思维，显然需要额外的空间，比如一个额外的HashMap，有没有不需要额外空间的做法呢？

## 解法
我们先提前给出做法，可能你看完解法之后仍然一头雾水，甚至质疑这个解法的有效性，这也正是这道题的魅力所在：
```
public ListNode detectCycle(ListNode mHead) {
           ListNode slow = mHead, quick = mHead;
            while (quick != null && quick.next != null && quick.next.next != null) {
                slow = slow.next;
                quick = quick.next.next;
                if (slow == quick) {
                    ListNode p1 = mHead, p2 = slow;
                    while (p1 != p2) {
                        p1 = p1.next;
                        p2 = p2.next;
                    }
                    return p1;
                }
            }
            return null;
        }
```
解法分成两步：
* 第一步我们像解[141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)那样，快慢指针找到相遇的点（我们假设这个点是P）；
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19a547d7ddf0?w=760&h=420&f=png&s=44291)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19b69531dcad?w=760&h=420&f=png&s=44491)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19c723d72263?w=760&h=420&f=png&s=44466)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19d3bb822925?w=760&h=420&f=png&s=44543)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19db3a7bd884?w=760&h=420&f=png&s=44587)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19e34162e5b4?w=760&h=420&f=png&s=46761)
 
* 第二步，让两个指针分别从【mHead，P】出发，一次走一步，它们第一次相遇的地方，就是圆的入口，也就是我们要求的结果。
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19f1cbe6b2bf?w=760&h=419&f=png&s=42255)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd19ff4acd5aa5?w=760&h=420&f=png&s=43032)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd1a06d7d2186e?w=760&h=420&f=png&s=44781)
![](https://user-gold-cdn.xitu.io/2019/8/27/16cd1a0f3df4835a?w=760&h=420&f=png&s=42355)

## 为什么
* 上图展示了完整的过程，但是你看完却不免有一种担心——这该不会是凑巧吧？

* 事实上你自己多画几次图示，不难发现——不是的，这是必然的，不是偶然的。

* 然后你就会一种疑惑——这是为什么啊？

**文字论述一下：**

假设相遇的点是  **P**  ，当  **慢指针slow**  走到P的时候 ，走了  **K**  步， **快指针quick**  走了  **2*K**  步（因为慢指针每走一步，快指针就走两步），当  **快指针quick**  先于  **慢指针slow**  第一次到达  **P**  的时候（不一定正好在那，由于一次走两步，可能直接跨过去，但是这里不影响分析），它还剩下  **K**  步没走，当  **快指针quick**  走了  **2*K**  的时候，它还在  **P**  那里，说明其中  **K**  步使得  **快指针quick**  以  **P**  为起点一直在转圈圈，也就是说  **K**  步正好是【环形部分周长】的整数倍。

接下来我们让两个指针同时从head和p出发，一次走一步，经过K步之后，它们必然又会相遇在P点，既然大家都是一次走一步，P能相遇，P-1也可以，向前类推，第一次相遇的地方就是第一次交汇的地方，也就是我们要求的答案了。

**画图代数算一下：**

![](https://raw.githubusercontent.com/JackLee-pro/algorithm_plus/master/pictures/hxlb2.png)
