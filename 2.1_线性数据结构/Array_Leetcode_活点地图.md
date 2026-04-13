# 
# 📊 Array 题库聚类分析：1554 道题背后的真相

> **原则**：Google leetcode Array一共有1554 Questions。题目是无限的，但底层的“步法”是有限的。数据结构是载体，算法步法是核心。将 1554 道题降维打击，归类为 6 大核心模式。

---

## 1. 基础操作类 (Basic Manipulation)
* **核心逻辑**：考察对数组连续存储特性的物理操作。
* **Google 考察重点**：**Space $O(1)$** (原地修改，不允许开辟新空间)。
* **典型操作**：反转 (Reverse)、旋转 (Rotate)、移除 (Remove)、合并 (Merge)。
* **代表题**：
    * `LC 27` Remove Element
    * `LC 189` Rotate Array

---

## 2. 线性扫描与双指针 (Linear Scan & Two Pointers)
* **核心逻辑**：利用一两个指针在 $O(n)$ 时间内完成逻辑检索。
* **常用模式**：
    * **对撞指针**：一头一尾向中间靠拢（常用于有序数组）。
    * **快慢指针**：同向移动，解决去重、找环或长度压缩。
* **代表题**：
    * `LC 1` Two Sum
    * `LC 26` Remove Duplicates from Sorted Array
    * `LC 15` 3Sum (排序 + 双指针)

---

## 3. 滑动窗口 (Sliding Window)
* **核心逻辑**：处理“连续子数组”或“连续子串”的最优解，避免 $O(n^2)$ 嵌套循环。
* **Google 考察重点**：窗口大小的动态伸缩逻辑。
* **代表题**：
    * `LC 3` Longest Substring Without Repeating Characters
    * `LC 209` Minimum Size Subarray Sum

---

## 4. 前缀和与差分 (Prefix Sum & Difference)
* **核心逻辑**：预处理数组，实现 $O(1)$ 查询任意区间的和。
* **应用场景**：频繁查询区间和、子数组和等于 K。
* **代表题**：
    * `LC 560` Subarray Sum Equals K
    * `LC 303` Range Sum Query - Immutable

---

## 5. 排序与二分查找 (Sorting & Binary Search)
* **核心逻辑**：只要数组**有序**，第一反应必须是 $O(\log n)$ 的二分。
* **Google 考察重点**：在不完全有序的数组（旋转数组、山脉数组）中寻找目标。
* **代表题**：
    * `LC 33` Search in Rotated Sorted Array
    * `LC 34` Find First and Last Position of Element in Sorted Array

---

## 6. 二维数组 / 矩阵 (Matrix / 2D Array)
* **核心逻辑**：一维逻辑的升维应用。
* **典型题型**：螺旋遍历、图像旋转、对角线遍历。
* **代表题**：
    * `LC 48` Rotate Image
    * `LC 54` Spiral Matrix

---

## 💡 刷题 Vibe 指南

1. **先做 Easy 找 Pattern**：每类选 2-3 道 Easy 题，把 Mermaid 逻辑图画出来。
2. **死磕 Medium 拿 Offer**：Google 面试 80% 的题目都在 Medium，特别是 Sliding Window 和 Two Pointers 的结合。
3. **跳过无谓的 Hard**：除非你已经把前 5 类彻底吃透，否则不要在大题量上浪费算力。

# 📑 ROUND 1: Google Array 高频经典题单 (Priority List)

> **Vibe**: 这里的每一道题都是 Google 面试官的“老朋友”。掌握了这些，你就掌握了 Array 的 80% 逻辑。

| 序号 | 题目名称 (LeetCode) | 难度 | 核心算法 / 步法 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Two Sum | Easy | Hash Table | 空间换时间的开山之作 |
| 11 | Container With Most Water | Medium | Two Pointers (对撞) | 贪心思想与双指针结合 |
| 15 | 3Sum | Medium | Sorting + Two Pointers | 降维打击：将 3Sum 转化为 2Sum |
| 26 | Remove Duplicates | Easy | Two Pointers (快慢) | 原地修改，Space O(1) |
| 31 | Next Permutation | Medium | Array / Logic | 考察对字典序的深度理解 |
| 33 | Search in Rotated Array | Medium | Binary Search | 在不完全有序中寻找 $O(\log n)$ |
| 34 | First/Last Position | Medium | Binary Search | 二分查找的边界处理模板 |
| 41 | First Missing Positive | Hard | Cyclic Sort / Hash | 原地哈希的巅峰之作 |
| 42 | Trapping Rain Water | Hard | Two Pointers / Stack | Google 极其经典的高频题 |
| 48 | Rotate Image | Medium | Matrix / Math | 矩阵转置 + 镜像反转 |
| 53 | Maximum Subarray | Medium | DP / Greedy | Kadane 算法，处理连续子数组 |
| 54 | Spiral Matrix | Medium | Matrix / Simulation | 考察代码实现的严密性 |
| 56 | Merge Intervals | Medium | Sorting | 区间问题的处理基石 |
| 66 | Plus One | Easy | Array / Math | 考察进位处理逻辑 |
| 75 | Sort Colors | Medium | Three-way Partition | 荷兰国旗问题，快排核心 |
| 84 | Largest Rectangle | Hard | Monotonic Stack | 单调栈处理“左右边界”问题 |
| 121 | Best Time to Buy Stock | Easy | Greedy / DP | 一次遍历找最值 |
| 238 | Product of Array Except Self | Medium | Prefix/Suffix Product | 不允许用除法的逻辑优化 |
| 283 | Move Zeroes | Easy | Two Pointers (快慢) | 基础的原地修改 |
| 560 | Subarray Sum Equals K | Medium | Prefix Sum + Hash Table | 前缀和与哈希表的完美结合 |

