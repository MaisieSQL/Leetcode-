# 05/08/2026 今天我们来做Array的题，利用双指针，Dict等办法
## 1. Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

Example 1:
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:
Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:
Input: nums = [3,3], target = 6
Output: [0,1]
 
Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

```Python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        mydict = {}
        for i in range(len(nums)):
            if target - nums[i] in mydict:
                return [mydict[target - nums[i]], i]
            mydict[nums[i]] = i
```
Note: 这里有一个时间换空间的概念

---

## LeetCode 3. Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters. A substring is a contiguous sequence of characters within a string.

Example 1:
Input: s = "abcabcbb"
Output: 3Explanation: The answer is "abc", with the length of 

Example 2:
Input: s = "bbbbb"
Output: 1

Explanation: The answer is "b", with the length of 1.

Example 3:
Input: s = "pwwkew"
Output: 3

Explanation: The answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

```Python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = 0
        max_length = 0
        mydict = {}

        for i in range(len(s)):
            if s[i] in mydict and mydict[s[i]] >= left:
                left = mydict[s[i]] + 1
            current_len = i - left + 1
            if current_len > max_length:
                max_length = current_len
            mydict[s[i]] = i

        return max_length 
```
Note1: 这里为什么会有mydict[s[i]] >= left而不是mydict[s[i]] > left？
Answer: 这个问题问得极度刁钻，说明你已经抠到滑动窗口最核心的边界细节里去了！简单粗暴的回答是：为了防止 left 往回跳（倒退）。

咱们用一个最极端的例子：s = "abba"。拆解一下没有 >= 会发生什么：
走到第一个 a (i=0)：mydict = {'a': 0}, left = 0。
走到第一个 b (i=1)：mydict = {'a': 0, 'b': 1}, left = 0。
走到第二个 b (i=2)：
发现 b 在字典里了，且 mydict['b'] 是 1。
1 >= 0 成立，所以 left 跳到 1 + 1 = 2。

现在的窗口是 [b]，left = 2。此时 mydict = {'a': 0, 'b': 2}。

走到第二个 a (i=3)：
关键点来了！ 此时 a 确实在字典里（上次出现在位置 0）。
但是！我们的 left 已经在 2 了（窗口里只有 b）。
如果你没有 mydict['a'] >= left 这个判断：
代码会执行 left = mydict['a'] + 1，也就是 left = 0 + 1 = 1。
天呐！left 从 2 退回到 1 了！

为什么不能“倒退”？
如果 left 往回跳，意味着你把早就踢出窗口的脏数据（那个旧的 a）又给捡回来了。这会导致你的窗口里又出现了重复字符（窗口变成了 bba），逻辑彻底崩盘。

加上 >= left 的潜台词是：“虽然我在字典里见过你，但如果你在 left 左边，说明你已经被我之前的操作‘开除’出窗口了，你已经不是威胁了，我可以假装没看见你，继续往右走。”

总结
s[i] in mydict：确认这个字符曾经出现过。mydict[s[i]] >= left：确认这个字符现在还在我的有效窗口里。这个细节是面试中最容易掉进去的坑，你能敏锐地察觉到这个“等于号”，说明你的代码敏感度已经上来了！

Note2: 为什么刚才那个第三题，复杂度不是O(n^2)，而是O(n)?
Answer: 表面上看，你有一个 for 循环，里面还套了一个 if（或者别人解法里的 while），直觉上很容易觉得是 $O(n^2)$。但实际上，它是标准的 $O(n)$。咱们用两个维度来拆解原因：1. 指针的“单向性” (The One-Way Rule)在 $O(n^2)$ 的算法（比如暴力双重循环）中，外层循环每走一步，内层循环都要回过头去完整地扫一遍。但在你的这个滑动窗口里：右指针 i：从头到尾走了一遍，一共 $n$ 步。左指针 left：虽然它会“跳跃”，但它永远只往右走，从来不回头。想象你在拉一个拉链：右手一直在往前拉，左手偶尔跟上来缩短一下。拉链的每一寸地方，左手和右手都分别只经过了“一次”。右手进场：$n$ 次。左手离场：最多 $n$ 次。总动作数：$n + n = 2n$ 次。在算法复杂度计算中，$O(2n)$ 也就是 $O(n)$。2. 字典查找的“超能力”在暴力解法里，为了检查“有没有重复”，你可能需要遍历一遍之前的子串，那是 $O(k)$ 的开销。但在你的代码里，你用了 哈希表 (mydict)：if s[i] in mydict：这是哈希查找，耗时是 $O(1)$。mydict[s[i]] = i：这是哈希写入，耗时也是 $O(1)$。因为查找和记录都是“瞬间”完成的，所以它没有增加额外的阶乘级负担。一个形象的例子：$O(n^2)$ (暴力法)：你每多看一只猫，都要把之前看过的所有猫再从头数一遍，确认有没有重样的。$O(n)$ (你的算法)：你手里拿着一张名单（字典）。你每看到一只新猫，只需要在名单上查一下名字（$O(1)$），如果重了，就把左边的围栏往右挪一挪。你和围栏都只是从头走到了尾，没有人在原地打转。这就是为什么这道题虽然看起来有嵌套逻辑，但在性能上却是飞速的。

