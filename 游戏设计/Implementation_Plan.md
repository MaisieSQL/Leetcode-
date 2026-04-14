# 🛠️ 《逻辑荒野》快速原型实现方案 (MVP)

> **目标**：用最少的代码实现“横版过关 + 算法拦截”的核心闭环。

---

## 1. 技术栈选择：Python + Pygame
* **理由**：你已经熟悉 Python 3，Pygame 处理 2D 碰撞和精灵（Sprites）非常直观。
* **风格**：8-bit 像素风（你可以直接用方块代表林克，圆圈代表怪物）。

---

## 2. 核心架构设计

### A. 游戏循环 (The Game Loop)
```python
while running:
    # 1. 处理输入 (左右跳跃)
    # 2. 更新状态 (物理引擎、碰撞检测)
    # 3. 检查冲突 (Check for Anomaly)
    if player.collides_with(enemy):
        trigger_algorithm_challenge(enemy.type)
    # 4. 渲染画面
