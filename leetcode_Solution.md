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
     【复习题 1】LeetCode 704. 二分查找 (Binary Search)目的：快速热身。确保你对基础二分（left <= right 还是 left < right）的闭区间/半开区间写法绝对熟练。
     【进阶题 2】LeetCode 34. 在排序数组中查找元素的第一个和最后一个位置 目的：二分真正的难点在边界控制 。这道题要求你分别去寻找左边界和右边界，是检验二分功底的试金石 。
     
  3. 滑动窗口与双指针（融合进阶，挑战中高难度）
     【复习题 3】LeetCode 3. 无重复字符的最长子串 目的：最经典的不定长滑动窗口 。复习 right 负责扩张、left 负责收缩的对撞/同向指针节奏 。
     【复习题 4】LeetCode 167. 两数之和 II - 输入有序数组 目的：复习你的对撞指针技巧 。利用有序性，两头往中间靠拢，秒杀原本需要 $O(n^2)$ 的问题 。
     【进阶题 5】LeetCode 11. 盛最多水的容器 目的：对撞指针的贪心思考 。为什么每次只能移动较短的那根柱子 ？这道中等题能帮你彻底吃透双指针的移动逻辑。

* 阶段二：知新（3 道题）
  
  3. 全新算法：单调栈 (Monotonic Stack)一句话大白话：它是一个特制的栈，里面的元素要么单调递增，要么单调递减 。每当新来的元素破坏了单调性，就要开始“弹栈”并触发结算 。专门用来搞定**“寻找下一个更大/更小元素”**的 $O(n)$ 神器 。
     
     【新题 1】LeetCode 739. 每日温度 (Daily Temperatures) 核心考点：单调栈的教科书级母题 。寻找“下一个比今天高的温度在几天后” 。维持一个单调递减栈，新元素大就弹栈算差值 。
     
     【新题 2】LeetCode 496. 下一个更大元素 I 核心考点：单调栈 + 哈希表 。用单调栈预处理出右边第一个更大元素，存进 Map 里供随时查询 。
     
     【新题 3】LeetCode 503. 下一个更大元素 II核心考点：循环数组的处理。如果数组是环形的（末尾的下一个是开头）该怎么办？利用“模拟遍历两次数组”或取模运算（i % n），配合单调栈解决。