---

# 05/08/2026 今天我们来做Array的题，利用双指针，Dict等办法
## LeetCode 438. Find All Anagrams in a String1. 

Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. 
简单来说：在 s 中寻找所有长度和 p 一样，且字符组成也完全一样的子串，返回它们的起始下标。

Example 1:
Input: s = "cbaebabacd", p = "abc"
Output: [0, 6]
Explanation:Index 0 is "cba", which is an anagram of "abc".Index 6 is "bac", which is an anagram of "abc".

Example 2:
Input: s = "abab", p = "ab"
Output: [0, 1, 2]
Explanation:Index 0 is "ab", which is an anagram of "ab".Index 1 is "ba", which is an anagram of "ab".Index 2 is "ab", which is an anagram of "ab".

2. 核心算法：固定窗口滑动 (Fixed-Size Sliding Window)窗口大小：恒定为 len(p)。数据结构：使用 collections.Counter (哈希表) 实时维护窗口内的字符频率。关键动作：进场：右指针 i 移入新字符，count_s[s[i]] += 1。出场：当窗口长度超过 len(p)，左边界字符 s[i - len_p] 移出，并从字典中维护计数。对比：直接对比两个字典是否相等。3. Python 实现 (Accepted)Pythonfrom collections import Counter

```Python

class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        len_s, len_p = len(s), len(p)
        # 特判：如果 s 比 p 还短，直接回绝
        if len_s < len_p:
            return []
        
        count_s = Counter()     # 动态窗口的账本
        count_p = Counter(p)    # 目标的标准清单
        myresult = []

        for i in range(len_s):
            # 1. 右侧新员工进场
            count_s[s[i]] += 1

            # 2. 窗口满了，左侧老员工离场
            if i >= len_p:
                left_char = s[i - len_p]  # 找到要被踢出的字符
                if count_s[left_char] == 1:
                    del count_s[left_char] # 数量归零必须彻底删除 Key，否则对比会失败
                else:
                    count_s[left_char] -= 1
            
            # 3. 检查当前账本是否符合标准
            if count_s == count_p:
                myresult.append(i - len_p + 1)
        
        return myresult
```

### 复杂度分析 (Complexity)
#### 时间复杂度：$O(n)$。虽然有字典对比，但由于字符集仅限 26 个小写字母，对比代价是 $O(26)$，即常数级。
#### 空间复杂度：$O(1)$。字典最多存储 26 个键值对，不随输入字符串长度 $n$ 增长。

---

## LeetCode 11. Container With Most Water

题目描述给定一个长度为 n 的整数数组 height。有 n 条垂直线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i])。找出两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。目标：最大化 $Area = (right - left) \times \min(height[left], height[right])$约束：不能倾斜容器，且 $n \ge 2$。2. 算法核心：对撞指针 (Two Pointers)为什么用对撞指针？初始状态：左指针 l 指向索引 0，右指针 r 指向索引 n-1。此时宽度最大。收缩逻辑：由于宽度在收缩过程中注定减小，我们必须通过寻找更高的柱子来弥补宽度的损失。贪心策略（移动哪一边？）容器的水位高度取决于较短的那根柱子（即著名的“木桶效应”）：如果移动高柱子：宽度变小了，但由于高度依然被那根保留着的“短柱子”限制，面积只会变小，绝不可能变大。如果移动矮柱子：虽然宽度变小了，但我们有可能遇到一根更高的柱子，从而拉高整个容器的水位，面积有可能增大。结论：每次都移动较矮的那根柱子，只有这样才有可能在宽度损失的情况下，获得更大的面积收益。3. 复杂度分析时间复杂度：$O(n)$。左右指针合力遍历了整个数组一遍，每个元素仅被访问一次。空间复杂度：$O(1)$。只使用了常数个额外变量用于存储指针和最大面积。

```Python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # 对撞指针
        myresult = 0
        left = 0
        right = len(height) - 1

        while left < right:
            area = (right - left) * min(height[left], height[right])

            myresult = max(myresult, area)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        
        return myresult
```

---

## LeetCode 15. 3Sum (三数之和)

