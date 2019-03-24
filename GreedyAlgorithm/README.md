

最好多举一些例子。

## 1. 分发饼干 -- assign_cookies

- 来源：[leetcode 455](https://leetcode-cn.com/problems/assign-cookies/)

- 题目：已知一些孩子和糖果，每个孩子有需求因子g，每个糖果有大小s，当某个糖果的大小s >= 某个孩子的需求因子g时，代表该糖果可以满足该孩子；求使用这些糖果，最多能满足多少孩子。
- 要求：某个孩子最多只能用1个糖果满足
- 举例：需求因子：g= [5,10,2,9,15,9]; 糖果大小s = [6,1,20,3,8]； 最多可以满足3个孩子

### 分析

1. 某个糖果如果不能满足某个孩子，则该谈过也一定不能满足需求因子更大的孩子
2. 某个孩子可以用更小的糖果满足，则没必要用更大的糖果满足，因为可以保留更大的糖果满足需求因子更大的孩子（贪心）
3. 需求因子小的孩子更容易被满足，故优先从需求因子小的孩子尝试，可以得到正确的结果

### 思路

1. 对需求因子g与糖果大小s进行从小到大排序
2. 按照从小到大的顺序使用糖果测试是否满足某个孩子；每个糖果只尝试一次；若成功，则换一个孩子；直到没孩子或没糖果


## 2. 摆动序列 -- wiggle_sequence

- **来源：[leetcode 376](https://leetcode-cn.com/problems/wiggle-subsequence/)**

- 题目： 一个整数序列，如果两个相邻元素的差恰好正负（负整）交替出现，则该序列被称为摇摆序列。 一个小于2个元素的序列直接为摇摆序列。给定一个整数序列，返回作为摆动序列的最长子序列的长度。可以删除原始序列中的一些元素来获得子序列，剩下的元素保持其原始顺序。

- 举例： [1,7,4,9,2,5]， 相邻元素差[6,-3,5, -7,3]， 该序列为摇摆序列
  [1,17,5,10,13,15,10,5,16,8]， 其最终的子序列为[1,17,10,13,10,16,8]，子序列长度为7

### 贪心规律
当序列有一段连续的递增（或递减）时，为形成摇摆子序列，我们只需要保留这段连续的递增（或递减）的首尾元素，这样更可能使得尾部的后一个元素称为摇摆子序列的下一个元素。 简单来说，就是只选择转折点的元素。

![](http://ww1.sinaimg.cn/large/006gOeiSly1g128uib1lqj30ga07pmy7.jpg)

### 思路1

采用一个变量 pre_flag 记录上一次的正负，采用pre_val 来记录数组的上一个元素。 

先获得当前的正负标志，然后与之前的正负标志 pre_flag 比较，如果相同，则接着遍历下一个元素，如果不同，则更改标志，接着遍历。

### 思路2

采用状态机的思想， 设置为3个状态： BEGIN, UP, DOWN 三个状态， BEGIN 表示的是初始时的状态， UP 表示的是递增的状态， DOWN 表示的是递减的状态，只有当状态发生变化的时候，摇摆序列的长度才加一。


## 3. 移掉k位数字

- 来源：**[leetcode 402](https://leetcode-cn.com/problems/remove-k-digits/)**

- 题目：已知一个使用字符串表示的非负整数num，将num中的k个数字移除，求移除k个数字后，可获得的最小的可能的新数字。

- 说明： num 的长度小于 10002 且 ≥ k。num 不会包含任何前导零。

### 分析

一个长度为n的数字，去掉k个数字，有 C(k,n) = n!/((n-m)! * m!)， 因此使用枚举是不行的。

思考： 对于数字1432219， k=1， 我们只去掉一个数字，应该去掉哪个？ 如果 k > 1 ，我们应该按照什么顺序与策略进行删除数字，得到的结果最优？

分析： 若只去掉一个数字，为了使得得到的新数字最小，需要尽可能让新数字的最高位最小，其次是次高位最小，以此类推。

### 思路

从高位向低位遍历，如果对应的数字大于下一位数字，则把该位数字去掉，得到的数字最小。

使用栈存储最终结果和删除工作，从高位向低位遍历num， 如果遍历的数字大于栈顶元素，则将该元素push入栈， 如果小于栈顶元素，则进行pop， 直至栈为空或不能再删除数字或者栈顶小于当前元素为止。

## 4. 跳跃游戏

- 来源： [](https://leetcode-cn.com/problems/jump-game/)
- 问题： 给定一个非负整型数组nums，nums[i]表示可以从数组第i个位置最多向前跳跃nums[i]步；求是否可以从数组第0个位置跳跃到数组的最后一个元素的位置？

- 举例： [2, 3, 1, 1, 4]， 返回true

### 分析

如果此时处在第i个位置，该位置最远可以跳至第j位置(index[i])， 故第i位置还可跳至第 i+1, ... j-1,j位置；那么此时我们应该选择 index[i+1], ...index[j] 中最大的那个。

### 思路

从头到尾的遍历，找到最远能走到的位置

- 求从第i位置最远可跳至第index[i]位置： index[i] = nums[i] + i
- jump 表示当前所处的位置，初始化为0
- 设置变量max_index 表示从第0个位置直至jump位置这个过程中，最远可达到的位置，初始化为index[0]
- 利用jump扫描index数组，直到jump达到index数组尾部或者jump超过max_index， 扫描过程中，更新 max_index
- 若最终jump为数组长度，则返回true，否则，返回false

## 5. 跳跃游戏2

- 来源：[跳跃游戏2](https://leetcode-cn.com/problems/jump-game-ii/)
- 问题： 给定一个非负整型数组nums，nums[i]表示可以从数组第i个位置最多向前跳跃nums[i]步；确认可以从第0个位置跳跃到最后一个位置，求最少需要跳跃几次？

### 分析

在无法到达更远位置时，在这之前应该跳到一个可以到达更远位置的位置。

### 思路

- 设置current_max_index 为当前可达到的最远位置
- 设置pre_max_max_index 为在遍历各个位置的过程中，各个位置可达到的最远位置
- jump_min 为最少跳跃的次数
- 利用i遍历nums数组，若i超过 current_max_index, jump_min +1， current_max_index = pre_max_max_index
- 遍历过程中，若 nums[i] + i 更大，则更新 pre_max_max_index = nums[i] + i

## 6. 射击气球

- 来源： [leetcode 452](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)
- 问题： 在一个平面上有一定数量的气球，平面可以看做一个坐标系， 在平面的x轴的不同位置安排弓箭手向y轴方向射箭，弓箭可以向y轴走无穷远； 给定气球宽度 xstart <= x <= xend， 问至少需要多少弓箭手，才能将全部气球打爆？
- 举例： 气球: [[10,16], [2,8], [1,6], [7, 12]]， 至少需要2名弓箭手

### 思路

现将数组按照第一个元素排序， 维护一个射击范围，该射击范围表示几个球的重叠部分。 如果新球不在这个重叠范围内，则 弓箭手+1，更新重叠范围； 否则，更新重叠范围即可。

## 6. 最优加油方法

- 来源： POJ 2431
- 题目： 已知一条公路上， 有一个起点和一个终点，这之间有n个加油站； 已知从这n个加油站到终点的距离d与各个加油站可以加油的量 l，起点位置至终点的距离 L 与起始时刻邮箱中的汽油量P； 假设使用1个单位的汽油即走1个单位的距离，油箱没有上限，最少加几次油， 可以从起点开至终点？（如果无法到达终点，返回-1）