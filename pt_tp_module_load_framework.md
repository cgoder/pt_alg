# 训练计划算法-模块-负荷计量框架

> 基于`FITTVP`运动处方原则，以`VIDF`为核心的可计算的负荷建模框架

- FITTVP：Frequency, Intensity, Time, Type, Volume, Progression

| 维度 | 含义        |
| ---- | ----------- |
| F    | 频率   |
| I    | 强度   |
| T    | 时间        |
| T    | 类型        |
| V    | 容量      |
| P    | 进阶 |


- VIDF：Volume, Intensity, Density, Frequency

| 变量 | 作用         |
| ---- | ------------ |
| V    | 机械总量     |
| I    | 单位刺激强度 |
| D    | 时间压缩度   |
| F    | 时间分布     |


------

# 一、核心目标

解决三个问题：

1. 如何定义单动作负荷？
2. 如何定义单节课负荷？
3. 如何定义周负荷与周期波动？

------

# 二、统一变量体系

根据宪法 D5 原则，我们只允许四个核心变量：

| 变量   | 符号 | 含义         |
| ------ | ---- | ------------ |
| 训练量 | V    | 有效重复总量 |
| 强度   | I    | 相对负荷强度 |
| 密度   | D    | 单位时间刺激 |
| 频率   | F    | 周内出现次数 |

------

# 三、第一步：定义单动作负荷 (Dual-Track Load)

## 1. 代谢负荷 (Metabolic Load - Volume)
侧重：肌肉生长、耐力、消耗。

```
MetabolicLoad = Weight × Reps
```

## 2. 神经负荷 (Neural Load - CNS Fatigue)
侧重：最大力量、爆发力、神经疲劳。

```
NeuralLoad = Weight × Reps × IntensityFactor²
```
其中 `IntensityFactor²` (或 $e^{Intensity}$) 使得大重量的权重呈指数级上升。
例如：
- 100kg × 1 reps @ 100% 1RM:
  - Metabolic = 100
  - Neural = 100 × 1² = 100
- 50kg × 20 reps @ 50% 1RM:
  - Metabolic = 1000
  - Neural = 1000 × 0.25 = 250

**结论**：高次数训练产生巨大代谢压力，但神经压力相对较小；极限重量反之。系统需分别监控两者。


------

## 徒手 / 时间型动作

徒手没有外部重量。

我们转化为：

```
ActionLoad = 体重 × 难度系数 × 有效时间
```

难度系数可以定义为：

- 双侧标准 = 1.0
- 单侧 = 1.5
- 爆发 = 1.3
- 慢速离心 = 1.2
- 支撑型 = 0.8~1.1

这使徒手动作也可计算。

------

# 四、统一“有效训练量”定义

定义：

```
V = Σ(有效重复次数)
```

对于时间型动作：

```
有效重复次数 = 时间 ÷ 标准节奏单位
```

例如：

- 标准1次 = 2秒
- 30秒动作 ≈ 15次

------

# 五、定义强度 I 与 RIR

强度必须统一到 0~1 区间，并结合 RIR (Reps In Reserve)。

### 1. 绝对强度 (Absolute Intensity)
```
I_abs = 实际负荷 / 估算1RM
```

### 2. 相对强度 (Relative Intensity / Effort)
引入 RIR 修正：
```
Effort = 1 / (RIR + 1)
```
- RIR 0 (力竭) → Effort 1.0
- RIR 4 (保留4次) → Effort 0.2

**实际强度系数** = `I_abs` × `Effort`

------

# 六、定义密度 D 与 强度约束

定义：训练密度 = 单位时间内完成的有效训练量。

```
D = 总有效训练量 / 总训练时间
```

单位：`有效重复 / 分钟`

### 密度-强度约束 (D-I Constraint)
**高密度必须牺牲强度。** 禁止同时要求高密度与高强度。

```
MaxAllowedIntensity = 1 / (1 + k × D)
```
若密度极高（如 HIIT），则强制限制最大强度（如 < 60% 1RM），防止安全事故。

------

# 七、单节课负荷公式

现在可以定义：

```
Session_MetabolicLoad = Σ(MetabolicLoad_i) × D_adjust
Session_NeuralLoad = Σ(NeuralLoad_i)
```

其中：
```
D_adjust = 1 + log(D)  // 密度对代谢压力的放大是对数的
```

------

# 八、周负荷

```
WeeklyLoad = Σ(SessionLoad_i)
```

------

# 九、周期波动函数

我们引入：

```
Load(t) = BaseLoad × PeriodFunction(t)
```

例如 4 周模型：

| 周   | 系数 |
| ---- | ---- |
| 1    | 1.0  |
| 2    | 1.1  |
| 3    | 1.2  |
| 4    | 0.7  |

这是最基础版本。

------

# 十、恢复边界控制

定义恢复上限：

```
MRV = k × 当前能力指数
```

能力指数可简单定义为：

```
能力指数 = 平均周负荷（过去4周）
```

控制条件：

```
WeeklyLoad < MRV
```

否则自动触发：

- 降低强度
- 或降低密度
- 或触发Deload

------

# 十一、最终结构

```mermaid
ActionLoad → SessionLoad → WeeklyLoad → PeriodLoad
```

每个层级都可计算。

- 可比较不同课程
- 可预测疲劳趋势
- 可控制递增幅度
- 可设计自适应修正