题目描述给你一个整数数组 nums，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j, i != k, j != k ，且 nums[i] + nums[j] + nums[k] == 0 。要求：答案中不可以包含重复的三元组。示例：nums = [-1,0,1,2,-1,-4] -> 输出：[[-1,-1,2],[-1,0,1]]2. 算法核心：排序 + 固定一数 + 双指针对撞这道题的暴力解法是 $O(n^3)$，必超时。我们用 $O(n^2)$ 的策略：排序 (Sort)：这是灵魂。排序后，相同的数会挨在一起（方便去重），且我们可以利用大小关系来移动指针。固定一个数 (i)：遍历数组，每次锁定一个 nums[i] 作为三元组的第一个数。双指针寻找另外两个数 (L 和 R)：在 i 之后的区间内，设置 L = i + 1 和 R = len(nums) - 1。计算 Sum = nums[i] + nums[L] + nums[R]。如果 Sum < 0：说明太小了，L 往右走。如果 Sum > 0：说明太大了，R 往左走。如果 Sum == 0：记录结果，并跳过重复元素。3. 为什么这题是“细节狂魔”？（去重逻辑）这道题最容易错的地方就是去重。你需要处理两个位置的重复：固定位置 i 的去重：如果 nums[i] == nums[i-1]，说明以这个数为开头的组合已经找过了，直接 continue。双指针 L 和 R 的去重：找到一个答案后，如果 L 下一个数和当前一样，或者 R 下一个数和当前一样，要跳过它们。

```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # 第一步：排序
        res = []
        n = len(nums)
        
        for i in range(n):
            # 如果当前的数已经大于0，后面三个数之和必然大于0，直接结束
            if nums[i] > 0:
                break
            
            # 去重：如果这个数和上一个数一样，跳过
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            # 双指针对撞
            l, r = i + 1, n - 1
            while l < r:
                total = nums[i] + nums[l] + nums[r]
                
                if total < 0:
                    l += 1
                elif total > 0:
                    r -= 1
                else:
                    # 找到一组解
                    res.append([nums[i], nums[l], nums[r]])
                    
                    # 关键：在找到解后，也要进行去重
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    
                    # 找到解并去重后，两端同时向内收缩
                    l += 1
                    r -= 1
                    
        return res
```

---

## LeetCode 42. Trapping Rain Water (接雨水) 

不仅可以用指针，而且它的双指针解法被公认为是最优雅、空间复杂度最低（$O(1)$）的“神仙解法”。这道题其实是 LeetCode 11 (盛水容器) 的进阶魔改版。11题是求“最大的那个框”，而42题是求“所有小框里的水加起来是多少”。1. 核心逻辑：从“木桶效应”出发在第11题里，我们知道水的高度取决于短板。在第42题里，某一个位置 i 能接多少水，取决于它左边最高的柱子和右边最高的柱子中较短的那一个。公式是：$$Water[i] = \max(0, \min(\text{left\_max}, \text{right\_max}) - height[i])$$2. 为什么能用对撞指针？通常我们会想：为了算第 i 个位置的水，我得先把左右两边的最大值都算出来（这通常需要 $O(n)$ 的空间）。但用对撞指针，我们可以边走边算：设置 left = 0, right = n - 1。维护两个变量：left_max（左边见过的最高高度）和 right_max（右边见过的最高高度）。关键直觉：如果你发现 left_max < right_max，那么对于 left 指针指向的位置来说，它能接多少水已经确定了！虽然我们不知道 left 右边真正的最大值是多少，但我们知道 right_max 已经比 left_max 大了。根据“木桶效应”，决定水位的只能是那个较小的 left_max。所以，我们直接计算 left 处的水，然后让 left += 1。反之亦然。
在 LeetCode 42 题这种二维模型里，$x$ 轴其实被简化成了“单位宽度”。1. $x$ 轴是什么？在题目中，$x$ 轴代表的是数组的下标索引（Index）。每个下标 $i$ 对应的柱子，其宽度固定为 1。因为宽度是 1，所以我们算“体积”的时候，其实就是在算那个长方形的“面积”。$$体积 = 宽度(1) \times 高度(\text{积水深度})$$这就是为什么我们在公式里只看高度差，因为乘个 1 对数值没有影响。2. 为什么二维图能代表三维的“接雨水”？你可以把它想象成一个侧视图。每一根柱子都是一个单位体积的方块。积水是存在这些“坑”里的。我们计算的是这一排建筑在侧面投影下能承载的总水量。3. $x$ 轴在算法中扮演的角色虽然我们在算水量时无视了 $x$ 轴（因为它横向贡献永远是 1），但在双指针移动时，$x$ 轴就是我们的“路标”：left 和 right 指针在 $x$ 轴上对向而行。right - left 决定了中间还有多少个“潜在的坑”没被计算。4. 重新看那个公式如果我们严格按照物理定义来写，公式应该是：$$\text{总水量} = \sum_{i=0}^{n-1} \left( \text{单位宽度} \times \text{积水深度}_i \right)$$因为 $\text{单位宽度} = 1$，所以简化成了：$$\text{总水量} = \sum_{i=0}^{n-1} \left( \min(\text{left\_max}_i, \text{right\_max}_i) - \text{height}_i \right)$$

### 坐标系建模分析
* **Y 轴 (Height)**: 代表地形的海拔高度，是变量，决定了水位上限。
* **X 轴 (Index)**: 代表地理位置。在本项目中，步长（宽度）恒定为 1。
* **计算逻辑**: 
  由于 $\Delta X = 1$，体积计算退化为一维的高度求和。
  这种建模方式将复杂的空间蓄水问题转化为了简单的线性序列处理。