---

## 💡 如何使用这张表？

1. **GitHub Checkbox**: 你可以在每行前面加上 `- [ ]`，做完后改成 `- [x]`。
2. **分类攻克**: 
   - 基础不稳先看 **1, 26, 283**。
   - 突破 $O(\log n)$ 必刷 **33, 34**。
   - 挑战 Google Hard 选 **42, 84**。
  

# 🛡️ ROUND 2: Google Array 专项突击清单 (60 题)

> **Vibe**: 10道题一个阵地。不求数量，求的是对该“步法”的肌肉记忆。

---

## 1. 双指针 (Two Pointers) —— 左右包抄
*核心逻辑：在有序数组或特定约束下，利用两个指针减少搜索空间。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 283 | Move Zeroes | Easy | 快慢指针基础 |
| 125 | Valid Palindrome | Easy | 对撞指针基础 |
| 167 | Two Sum II (Sorted) | Easy | 排序数组找和 |
| 11 | Container With Most Water | Medium | 贪心 + 对撞 |
| 15 | 3Sum | Medium | 排序 + 双指针去重 |
| 16 | 3Sum Closest | Medium | 逼近法 |
| 80 | Remove Duplicates II | Medium | 计数型双指针 |
| 18 | 4Sum | Medium | $O(n^3)$ 逻辑降维 |
| 42 | Trapping Rain Water | Hard | 双指针极值法 |
| 151 | Reverse Words in a String | Medium | 翻转逻辑 |

---

## 2. 滑动窗口 (Sliding Window) —— 动态监控
*核心逻辑：处理“连续”区间，通过维护窗口边界将 $O(n^2)$ 降为 $O(n)$。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 643 | Max Average Subarray I | Easy | 固定长度窗口 |
| 219 | Contains Duplicate II | Easy | 哈希 + 滑动窗口 |
| 3 | Longest Substring... | Medium | 变长窗口核心题 |
| 209 | Min Size Subarray Sum | Medium | 寻找最短区间 |
| 1004 | Max Consecutive Ones III | Medium | 允许翻转的窗口 |
| 567 | Permutation in String | Medium | 频率统计窗口 |
| 438 | Find All Anagrams | Medium | 字符串匹配窗口 |
| 904 | Fruit Into Baskets | Medium | 至多 K 个不同元素 |
| 76 | Min Window Substring | Hard | 滑动窗口终极形态 |
| 992 | Subarrays with K... | Hard | 窗口组合逻辑 |

---

## 3. 前缀和与差分 (Prefix Sum) —— 算力缓存
*核心逻辑：预处理数据，让区间查询变成 $O(1)$。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 303 | Range Sum Query | Easy | 基础缓存思想 |
| 724 | Find Pivot Index | Easy | 左右平衡查找 |
| 1991 | Find Middle Index | Easy | 基础索引搜索 |
| 560 | Subarray Sum Equals K | Medium | 前缀和 + 哈希表 |
| 523 | Continuous Subarray Sum | Medium | 同余定理应用 |
| 974 | Subarray Sums Divisible | Medium | 模运算优化 |
| 238 | Product Except Self | Medium | 前后缀乘积 |
| 525 | Contiguous Array | Medium | 0/1 平衡转换 |
| 1248 | Count Number of Nice... | Medium | 奇数个数转化 |
| 1074 | Submatrix Sum Equals K | Hard | 二维前缀和 |

---

