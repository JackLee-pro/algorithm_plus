# 题意解析
题目原话：
> 给定由一些正数（代表长度）组成的数组A，返回由其中三个长度组成的、面积不为零的三角形的最大周长。如果不能形成任何面积不为零的三角形，返回 0。

可以翻译为：

**在满足三角形定义的条件下尽可能找到使得周长最大的三条边组合，并返回这个周长；**

而三角形的定义：

**任意两边和大于第三边；**

假设三条边分别为a、b、c，则有(a+b>c) && (a+c>b) && (b+c>a)

可以看到条件还是稍显复杂，联想一下这道题其中一个标签是【排序】，如果我们知道了a,b,c之间的大小关系，比如a<b<c,那么条件就可以简化为a+b>c，即：

**短边和大于第三边；**

# 思路一

至此，我们就可以得到第一种解法，先将数组排序，然后从大到小，依次取三数一组，当其满足a+b>c时，其就是满足题意的最大周长了;

我们借助Java的`Arrays.sort()`进行时间复杂度为O(nlogn)的排序，然后倒序依次取三数校验：
```
    /**
     * 方案一：先排序后筛选 [LeetCode：22ms]
     */
    class Solution1 {
        public int largestPerimeter(int[] A) {
            Arrays.sort(A);
            for (int i = A.length - 1; i >= 2; i--) {
                if (A[i] < A[i - 1] + A[i - 2])
                    return A[i] + A[i - 1] + A[i - 2];
            }
            return 0;
        }
    }
```

# 思路二

上面的代码看似很简洁，但是在leetcode的排名却不高，肯定还有优化空间！

我们看到`return`的位置，除了末尾的`return 0;`之外，中间的某个时刻也有可能直接返回，而排序是针对整个数组的，假设我们在**第三组**的时候就得到我们想要的结果了，那么其实我们只需要找出最大的**五个数**，之后数组其它元素的顺序我们并不关心。

但是，我们不清楚第几组是我们要求的结果，所以最好就是我们**每求出一个最大值**，就尝试校验它是否满足返回条件，如果不满足则继续求剩下元素的最大值；

画重点：**每求出一个最大值**

什么样的排序算法可以满足每次都求出最大值呢？

**选择排序**、**冒泡排序**、**堆排序**...（欢迎补充）

冒泡排序，需要频繁交换数据，被我首先排除了。

接着试着用**选择排序**、**堆排序**来实现一下，每次求出最大值，在满足已有三个的情况下尝试校验返回：
```

    /**
     * 方案二：选择排序，提前筛选 [LeetCode:14ms]
     */
    class Solution2 {
        public int largestPerimeter(int[] A) {
            for (int i = 0; i < A.length; i++) {
                int minP = i;
                for (int j = i + 1; j < A.length; j++) {
                    if (A[j] > A[minP]) {
                        minP = j;
                    }
                }
                int temp = A[i];
                A[i] = A[minP];
                A[minP] = temp;
                if (i >= 2 && A[i - 2] < A[i - 1] + A[i])
                    return A[i] + A[i - 1] + A[i - 2];
            }
            return 0;
        }
    }

    /**
     * 方案三：大顶堆，提前筛选 [LeetCode:8ms]
     */
    class Solution3 {
        /**
         * 大顶堆
         *
         * @param A
         */
        public int largestPerimeter(int[] A) {
            buildHeap(A);
            for (int i = A.length - 1; i >= 0; i--) {
                swap(A, 0, i);
                adjust(A, i, 0);
                if (i < A.length - 2 && A[i + 2] < A[i + 1] + A[i]) {
                    return A[i + 2] + A[i + 1] + A[i];
                }
            }
            return 0;
        }

        /**
         * 调整
         *
         * @param nums
         * @param length
         * @param x
         */
        public void adjust(int[] nums, int length, int x) {
            while (x <= length / 2 - 1) {
                int left = nums[2 * x + 1];
                int right = left - 1;
                if (2 * x + 2 < length) {
                    right = nums[2 * x + 2];
                }
                int max = Math.max(left, right);
                if (nums[x] >= max)
                    break;
                if (max == left) {
                    int temp = nums[x];
                    nums[x] = left;
                    nums[2 * x + 1] = temp;
                    x = 2 * x + 1;
                } else {
                    int temp = nums[x];
                    nums[x] = right;
                    nums[2 * x + 2] = temp;
                    x = 2 * x + 2;
                }
            }
        }


        /**
         * 交换两个数
         *
         * @param nums
         * @param i
         * @param j
         */
        public void swap(int[] nums, int i, int j) {
            if (i == j)
                return;
            nums[i] = nums[i] ^ nums[j];
            nums[j] = nums[i] ^ nums[j];
            nums[i] = nums[i] ^ nums[j];
        }

        /**
         * 建堆
         *
         * @param nums
         */
        public void buildHeap(int[] nums) {
            int length = nums.length;
            for (int i = length / 2 - 1; i >= 0; i--) {
                adjust(nums, nums.length, i);
            }
        }
    }
```
从运行结果看，**堆排序**是目为止的最优解，如果你还有更好的解法，欢迎补充~