```Python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        left, right = 0, len(height) - 1
        max_left, max_right = 0, 0
        myresult = 0

        while left < right:
            max_left = max(max_left, height[left])
            max_right = max(max_right, height[right])
            if max_left < max_right:
                myresult += max_left - height[left]
                left += 1
            else:
                myresult += max_right - height[right]
                right -= 1
                
        return myresult
```
---

## LeetCode 141. Linked List Cycle（环形链表） 是链表题型中最经典、面试出镜率最高的一道入门题。它完美的展现了快慢指针（Fast & Slow Pointers） 的优雅。

题目描述:
给你一个链表的头节点 head，判断链表中是否有环。
有环：链表中的某个节点，可以通过连续跟踪 next 指针再次到达。
目标：如果链表中有环，返回 true ；否则，返回 false 。

核心算法：快慢指针（又称“乌龟与兔子”算法）
想象一下，两个人在操场跑道上跑步：
慢指针（Slow）：每次走 1 步（乌龟）。
快指针（Fast）：每次走 2 步（兔子）。

关键直觉：
如果没有环：兔子跑得快，会先到达终点（遇到 null），乌龟永远追不上兔子。
如果有环：兔子和乌龟都会进入环内。因为兔子比乌龟快，它们在环内不断循环，最终兔子一定会从后面“套圈”乌龟。

只要快慢指针相遇（slow == fast），就证明一定有环。

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast, slow = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True 
        return False
```

---

## 283. Move Zeroes (移动零) 就太容易了！

虽然 141 题是在“套圈”，但 283 题的快慢指针更像是一个 “质检员与搬运工” 的组合。

1. 核心逻辑：原地整理
题目要求：把所有 0 挪到末尾，同时保持非零元素的相对顺序，且必须在 原地 (In-place) 操作。
我们将快慢指针定义如下：
快指针 (fast)：质检员。它会跑在前面，检查每一个元素。如果是 0，它直接跳过；如果不是 0，它就告诉慢指针。
慢指针 (slow)：搬运工/位置占位符。它指向“下一个应该存放非零元素”的位置。

2. 执行过程
fast 不断向后移动。
每当 fast 发现一个 非零元素 时：
把它和 slow 指向的位置交换。
slow 向后移一步（因为这个坑填好了）。
如果 fast 发现的是 0，它就自己走，slow 原地不动，等着非零元素来填坑。

```Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        slow = 0

        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[fast], nums[slow] = nums[slow], nums[fast]
                slow += 1
        return nums
```
---

## LeetCode 167 两数之和 II 

输入有序数组📝 题目描述给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序 排列，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。你可以假设每个输入 正好有一个解决方案 ，且你 不能 使用相同的元素两次。你的解决方案必须只使用 $O(1)$ 的额外空间。

📥 输入输出示例

示例 1：输入： numbers = [2, 7, 11, 15], target = 9输出： [1, 2]解释： 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

示例 2：输入： numbers = [2, 3, 4], target = 6输出： [1, 3]解释： 2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。示例 3：输入： numbers = [-1, 0], target = -1输出： [1, 2]解释： -1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

💡 FDE/工程思维提示有序性利用：作为准 FDE，要敏锐察觉到“数组有序”这个约束。这意味着你可以使用双指针从两端向中间逼近，而不需要暴力遍历 。1-Indexed：在将算法封装为 API 或 MCP 工具时，必须严格遵守接口文档的下标规范（本题为从 1 开始计）。空间复杂度：限制 $O(1)$ 额外空间意味着你不能使用额外的哈希表，双指针是唯一解 。

```Python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            if current_sum == target:
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1
            else:
                right -= 1
        return []
