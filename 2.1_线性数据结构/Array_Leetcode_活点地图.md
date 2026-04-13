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

# 📑 Google Array 高频经典题单 (Priority List)

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
