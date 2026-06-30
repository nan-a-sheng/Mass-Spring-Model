# Taichi Cloth Simulation 布料物理仿真
基于 Taichi 实现的经典弹簧-质点布料仿真系统，实现三种欧拉积分求解器对比，GPU 高速并行计算，支持 3D 实时可视化与交互操作。

## ✨ 项目特性
- 三种积分算法：显式欧拉 / 半隐式欧拉 / 隐式欧拉（定点迭代）
- 纯 GPU 并行计算，仿真速度快、性能优异
- 完整布料物理模型：结构弹簧 + 重力 + 阻尼 + 速度钳制防爆炸
- 实时 GUI 控制面板：一键切换算法、暂停、重置
- 可交互 3D 相机：视角旋转、缩放、平移
- 经典布料约束：顶部两角固定，实现自然下垂摆动效果
  
## 📦 环境依赖
Python 3.8+ 与 Taichi 物理引擎
pip install taichi

## 🚀 快速运行
1. 将代码保存为 cloth.py
2. 终端执行：
python cloth.py

## 🎮 操作说明
### 界面控制面板
- Explicit Euler：显式欧拉，精度低、极易发散爆炸（教学对比用）
- Semi-Implicit Euler：半隐式欧拉，稳定且效果自然（默认推荐）
- Implicit Euler：隐式欧拉（3 次定点迭代），高稳定、略带阻尼感
- Pause / Resume：暂停/继续仿真
- Reset Cloth：重置布料初始状态
### 相机操作
- 鼠标右键拖拽：旋转视角
- 鼠标滚轮：缩放场景
- WASD：平移相机
- 
## ⚙️ 核心物理参数
可自由调节，修改即可改变布料质感
- N：布料网格分辨率（默认 20×20）
- mass：质点质量
- dt：物理时间步长
- k_s：弹簧刚度（越大布料越硬）
- k_d：阻尼系数（越大越不容易晃动）
- gravity：重力加速度
- max_velocity：速度上限，防止数值爆炸
  
## 📁 代码结构
- 场定义：位置、速度、受力、固定点、弹簧数据
- 初始化模块：网格位置初始化、弹簧拓扑构建、固定点约束
- 力计算模块：统一重力、阻尼、弹簧弹力计算
- 三大积分求解器：显式 / 半隐式 / 隐式迭代
- 渲染与交互：3D 场景、光照、粒子+线框绘制、GUI 控制
  
## 💡 原理特点
- 所有物理计算合并为少量 Kernel，极致 GPU 效率
- 使用 ti.func 内联计算，减少内核启动开销
- 速度钳制 + 多步迭代，有效避免数值不稳定
- 三种积分器直观展示 数值稳定性差异，适合学习物理仿真

## 效果展示
### 阻尼系数为1
<img width="640" height="662" alt="DyjtnjNk_converted" src="https://github.com/user-attachments/assets/e076943a-2bb6-48e7-830f-f3034a5f6dc8" />
### 阻尼系数为5
<img width="640" height="662" alt="uBX6nfqT_converted" src="https://github.com/user-attachments/assets/b7264b47-be07-4709-963b-b210363e49ea" />

## ✨ 选做拓展功能
### 1. 三类完整弹簧模型
在原生结构弹簧基础上新增：剪切弹簧（防对角拉伸）+弯曲弹簧（抑制过度弯折），布料刚性提升，褶皱与形变更贴近现实织物。
- Structural：横竖相邻质点
- Shear：对角线相邻质点
- Bending：间隔一格的远距离质点
可调参数：`k_shear、k_bend`

### 2. 球体碰撞物理
场景中央生成红色实心球体，布料下落与球体发生碰撞：
- 质点穿入球体后自动被顶回球面；
- 附带简易法向速度阻尼反弹；
可修改 `sphere_pos、sphere_radius` 调整球体位置与大小。

## 效果展示
<img width="800" height="827" alt="7-ezgif com-video-to-gif-converter" src="https://github.com/user-attachments/assets/f392d878-6700-40b9-b38f-67f04bc543ee" />

---
项目用途：计算机图形学、游戏物理、弹簧质点系统学习演示