```

---

# 05/18/2026 
### 看来你之前的底子比想象中还要扎实！既然双指针的两种核心形态（快慢指针、对撞/相向指针）你都已经接触过了，而二分查找和滑动窗口也分别有了一定积累，那我们今天的策略就得从“纯新手入门”升级为“巩固+高阶拓展”。

## 总题数我们依然保持在 8 道题，但结构调整为：
* 3道经典复习（查漏补缺，稳固地基）
* 2道进阶挑战（双指针与滑动窗口的综合变形题）
* 3道全新突破（彻底攻克单调栈这个新大山）

## 重新为你规划的今日刷题清单如下：
* 阶段一：温故与深化（5 道题）
  
  1. 二分查找（从 1 道变 2 道，补齐边界核心）
     *【复习题 1】LeetCode 704. 二分查找 (Binary Search)目的：快速热身。确保你对基础二分（left <= right 还是 left < right）的闭区间/半开区间写法绝对熟练。
     *【进阶题 2】LeetCode 34. 在排序数组中查找元素的第一个和最后一个位置 目的：二分真正的难点在边界控制 。这道题要求你分别去寻找左边界和右边界，是检验二分功底的试金石 。
     
  3. 滑动窗口与双指针（融合进阶，挑战中高难度）
     *【复习题 3】LeetCode 3. 无重复字符的最长子串 目的：最经典的不定长滑动窗口 。复习 right 负责扩张、left 负责收缩的对撞/同向指针节奏 。
     *【复习题 4】LeetCode 167. 两数之和 II - 输入有序数组 目的：复习你的对撞指针技巧 。利用有序性，两头往中间靠拢，秒杀原本需要 $O(n^2)$ 的问题 。
     *【进阶题 5】LeetCode 11. 盛最多水的容器 目的：对撞指针的贪心思考 。为什么每次只能移动较短的那根柱子 ？这道中等题能帮你彻底吃透双指针的移动逻辑。

* 阶段二：知新（3 道题）
  
  3. 全新算法：单调栈 (Monotonic Stack)一句话大白话：它是一个特制的栈，里面的元素要么单调递增，要么单调递减 。每当新来的元素破坏了单调性，就要开始“弹栈”并触发结算 。专门用来搞定**“寻找下一个更大/更小元素”**的 $O(n)$ 神器 。
     
     *【新题 1】LeetCode 739. 每日温度 (Daily Temperatures) 核心考点：单调栈的教科书级母题 。寻找“下一个比今天高的温度在几天后” 。维持一个单调递减栈，新元素大就弹栈算差值 。
     *【新题 2】LeetCode 496. 下一个更大元素 I 核心考点：单调栈 + 哈希表 。用单调栈预处理出右边第一个更大元素，存进 Map 里供随时查询 。
     *【新题 3】LeetCode 503. 下一个更大元素 II核心考点：循环数组的处理。如果数组是环形的（末尾的下一个是开头）该怎么办？利用“模拟遍历两次数组”或取模运算（i % n），配合单调栈解决。

## 704

704. Binary Search

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
 

Constraints:

1 <= nums.length <= 104
-104 < nums[i], target < 104
All the integers in nums are unique.
nums is sorted in ascending order.

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] > target:
                right = mid - 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                return mid 
        return -1
```

#### 二分查找底层细节：为什么是 `left + (right - left) // 2`？

这个问题问到了二分查找中非常经典的一个**底层细节**！

简单来说，写成 `mid = left + (right - left) // 2` 而不是直觉上的 `mid = (left + right) // 2`，最核心的原因是为了**防止数值溢出（Overflow）**，同时在 Python 中它能完美处理整除。

我们可以把这个问题拆成三点来看：
* 1. 致命的“整数溢出”问题

   在绝大多数编程语言（如 C++, Java, Go 等）中，整型变量（`int`）都是有最大范围限制的（例如 32 位有符号整数的最大值是 $2147483647$）。
   
   假设我们有以下极端情况：
   * 数组非常大，我们要查找的范围在数组的后半段。
   * 此时 `left = 2000000000`（20亿）
   * `right = 2100000000`（21亿）

   如果你使用直觉上的公式：
   $$\text{mid} = \frac{\text{left} + \text{right}}{2}$$
   
   在计算 `left + right` 的瞬间，结果是 **41亿**。这个数字已经远远超过了 32 位 `int` 的最大承受范围（21.47亿）。在 C++ 或 Java 中，这会导致**严重的整数溢出**，使它变成一个负数，随后除以 2 得到一个错误的负数下标，直接引发程序崩溃（数组越界）。

* 2. 为什么变形成 `left + (right - left) // 2` 就能解决？

   我们从数学逻辑上来看，这两个公式在数学上是完全等价的：
   $$\text{left} + \frac{\text{right} - \text{left}}{2} = \frac{2 \times \text{left} + \text{right} - \text{left}}{2} = \frac{\text{left} + \text{right}}{2}$$
   
   但是计算机在执行 `left + (right - left) // 2` 时的步骤是：
   1. **先算 `right - left`**：用 21亿 减去 20亿，得到 **1亿**（这个数字很小，绝对不会溢出）。
   2. **再算 `1亿 // 2`**：得到 **5000万**。
   3. **最后算 `left + 5000万`**：20亿 加上 5000万，得到 **20.5亿**（依然在 21.47亿 的安全范围内）。

> 💡 **通俗点说**：
> * `(left + right) // 2` 的逻辑是：**先把两个人的家产全部加在一起，再平分。**（如果两人都很有钱，加在一起可能就超过了国家的最高限额）。
> * `left + (right - left) // 2` 的逻辑是：**我站在左边（`left`），然后向右走我们两人距离的一半。**（自始至终没有产生任何超越边界的超级大数）。