## 4. 二分查找 (Binary Search) —— 降维打击
*核心逻辑：在有序或局部有序空间，利用 $O(\log n)$ 效率收缩范围。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 704 | Binary Search | Easy | 标准模板 |
| 35 | Search Insert Position | Easy | 查找插入点 |
| 33 | Search in Rotated Array | Medium | 旋转数组逻辑 |
| 34 | First/Last Position | Medium | 边界搜索逻辑 |
| 81 | Search in Rotated II | Medium | 含重复元素处理 |
| 153 | Find Min in Rotated | Medium | 寻找拐点 |
| 162 | Find Peak Element | Medium | 局部二分思想 |
| 540 | Single Element... | Medium | 奇偶索引二分 |
| 4 | Median of Two... | Hard | 数组二分终极题 |
| 410 | Split Array Largest Sum | Hard | 答案值域二分 |

---

## 5. 排序与分治 (Sorting & Partition) —— 重新排队
*核心逻辑：通过调整顺序（如快排、堆排思想）获取特定特征。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 88 | Merge Sorted Array | Easy | 逆向填充双指针 |
| 912 | Sort an Array | Medium | 手写快排/归并 |
| 75 | Sort Colors | Medium | 三路快排 (DNF) |
| 215 | Kth Largest Element | Medium | Quickselect 思想 |
| 56 | Merge Intervals | Medium | 区间重叠处理 |
| 57 | Insert Interval | Medium | 区间插入维护 |
| 973 | K Closest Points... | Medium | 堆排序应用 |
| 179 | Largest Number | Medium | 自定义排序规则 |
| 493 | Reverse Pairs | Hard | 归并排序求逆序对 |
| 327 | Count of Range Sum | Hard | 归并 + 前缀和 |

---

## 6. 原地修改与哈希思想 (In-place & Cyclic Sort) —— 内存博弈
*核心逻辑：不使用额外数组，利用下标或符号作为标记位。*

| 序号 | 题目 | 难度 | 重点 |
| :--- | :--- | :--- | :--- |
| 26 | Remove Duplicates | Easy | 原地覆盖 |
| 27 | Remove Element | Easy | 基础交换 |
| 448 | Find All Numbers Disappeared | Easy | 负号标记法 |
| 442 | Find All Duplicates | Medium | 符号标记进阶 |
| 287 | Find the Duplicate | Medium | 快慢指针找环 |
| 189 | Rotate Array | Medium | 翻转法实现旋转 |
| 41 | First Missing Positive | Hard | 桶排序/原地哈希 |
| 31 | Next Permutation | Medium | 字典序变换 |
| 48 | Rotate Image | Medium | 矩阵原地翻转 |
| 289 | Game of Life | Medium | 状态压缩标记 |


# 🚀 ROUND 3: Google 模拟实战与边界审计 (Production Ready)

> **Vibe**: 像 Data Analyst 审计账单一样审计你的代码。不只是写对，而是要写得漂亮、无懈可击。

---

## 1. 混合模式：取消分类 (Random Shuffle)
在前两轮，你按分类刷，大脑会有“暗示”。第三轮必须打破这种心理暗示。
- **操作**：使用 LeetCode 的 `Pick One` 功能，或从你的 60 题清单中随机抽取。
- **目标**：在 **2 分钟内** 识别出题目背后的“马甲”，瞬间反应出该用哪种步法（滑动窗口、双指针、还是哈希表）。

---

## 2. 压力测试：限时白板 (Time-boxed Mock)
Google 的面试通常为 45 分钟，你需要在压力下保持逻辑清晰。
- **操作**：每道 Medium 题限时 **20-25 分钟** 完成。
- **强制流程**：不要直接写代码！
    1. **逻辑审计**：先在文档中用注释写下你的核心思路。
    2. **复杂度分析**：明确标注时间复杂度和空间复杂度。
    3. **实现代码**：最后才开始编写 Python 3 代码。

---

## 3. 边界审计：防御性编程 (Defensive Coding)
这是区分“初级码农”和“Google 工程师”的关键。
- **操作**：写完代码后，必须主动列出并手动走一遍以下 **Test Cases**：
    - [ ] `nums` 为空或长度为 1（边界情况）。
    - [ ] `target` 不在数组中（异常情况）。
    - [ ] 数组中包含重复数字（重复性干扰）。
    - [ ] 数值极大或极小（溢出风险/极端值）。

---

## 4. 优化表达：说出你的逻辑 (Verbalization)
Google 寻找的是能够协作的 Analytical Engineer，而非沉默的写代码机器。
- **操作**：一边写一边自言自语，解释你的决策。
    - *“这里我选择 Hash Table 而不是排序，是因为我需要 O(1) 的查询效率，且空间开销在可控范围内。”*
- **意义**：训练你在压力下依然能进行高质量的逻辑输出。

---

## 🛠️ 第三轮 GitHub 记录模板 (Refactor Record)

建议为每道重点题增加如下记录，作为你逻辑进化的存证：

