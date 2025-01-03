

## 类别不平衡问题及解决方法

### 问题描述

- **检测困难**：当某一类别数据较少时，模型难以检测到这个小众类别。
- **倾向于多数类别**：模型倾向于将样本标注为数量较多的类别。
- **错误估计增加**：由于类别不平衡，可能导致更高的错误估计。

### 解决策略

#### 选择正确的指标

- **准确度**：真正例 / (真正例 + 假正例) Precision
- **召回率**：真正例 / (真正例 + 假反例) Recall
- **ROC曲线**：用于二元分类，通过准确度和召回率判断模型效果。

#### 数据层面的处理

- **过采样**：复制少数类别的数据，使得数据更平衡。适用于少数类别数据较少的情况。

#### 算法层面的调整

- **loss Function**：在损失函数中对不同类别的结果赋予不同的权重，实现类别平衡损失。
- **类别分布的调整**：针对每个类别做适当的概率分布调整。