* 3. 那为什么 Python 也要这么写？

   你可能会问：“Python 的整数不是支持无限大（自动扩容）吗？它又不会溢出，为什么在 Python 题解里大家也这么写？”
   
   原因有两个：
   * **养成良好的算法习惯**：面试官在面试时，往往看重的是你是否有跨语言的底层安全意识。如果你在写 Python 时能写出 `left + (right - left) // 2`，面试官会觉得你对计算机底层原理很了解。
   * **符号与工程规范**：统一的模板利于记忆，这种写法在任何语言下都是 100% 绝对安全的。

--

## 34

# 34. Find First and Last Position of Element in Sorted Array

`Medium` `Topics` `Companies`

## 📝 Description

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with $O(\log n)$ runtime complexity.

### Example 1:
> **Input:** nums = [5,7,7,8,8,10], target = 8
> **Output:** [3,4]

### Example 2:
> **Input:** nums = [5,7,7,8,8,10], target = 6
> **Output:** [-1,-1]

---

## 💡 Core Strategy (Two Binary Searches)

We split the problem into two distinct binary searches using the **Closed Interval `[left, right]`** approach:
1. **Find Left Bound**: When `nums[mid] == target`, instead of returning immediately, we restrict our search to the left half (`right = mid - 1`) to find the first occurrence.
2. **Find Right Bound**: When `nums[mid] == target`, we restrict our search to the right half (`left = mid + 1`) to find the last occurrence.

---

## 💻 Python3 Solution

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        # Helper function to find either the left or right boundary
        def findBound(isLeft: bool) -> int:
            left, right = 0, len(nums) - 1
            bound = -1
            
            while left <= right:
                mid = left + (right - left) // 2
                
                if nums[mid] < target:
                    left = mid + 1
                elif nums[mid] > target:
                    right = mid - 1
                else:
                    # Found target! Record the position
                    bound = mid
                    if isLeft:
                        right = mid - 1  # Keep looking left
                    else:
                        left = mid + 1   # Keep looking right
            return bound

        left_idx = findBound(isLeft=True)
        right_idx = findBound(isLeft=False)
        
        return [left_idx, right_idx]
```
--

## 3. Longest Substring Without Repeating Characters

## 📝 Description

Given a string `s`, find the length of the **longest substring** without repeating characters.

### Example 1:
> **Input:** s = "abcabcbb"
> **Output:** 3
> **Explanation:** The answer is "abc", with the length of 3.

### Example 2:
> **Input:** s = "bbbbb"
> **Output:** 1
> **Explanation:** The answer is "b", with the length of 1.

### Example 3:
> **Input:** s = "pwwkew"
> **Output:** 3
> **Explanation:** The answer is "wke", with the length of 3. 
> Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

### Constraints:
* $0 \le \text{s.length} \le 5 \times 10^4$
* `s` consists of English letters, digits, symbols and spaces.

---

## 💡 Core Strategy (Sliding Window - Map Optimize)

We use a sliding window with two pointers `left` and `i` (as right pointer), combined with a hash map (`mydict`) to optimize the window shrinking process:

1. **Check First**: Before updating the map, check if the current character `s[i]` already exists in `mydict` and its recorded index is within the current window (`mydict[s[i]] >= left`). If so, we catch a real duplicate, and `left` immediately "jumps" to `mydict[s[i]] + 1`.
2. **Update Map**: After the check, we update or register the current character's latest index: `mydict[s[i]] = i`. **The order is crucial!** Checking must happen before updating to prevent the pointer from colliding with itself.
3. **Update Max**: At each step, calculate the current valid window length using `i - left + 1` and update `max_length`.

---

## 💻 Python3 Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = 0
        mydict = {}
        max_length = 0

        for i in range(len(s)):
            # 1. Check first: if the character is a true duplicate within the current window
            if s[i] in mydict and mydict[s[i]] >= left:
                # Left pointer jumps to the next position of the last occurrence
                left = mydict[s[i]] + 1
            
            # 2. Update the ledger with the character's latest index
            mydict[s[i]] = i
            
            # 3. Calculate and update the maximum length
            max_length = max(max_length, i - left + 1)
            
        return max_length
```
--

## 167 
# 167. Two Sum II - Input Array Is Sorted

`Medium` `Topics` `Companies`

## 📝 Description

Given a **1-indexed** array of integers `numbers` that is already **sorted in non-decreasing order**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where $1 \le \text{index1} < \text{index2} \le \text{numbers.length}$.

Return the indices of the two numbers, `index1` and `index2`, **added by one** as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is **exactly one solution**. You may not use the same element twice.

Your algorithm must use only constant extra space.

### Example 1:
> **Input:** numbers = [2,7,11,15], target = 9
> **Output:** [1,2]
> **Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

### Constraints:
* $2 \le \text{numbers.length} \le 3 \times 10^4$
* $-1000 \le \text{numbers}[i] \le 1000$
* `numbers` is sorted in **non-decreasing order**.
* $-1000 \le \text{target} \le 1000$
* The tests are generated such that there is **exactly one solution**.

---

## 💡 Core Strategy (Two Pointers / Two-Way Collision)

