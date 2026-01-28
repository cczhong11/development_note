

# 🧠 大语言模型中的 Sampling 与 Logit 总结

## 1. 什么是 Logits？

- 模型输出的“原始分数”向量
- 大小 = 词表大小（例如 50,000）
- 每个值代表某个词的“倾向性分数”
- 是 softmax 的输入

---

## 2. softmax 的作用

将 logits 转换为概率分布：

```math
softmax(logits_i) = exp(logits_i) / ∑_j exp(logits_j)
````

* logits 越大，softmax 概率越高
* 输出概率和为 1


##  3. Sampling 策略

### 🎯 Top-k Sampling（固定数量）

* 只保留前 k 个最高概率的词
* 将其重新归一化为概率，再随机采样一个词

✅ 控制强，稳定
❗不够灵活，不自适应

---

### 🎯 Top-p Sampling（Nucleus Sampling）

* 累加概率，直到总和 ≥ p（如 0.9）
* 在这组词中重新归一化并采样

✅ 自适应，语言更自然
❗不稳定，p 太高可能引入低质量词

---

## 4. Temperature（温度）

控制 logits 在 softmax 之前的缩放：

```python
probs = softmax(logits / T)
```

* T < 1：概率分布变尖锐 → 更“自信”
* T = 1：原始分布
* T > 1：分布更平滑 → 更“创造性”

---

## 🔁 小复习口诀：

```
Top-k → 固定选前 k 个  
Top-p → 累加到 p 就停  

T 小 → 冷静果断  
T 大 → 放飞自由
```

---

## 推荐参数（实践中常用）

| 策略        | 常用值    | 效果描述           |
| ----------- | --------- | ------------------ |
| Temperature | 0.7–1.0   | 平衡创造力与稳定性 |
| Top-k       | 40–100    | 控制最大选择范围   |
| Top-p       | 0.85–0.95 | 自适应生成流畅性   |
