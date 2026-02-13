# 训练计划算法-规则引擎结构

## 0. 设计目标

构建一个可计算、可扩展、可自适应的训练计划生成引擎。
该引擎必须遵守：

- 算法的原则
- 算法的逻辑
- 算法的架构

以及，
- 算法的负荷计量框架


---

## 1. 引擎总体流程
```mermaid
Input
 ↓
目标解析器
 ↓
刺激模型选择器
 ↓
负荷目标计算器
 ↓
动作选择器
 ↓
结构生成器
 ↓
负荷校准器
 ↓
输出 Session
```

---

## 2. 模块结构说明

### 2.1 目标解析器（Goal Parser）

输入：
- 主目标
- 周训练频率
- 单次训练时长
- 器械条件
- 当前能力指数

输出：
- GoalVector
- StimulusWeightMatrix
- WeeklyLoadTarget

---

### 2.2 刺激模型选择器（Stimulus Model Selector）

根据 GoalVector 选择：

- 增肌模型
- 力量模型
- 减脂模型
- 控制模型

输出：
- 变量权重（Volume / Intensity / Density 权重比例）
- 推荐结构模板

---

### 2.3 负荷目标计算器（Load Target Engine）

计算：

WeeklyLoadTarget
SessionLoadTarget

约束：
- MEV < WeeklyLoad < MRV
- 周期波动函数修正

---

### 2.4 动作数据库（Exercise Database）

每个动作必须结构化：

{
  name,
  movement_pattern,
  primary_muscle,
  equipment_type,
  difficulty_level,
  intensity_range,
  density_factor,
  risk_factor
}

---

### 2.5 动作选择器（Exercise Selector）

分三层规则：

1. 硬约束规则
   - 器械匹配
   - 时间限制
   - 风险限制
   - 不超负荷

2. 结构规则
   - 复合动作优先
   - 运动模式均衡
   - 主次分层

3. 优化规则
   - 覆盖最大化
   - 风险最小化
   - 重复度惩罚

---

### 2.6 结构生成器（Session Builder）

逻辑：

1. 分配负荷预算
2. 按刺激权重分配到动作类别
3. 计算组数 / 次数 / 强度
4. 设定休息时间（影响密度）

---

### 2.7 负荷校准器（Load Calibrator）

校验：

实际负荷 ≈ 目标负荷（误差 < 5%）

若偏差：
- 调整组数
- 调整强度
- 调整休息时间

---

## 3. 规则优先级
```mermaid
宪法原则
 > 生理边界
  > 硬约束
   > 结构规则
    > 优化规则
```

---

## 4. 输出格式
```json
Session = {
  动作列表,
  组数,
  次数,
  强度,
  休息时间,
  SessionLoad,
  刺激模型说明
}
```