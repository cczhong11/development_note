
## 如何创建训练数据

### 选择训练数据的抽样方法

由于无法获取所有真实世界的数据，我们需要从中抽取一部分进行训练。关键在于如何实现有效的抽样。抽样方法大致分为两类：非概率抽样和随机抽样。

#### 非概率抽样方法

1. **便利抽样**（Convenience Sampling）  
   主要基于数据的可获取性。

2. **雪球抽样**（Snowball Sampling）  
   以已有的样本为基础，逐渐扩大样本范围，类似于深度优先搜索（DFS）算法。

3. **判断抽样**（Judgmental Sampling）  
   由专家根据经验决定抽样方法。

4. **配额抽样**（Quota Sampling）  
   在每个类别中选择固定数量的样本进行调查。

#### 随机抽样方法

1. **简单随机抽样**  
   随机选择数据的一个子集，例如随机选取10%的数据。

2. **分层抽样**（Stratified Sampling）  
   从不同的类别（如A和B）中各选取一定比例，如1%，以确保包含这些类别的数据。

3. **加权抽样**（Weighted Sampling）  
   对每个样本赋予不同的权重，代表其被抽中的可能性。

4. **保留抽样**（Reservoir Sampling）  
   适用于流数据。初始选择K个元素，对于新到的第N个元素，生成1到N之间的随机数。如果这个数在1到K之间，用第N个元素替换对应位置的元素。

5. **重要性抽样**（Importance Sampling）  
   当从一个分布P抽样成本很高时，可以使用一个替代的分布Q进行抽样，然后根据权重调整样本。