### LC [题目序号] - [题目名称] (Third Pass)
- **Time to Solve**: 18 mins
- **Initial Thought**: 滑动窗口
- **Optimization**: 发现可以用 Hash Map 记录索引，从而将左边界的移动从 $O(n)$ 优化为 $O(1)$。
- **Edge Cases Checked**: 
    - [x] Empty array / Null input
    - [x] Negative numbers
    - [x] All identical elements
- **Complexity**: Time $O(n)$, Space $O(k)$

---

## 💡 “第三轮心法”

1. **从“做对”到“做优”**：你审计那 70 万资产时，漏掉一个小数点都不行。刷题也是这种心态——**你要像审视对手的漏洞一样审视自己的代码。**
2. **肌肉记忆**：第三轮是为了让你在面对高压环境时（无论是面试还是法庭），哪怕大脑带宽只剩 $30\%$，剩下的 $70\%$ 也能靠生理本能自动运行最正确的逻辑。

# 🕵️ ROUND 4: 聚类扩张与“马甲”识别 (Massive Expansion)

> **Vibe**: 100% 的题目，其实只是 5% 逻辑的无限次重采样。这一轮的目标是：在看到题目的 **10 秒内**，准确报出它的“归类标签”。

---

## 1. 聚类分析法 (Clustering Approach)

不要按顺序刷，要按“逻辑特征”抓取。将 1554 道题聚类为以下三种类型：

### A. 换汤不换药类 (Identity Mapping)
* **特征**：逻辑与母题完全一致，只是背景从“数字求和”变成了“买卖股票”或“分发糖果”。
* **操作**：口述思路，脑补或手写核心伪代码，**直接跳过**，不做无谓的算力浪费。

### B. 约束变体类 (Constraint Variants)
* **特征**：在母题基础上增加了限制条件。
    * *例：从“不重复字符”变成“最多包含 K 个重复字符”。*
* **操作**：**重点攻克**！在 GitHub 中记录该 **Constraint** 是如何改变 `if` 判断逻辑或边界收缩条件的。

### C. 结构转换类 (Structural Shift)
* **特征**：表面是数组，实际考察的是将数组转化为 `Graph` (图) 或 `Tree` (树)。
    * *代表：数组表示的二叉树遍历、跳跃游戏 (Jump Game)。*
* **操作**：识别底层物理结构的变化，建立跨结构的逻辑映射。

---

## 2. 第四轮：10秒审计大法 (10-Sec Audit)

对剩下的 1000 多道题目，执行以下流水线操作：

1. **识别 (Identify)**：它是 6 大核心模式（双指针、滑动窗口、二分等）中的哪一种？
2. **定位 (Locate)**：它和 Round 2 里的哪道母题最像？
3. **差异分析 (Gap Analysis)**：它多了哪些边界条件 (Edge Cases) 或特殊约束？
4. **决策 (Decision)**：
    - **完全一样** $\rightarrow$ 扔掉（标记为已掌握）。
    - **逻辑微调** $\rightarrow$ 快速手速练习，验证 Corner Case。
    - **完全没见过** $\rightarrow$ 升级为“新母题”，加入 Round 2 核心清单。

---

## 🛠️ 第四轮 GitHub 聚类笔记模板 (`The_Gap_Audit.md`)

建议专门建立此文件，记录从母题到变体的进化逻辑：

### 🧩 模式：滑动窗口 (Sliding Window)
- **母题**: `LC 3` (最长不重复子串)
- **变体 1**: `LC 159` (至多两个不同字符) $\rightarrow$ **差异**: 字典/哈希表大小限制为 2。
- **变体 2**: `LC 340` (至多 K 个不同字符) $\rightarrow$ **差异**: 将固定常数替换为变量 K。
- **变体 3**: `LC 424` (替换 K 次后的最长字符) $\rightarrow$ **差异**: 窗口收缩逻辑变为 `(window_size - max_freq) > k`。

### 🧩 模式：双指针 (Two Pointers)
- **母题**: `LC 15` (3Sum)
- **变体**: `LC 18` (4Sum) $\rightarrow$ **差异**: 在 3Sum 基础上多套一层循环，本质逻辑未变，注意剪枝优化。

---

## 💡 “第四轮觉悟”

1. **拒绝算力浪费**：你不需要刷完 1554 题。当你发现连续看 20 道题都能秒出思路时，这一章你就已经彻底**通关**了。
2. **审计思维**：就像你在 **Department 65** 审计账单时，你不是在看每一张纸，而是在找那几个**异常模式 (Anomaly Detection)**。
3. **Mischief Managed**：通过聚类，你把 $O(n)$ 的刷题任务变成了一个 $O(1)$ 的查表操作。

