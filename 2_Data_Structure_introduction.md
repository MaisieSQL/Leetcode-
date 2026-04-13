# 🛡️ 算法基准库：Array, String & Hash Table

> **Vibe**: 追求 $O(1)$ 的极致检索，拒绝 $O(n^2)$ 的冗余损耗。

---
# 🗺️ 数据结构全景地图 (Comprehensive Data Structures Map)

> **原则**：没有最好的数据结构，只有最适合当前问题的“算力/内存”平衡点。

---

## 一、 线性数据结构 (Linear Data Structures)
*元素排列在逻辑上的直线序列中。*

### 1. 数组 (Array / Static Array)
- **定义**: 内存中连续存储的同类型元素。
- **复杂度**: 
  - 访问: $O(1)$ (通过索引直达)
  - 插入/删除: $O(n)$ (涉及元素移动)
- **Vibe**: 基础底座。如果知道索引，它是最快的。

### 2. 链表 (Linked List)
- **定义**: 节点组成，每个节点包含数据和指向下一个节点的指针。
- **分类**: 单链表、双链表、循环链表。
- **复杂度**:
  - 访问: $O(n)$
  - 插入/删除: $O(1)$ (只需修改指针)
- **Vibe**: 灵活的变形金刚，适合频繁增删。

### 3. 栈 (Stack)
- **定义**: 后进先出 (LIFO)。
- **操作**: `push`, `pop`, `peek`。
- **场景**: 递归调用、括号匹配、浏览器后退。
- **Vibe**: 像一叠盘子。

### 4. 队列 (Queue)
- **定义**: 先进先出 (FIFO)。
- **操作**: `enqueue`, `dequeue`。
- **场景**: 任务调度、宽度优先搜索 (BFS)。
- **特殊类型**: 双端队列 (Deque), 优先队列 (Priority Queue/Heap)。

---

## 二、 散列数据结构 (Hashing)

### 5. 哈希表 (Hash Table / Dictionary / Set)
- **定义**: 通过 Key 计算 Hash 值，直接映射到内存位置。
- **复杂度**: 查找、插入、删除平均均为 **$O(1)$**。
- **场景**: 几乎所有需要“快速查找”的问题。
- **Vibe**: 瞬移，跳过所有搜索过程。

---

## 三、 非线性数据结构 (Non-linear Data Structures)
*元素之间存在一对多或多对多的复杂关系。*

### 6. 树 (Tree)
- **定义**: 层次结构，一个根节点下挂载多个子节点。
- **核心类型**:
  - **二叉树 (Binary Tree)**: 每个节点最多两个子节点。
  - **二叉搜索树 (BST)**: 左 < 根 < 右，查找效率高。
  - **堆 (Heap)**: 特殊的完全二叉树。大顶堆（找最大项）或小顶堆（找最小项）。
  - **字典树 (Trie)**: 专门处理字符串前缀。
- **Vibe**: 决策树与组织架构，逻辑的分叉。

### 7. 图 (Graph)
- **定义**: 节点 (Vertex) 和 边 (Edge) 组成。
- **分类**: 有向图 vs 无向图；加权图 vs 权值图。
- **算法**: DFS, BFS, 最短路径 (Dijkstra)。
- **场景**: 社交网络、地图导航。
- **Vibe**: 万物互联的网。

---

## 四、 抽象数据结构汇总表 (Cheat Sheet)

| 数据结构 | 查找 | 插入 | 优势 | 劣势 |
| :--- | :--- | :--- | :--- | :--- |
| **Array** | $O(1)$ | $O(n)$ | 随机访问快 | 长度固定或扩容开销大 |
| **Linked List** | $O(n)$ | $O(1)$ | 插入删除极快 | 无法随机访问 |
| **Hash Table** | $O(1)$ | $O(1)$ | 极速检索 | 消耗额外空间，Hash冲突 |
| **BST (Balanced)** | $O(\log n)$ | $O(\log n)$ | 有序且高效 | 实现复杂 |
| **Stack/Queue** | $O(n)$ | $O(1)$ | 逻辑受限但安全 | 无法访问中间元素 |

---

## 🛠️ Python 3 实现工具箱

```python
# Array: 使用 list
arr = [1, 2, 3]

# Linked List: 需要自定义类
class Node:
    def __init__(self, val): self.val = val; self.next = None

# Hash Table: 使用 dict 或 set
mapping = {"key": "value"}
unique_items = {1, 2, 3}

# Stack/Queue: 推荐 collections.deque
from collections import deque
stack_or_queue = deque()

# Heap: 使用 heapq (默认小顶堆)
import heapq
heap = []; heapq.heappush(heap, 10)
```

---

## 1. 数组与字符串 (Array & String)

### 核心定义
- **Array**: 内存中连续存储的元素序列。支持随机访问。
- **String**: 字符序列。在 Python 中，字符串是**不可变对象 (Immutable)**。每次修改都会创建新对象。

### Google 考察频率：⭐⭐⭐⭐⭐
Google 极其看重基础。常考题型包括：
- **双指针 (Two Pointers)**: 解决反转、求和、去重。
- **滑动窗口 (Sliding Window)**: 解决子串、子数组最值问题。

### Python 3 常用函数
- `len(arr)`: 获取长度。
- `arr.append(x)` / `arr.pop()`: 尾部操作 ($O(1)$)。
- `arr.sort()`: 原地排序 ($O(n \log n)$)。
- `s.split(sep)`: 字符串切分。
- `"".join(list)`: 将列表拼回字符串（比 `+` 拼接更省算力）。
- `s[::-1]`: 字符串/列表翻转。

---

## 2. 哈希表 (Hash Table / Dictionary)

### 核心定义
- **Hash Table**: 通过哈希函数将 Key 映射到内存地址，实现**瞬时查找**。
- **本质**: 空间换时间的终极武器。

### Google 考察频率：⭐⭐⭐⭐⭐ (必考)
这是 Google 面试的核心。如果你能用 Hash Table 将 $O(n^2)$ 优化到 $O(n)$，面试就稳了一半。

### Python 3 常用函数
- `d = {}`: 初始化字典。
- `d[key] = value`: 插入或更新 ($O(1)$)。
- `if key in d:`: 检查是否存在 ($O(1)$)。
- `d.get(key, default)`: 安全取值，防止报错。
- `collections.Counter(arr)`: 自动统计频率（面试神技）。

---

## 3. 经典实战：Two Sum 逻辑审计

### 题目简述
给定一个数组 `nums` 和一个目标值 `target`，找出数组中和为目标值的两个数的索引。

### 优化思路 (Hash Map 策略)
不要进行两次循环（暴力枚举），而是遍历一次，同时记下“我见过了谁”。

### 伪代码 (Pseudo-code)
```python
# 初始化一个空的哈希表: prev_map {数值: 索引}
# 遍历数组中的每一个数字 (current_val) 及其索引 (i):
#     计算我们需要的差值: diff = target - current_val
#     
#     如果 diff 已经在 prev_map 中:
#         返回 [prev_map[diff], i]  # 找到匹配，输出索引
#     
#     否则:
#         将当前数字存入哈希表: prev_map[current_val] = i
#
# 如果循环结束未找到，返回空 (理论上题目保证有解)
```
