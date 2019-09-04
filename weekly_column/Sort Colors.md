# [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors)

题目中给出一个包含0、1、2的乱序数组要求我们进行排序。
通过下面的提示来思考解答
**一个直观的解决方案是使用计数排序的两趟扫描算法。
首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。**

此方法教我们通过一次遍历，分别统计出0、1、2的个数，然后通过对原数组进行赋值，就得到了排好序的数组。这种方法被称作**计数排序**
> ## 计数排序
> 1. 通过一次遍历，得到数组中的最大值 max 和最小值 min
> 2. 使用 {max - min} 得到的数申请额外数组空间，其中的每一位将记录原数组元素出现的次数
> 3. 再次遍历原数组，统计原数组中元素出现的次数。「如原数组出现了 n ,则将额外数组空间的第 (n - min) 位 + 1 代表其出现次数多了一位」
> 4. 遍历额外空间数组，从小到大对原数组重新赋值
> 可以预见，如果待排序数组的最大值和最小值的差距很大，会很耗费空间。

**计数排序法解答**
```Java
public void sortColors(int[] nums) {
    int[] times = new int[3];
    for (int num : nums) {
        times[num]++;
    }
    int index = 0;
    for (int i = 0; i < times.length; i++) {
        while (times[i] > 0) {
            nums[index] = i;
            times[i] --;
            index++;
        }
    }
}
```
因为此题中我们已经知道取值范围是0-2，所以我直接申请了大小为3的数组用排序。
这种方法遍历了两次。

--- 

下面让我们探讨一下题目中的第二个提示：
**你能想出一个仅使用常数空间的一趟扫描算法吗？**

题目的要求很简单
1. 0 放首位
2. 2 放末尾
3. 1 放中间

我们可以使用类似快排的交换首尾位置不对的元素，逐步向内缩减范围。

定义两个指针，left`👆` 和 right`👇`
数组开始状态，left 指向开始的位置而 right 指向末尾
```
                        👇
 2   0   1   0   2   1   0
👆
```
left 和 right 都不满足题目要求1、2，交换他们的位置
```
                        👇
 0   0   1   0   2   1   2
👆
```
分别向后移动 left 和 right ，找到他们下一个不满足1、2的位置，换句话说，经过这次遍历，`left = right = 1`
```
                    👇
 0   0   1   0   2   1   2
        👆
```
此时我们两指中间的数据是没有拍好序的，我们再定义一个数 current`👇🏻` = left + 1，判断他的值是否小于当前 left 的值「即current = 0」亦或大于当前 right 的值「即current = 2」,找到这种数，与相应的 left 或 right 交换，使算法继续回到移动 left 和 right
```
            👇🏻      👇
 0   0   1   0   2   1   2
        👆
```
交换 left 与 current
```
            👇🏻      👇
 0   0   0   1   2   1   2
        👆
```
继续找到 left 和 right 不满足条件 1、2的位置
```
                    👇
 0   0   0   1   2   1   2
            👆
```
继续定义 current 
```
                👇🏻  👇
 0   0   0   1   2   1   2
            👆
```
交换 right 与 current
```
                👇🏻  👇
 0   0   0   1   1   2   2
            👆
```
继续循环找到 left 和 right 不满足条件 1、2的位置
```
                👇
 0   0   0   1   1   2   2
            👆
```
current >= right 循环结束

付诸代码如下：

```Java
public static void sortColors(int[] nums) {
    int left = 0, right = nums.length - 1;
    tag:
    while (left < right) {
        while (left < nums.length && nums[left] == 0) {
            left++;
        }
        while (right > 0 && nums[right] == 2) {
            right--;
        }
        if (right < 0 || left >= nums.length || left >= right) {
            break;
        }
        if (nums[left] > nums[right]) {
            swopNum(nums, left, right);
        } else if (nums[left] == nums[right]) {
            int current = left + 1;
            while (current < right) {
                if (nums[current] < nums[left]) {
                    swopNum(nums, left, current);
                    continue tag;
                } else if (nums[right] < nums[current]) {
                    swopNum(nums, current, right);
                    continue tag;
                }
                current++;
            }
            return;
        }
    }
}

private static void swopNum(int[] nums, int i, int j) {
    nums[i] = nums[i] + nums[j];
    nums[j] = nums[i] - nums[j];
    nums[j] = nums[i] - nums[j];
}
```
用到了两个小技巧：
- 对 for/while 循环使用 label 
    可以对 for/while 循环命名。在多层循环中，可以在内层循环中指定 break/continue 指定循环
- 两数交换
    交换两个数 a 和 b 的值，可以使用如下代码：
    ```
    a = a + b;
    b = a - b;
    a = a - b;
    ```
    这里没有使用多余的临时空间来交换，具体的算法的交换的精妙之处留待你自行思考。此外，用如下位运算也可以实现两数交换。
    
```
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
```