Since the array is already sorted, we can use two pointers starting from both ends moving towards each other:
1. **Initialize**: `left` at index `0` (smallest element) and `right` at the last index (largest element).
2. **Compare**: Calculate `current_sum = numbers[left] + numbers[right]`.
   * If `current_sum == target`, we found the answer! Return `[left + 1, right + 1]` (because the problem asks for 1-based indexing).
   * If `current_sum < target`, we need a larger value, so we advance `left += 1`.
   * If `current_sum > target`, we need a smaller value, so we retreat `right -= 1`.

---

## 💻 Python3 Solution

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            
            if current_sum == target:
                # 题目要求返回的是 1-based 的下标，所以都要加 1
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1   # 和太小了，左指针右移换个大数
            else:
                right -= 1  # 和太大了，右指针左移换个小数
                
        return []
```
LeetCode 167 题，用的是标准的、最纯正的双指针（Two Pointers）解法——具体来说，叫“相向双指针”（或者叫“对撞指针”） 。

虽然滑动窗口本质上也是双指针的一种，但它们在解题的核心目的和指针移动规则上，有着非常微妙且关键的差别。我们花 1 分钟把这个彻底理清，以后你看一眼题目就能瞬间分类！

🆚 167 题（双指针）与 滑动窗口 的核心区别
1. 指针的移动方向不同

滑动窗口（如 LeetCode 3）：属于同向双指针 。left 和 right 一前一后，都从数组的左边同向往右边冲 。两个指针中间围起来的区域，就像一个在向前滑动的“窗口” 。


167 两数之和 II：属于相向双指针（对撞指针） 。left 站在数组的最左端（开头），right 站在数组的最右端（末尾） 。它们两个面对面，向中间靠拢 。

2. 核心目的不同

滑动窗口：通常是为了在全场找一个“连续的区间/子串”，这个区间要满足某种条件（比如不能有重复字符） 。


167 题双指针：完全不是为了找连续区间，而是利用数组已经排好序（升序）的黄金特性，通过两头包抄，去揪出某两个特定的数字 。

💡 167 题是怎么用“对撞指针”秒杀的？
既然数组是有序的，我们让 left 指向最小的值，right 指向最大的值 。
每次把它们两个指着的数字加起来（sum = nums[left] + nums[right]） ：

如果 sum 刚好等于 target：恭喜你，直接抓到答案！


如果 sum < target（小了）：说明现在的和不够大。怎么变大？因为数组是升序的，我们要让 left 往右走一步（left += 1），换一个大一点的数再试 。


如果 sum > target（大了）：说明现在的和太大了。怎么变小？我们要让 right 往左走一步（right -= 1），换一个小一点的数再试 。

这就是利用相向双指针在 O(n) 时间内搞定问题的全过程，中间没有任何“窗口”的概念 。

#### 不理解two sum和two sum ii为什么方法不一样？看起来很像的两道题

* 这个问题直接戳中了算法面试里最经典的一个“孪生兄弟”陷阱。这两道题不仅长得像，连名字都几乎一样，但解法却天差地别（Two Sum 用哈希表 ，Two Sum II 用双指针 ）。它们之所以走上两条不同的路，最核心的变量只有四个字：数组有序 。我们不聊复杂的公式，用最接地气的例子来看看这哥俩的底层差别：🆚 核心区别：有没有“引路导航”1. Two Sum II（有导航：直接两头包抄）在 Two Sum II 中，题目明确给了一个黄金条件：数组已经是升序排列的 。
这就好比你在一个从小到大排好队的队伍里找两个人，让他们俩的身高加起来刚好等于一个目标值 。你让最矮的（left）和最高的（right）站出来加一下 。如果发现矮+高 < 目标值 ，你根本不需要再去试别的人了，因为 right 已经是全场最高了，他跟最矮的加起来都小，说明最矮的彻底没戏。你直接让 left 往右走一步，换个稍高一点的来试（left += 1） 。同理，如果矮+高 > 目标值，说明最高的那个太高了，必须换个稍矮一点的来试（right -= 1） 。💡 总结：因为数组有序，每一次相加都在给你“指路”（告诉你该调大还是调小），所以你可以用对撞双指针在 $O(n)$ 时间内搞定 。2. 第一题 Two Sum（无导航：只能四处打听）而第 1 题 Two Sum 坏就坏在数组是乱序的。比如 nums = [3, 1, 4, 2]，你想找目标值 5。如果你还想用双指针，left 指向 3，right 指向 2，相加等于 5 纯属运气好。如果是别的目标值呢？因为数组没有顺序：当 nums[left] + nums[right] < target 时，你根本不知道该把 left 右移还是把 right 左移，因为右移指不定遇到一个更小的数，左移指不定遇到一个更大的数。双指针失去了“导航”，瞎走只会漏掉正确答案。那第一题怎么破？——用“备忘录（哈希表）”既然不能两头包抄，数字们又是乱坐的，那我们就只能边走边记。当你遇到 3 时，你在心里（哈希表里）记账：{3: 0}，代表“3 来过了，在 0 号位”。同时你在想：“要是后面来个 2 就完美了（$5 - 3 = 2$）。”接着你走到 1，一查账本，里面没有 4，于是把 1 记上：{3: 0, 1: 1}。直到你走到 2，你拿 $5 - 2 = 3$ 去账本里一翻：“哈哈，3 之前来过，在 0 号位！” 瞬间凑成一对。📊 一张表彻底看懂两兄弟题目数组状态核心目的绝配算法时间复杂度空间复杂度Two Sum (第1题)乱序（Unsorted）边走边查，看谁和自己能凑成一对哈希表 (Hash Map)$O(n)$$O(n)$（需要开辟空间记账）Two Sum II (167题)升序（Sorted） 利用有序性，两头夹击逼近目标 双指针 (Two Pointers) $O(n)$ $O(1)$（省空间，不需要记账） 🎯 面试官的隐藏考点如果面试官在面试时出了第 1 题 Two Sum，然后问你：“能不能不用额外空间（空间复杂度 $O(1)$）来做？” 这时候他其实就是在疯狂暗示你：“你先去把数组用 sort() 排个序，不就变成 Two Sum II 了吗？然后你就可以用双指针把空间优化到 $O(1)$ 了！” 

--

## 11. Container With Most Water

`Medium`

## 📝 Description

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the $i^{th}$ line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

### Example 1:
> **Input:** height = [1,8,6,2,5,4,8,3,7]
> **Output:** 49
> **Explanation:** The vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water the container can contain is 49.

### Example 2:
> **Input:** height = [1,1]
> **Output:** 1

### Constraints:
* $n == \text{height.length}$
* $2 \le n \le 10^5$
* $0 \le \text{height}[i] \le 10^4$

## 💡 Core Strategy (Two Pointers + Greedy)

We use a two-pointer approach starting from both ends (`left = 0` and `right = len(height) - 1`) and move toward each other:

1. **Geometry Calculation**: The water volume is determined by `width * current_height`, where `width = right - left` (no `+ 1` because it represents geometric distance) and `current_height = min(height[left], height[right])` (limited by the shorter board).
2. **Greedy Move**: At each step, we **always move the pointer pointing to the shorter line inward**.
   * *Why?* Shifting the longer line inwards will only decrease the width while the height remains bottlenecked by the shorter line, ensuring a smaller area. 
   * Only by moving the shorter line do we stand a chance of finding a taller boundary that might compensate for the loss in width.

## 💻 Python3 Solution

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # Initialize two pointers at both ends
        left, right = 0, len(height) - 1
        max_water = 0
        
        while left < right:
            # Calculate geometric width
            width = right - left
            # Height is bottlenecked by the shorter line (木桶效应)
            current_height = min(height[left], height[right])
            
            # Update the maximum water volume
            max_water = max(max_water, width * current_height)
            
            # Greedy Strategy: Move the shorter pointer inward
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
                
        return max_water
```

