# 训练计划算法-模块-负荷计量算法

## 1. 目标

将 SessionLoadTarget 合理分配至各动作类别。

---

## 2. 输入

- SessionLoadTarget
- StimulusWeightMatrix
- 可用动作集合

---

## 3. 基本公式

## 3. 基本公式 (Dual-Track)

```
Target_Metabolic = Σ(Weight × Reps × D_adjust)
Target_Neural    = Σ(Weight × Reps × IntensityFactor²)
```

---

## 4. 分配流程

### Step 1：按刺激权重分配

例如增肌模型（侧重 Metabolic）：
- 主复合动作：40% Load
  - 需同时监控 Neural Load 消耗，防止过早力竭。

---

### Step 2：按运动模式分配

确保：
- 推 / 拉 / 蹲 / 髋 / 核心
- 均衡覆盖（若无特别目标）

---

### Step 3：转化为 组数 / 次数 / RIR

**必需引入 RIR (Reps In Reserve) 标尺：**

- **增肌 (Hypertrophy)**:
  - Reps: 8-12
  - RIR: 1-2 (接近力竭)
  - Effort Coefficient: 0.9

- **力量 (Strength)**:
  - Reps: 3-5
  - RIR: 2-3 (保证技术动作)
  - Effort Coefficient: 0.8

- **代谢/耐力 (Endurance)**:
  - Reps: 15+
  - RIR: 0-1 (完全力竭)
  - Effort Coefficient: 1.0

**计算公式：**
```
Sets = TargetLoad / (AvgWeight × Reps × Effort)
```

---

### Step 4：密度与强度校正

**关键约束：高密度 = 低强度上限**

若 `Density > Threshold` (如组间歇 < 60s):
- 强制 `MaxIntensity < 75% 1RM`
- 若原计划强度过高，自动降重加次。


---

## 5. 校准机制

若 Σ(ActionLoad_i) 偏差 > 5%

优先调整：
1. 组数
2. 强度
3. 休息时间

---

## 6. 输出

- 动作负荷分配表
- 结构合理性评分
- 最终 SessionLoad
