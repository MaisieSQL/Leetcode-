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