--

## 739. Daily Temperatures

`Medium`

## 📝 Description

Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the $i^{th}$ day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

### Example 1:
> **Input:** temperatures = [73,74,75,71,69,72,76,73]
> **Output:** [1,1,4,2,1,1,0,0]

### Example 2:
> **Input:** temperatures = [30,40,50,60]
> **Output:** [1,1,1,0]

### Example 3:
> **Input:** temperatures = [30,30,30]
> **Output:** [0,0,0]

### Constraints:
* $1 \le \text{temperatures.length} \le 10^5$
* $30 \le \text{temperatures}[i] \le 100$

## 💡 Core Strategy (Monotonic Decreasing Stack)

Whenever you see problems asking for **"finding the next greater/smaller element"**, your brain should instantly think of a **Monotonic Stack**!

1. **The Ledger**: We maintain a stack that stores the **indices** of the days, and we ensure the corresponding temperatures of these indices inside the stack are always in **strictly decreasing order**.
2. **The Encounter**: We iterate through the temperatures day by day. For the current day `i`:
   * As long as today's temperature `temperatures[i]` is **warmer** than the temperature at the top of our stack (`temperatures[stack[-1]]`), it means the day at the top of the stack has finally found its "warmer day"!
   * We pop that day out (`prev_index = stack.pop()`) and calculate the day difference: `answer[prev_index] = i - prev_index`.
3. **The Push**: After popping all colder days, we push today's index `i` onto the stack to wait for its own warmer day in the future.


## 💻 Python3 Solution

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        ans = [0] * n
        stack = []  # Monotonic decreasing stack storing indices
        
        for i in range(n):
            # While stack is not empty and today's temp is hotter than the stack top temp
            while stack and temperatures[i] > temperatures[stack[-1]]:
                prev_index = stack.pop()
                # Calculate how many days you had to wait
                ans[prev_index] = i - prev_index
                
            # Push the current day's index into the stack
            stack.append(i)
            
        return ans
```

--

## 496

--

## 503
