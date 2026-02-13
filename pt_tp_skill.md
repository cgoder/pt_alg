
---
name: personal-training-plan-engine
description: Generates personalized training plans based on user goals and fitness levels.

---

# 训练计划生成引擎

## 一、技能目标

基于用户输入条件，
自动生成符合生理原则与算法原则的训练计划。

---

## 二、输入参数

- 主目标（增肌 / 减脂 / 力量 / 健康）
- 周训练频率
- 单次训练时长
- 器械条件
- 当前能力指数（可选）
- 反馈数据（可选）

---

## 三、处理流程

1. 解析目标 → 生成 GoalVector
2. 选择刺激模型
3. 计算 WeeklyLoadTarget
4. 计算 SessionLoadTarget
5. 负荷预算分配
6. 动作筛选与结构生成
7. 负荷校准
8. 输出训练计划
9. 若有反馈 → 调用疲劳预测模块 → 修正下周期负荷

---

## 四、输出内容

- 周训练结构
- 单节课动作清单
- 每动作组数 / 次数 / 强度
- 休息时间
- SessionLoad
- WeeklyLoad
- 当前周期位置
- 下周负荷建议

---

## 五、核心约束

- 不得突破 MRV
- 必须存在周期波动
- 必须可计算负荷
- 默认均衡刺激
- 动作质量优先

---

## 六、自适应规则

- RPE 超标 → 自动减负
- 连续低疲劳 → 自动加负
- 疲劳超界 → 强制 Deload

---

## 七、技能定位

该技能不是模板排课器，
而是基于负荷控制与疲劳管理的自适应训练决策系统。
