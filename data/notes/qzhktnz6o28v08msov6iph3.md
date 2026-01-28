下面是一份针对《AI Engineering》第 3 章“Evaluation Methodology”中“评价（Evaluation）”部分的结构化读书笔记，已补充更多常用指标、方法和实践思路，并突出Fact Check 与领域知识的重要性，供参考和进一步完善。

⸻

1. 评价的挑战
	•	开放性输出：Foundation Models 生成的文本、图像等都是开放式的，可能有无限多种合理答案，缺乏确定的“标准答案”来做比对。
	•	领域差异：不同应用（法律、医学、金融）需要不同领域知识，模型在某些领域可能表现优秀，在另一些领域则“术业有专攻”难以评估。
	•	事实正确性（Fact Check）：生成内容往往包含未经验证的信息或“幻觉”（hallucination），需要自动和人工结合的事实校验机制。
	•	人类偏好：用户对“好回答”的定义极度主观，需要通过人类反馈或代理模型来捕捉偏好。

⸻

2. 基于概率的核心指标

| 指标 | 含义 | 公式（简化） | 注释 |
| - | - | - | - |
| 熵（Entropy） | 随机变量不确定度；字符/token 所携带的信息量，通常以比特（bits）为单位 | $H(P) = -\sum_x P(x)\log_2 P(x)$ | 最高熵发生在所有事件等概率时，信息量最大 |
| 交叉熵（Cross-Entropy） | 真实分布 $P$ 与模型分布 $Q$ 的平均差异 | $H(P,Q) = -\sum_x P(x)\log_2 Q(x)$ | 等于真实熵 $H(P)$ 与 KL 散度 $D_{KL}(P\|Q)$ 之和 |
| KL 散度 | 衡量 $Q$ 相对于 $P$ 的偏差 | $D_{KL}(P\|Q) = \sum_x P(x)\log_2\frac{P(x)}{Q(x)}$ | 非对称，非度量；常作为损失的一部分 |
| 困惑度（Perplexity） | 语言建模上的“疑惑”程度；越低越好 | $\mathrm{PPL} = 2^{H(P,Q)}$ | 直观：模型相当于在词表上平均挑选 $\mathrm{PPL}$ 个 token |
| 比特/字符（BPC） | 以字符为单位的交叉熵度量 | $\mathrm{BPC} = \frac{H(P,Q)}{\log_2 e}$ | 与 PPL 对应，适合字符级模型 |

示例：若训练集真实熵 $H(P)=3$ bits、模型交叉熵 $H(P,Q)=3.5$ bits，则
	•	模型困惑度 $\mathrm{PPL} \approx 2^{3.5} \approx 11.3$
	•	说明生成每个 token 时相当于在 $11.3$ 个最可能的选项里平均挑 $1$ 个

⸻

3. 精确评价（Exact Evaluation）
	1.	功能正确性（Functional Correctness）
	•	适用于有“标准答案”的任务：数学题、编程题、用户意图分类。
	•	常用指标：Accuracy、Precision/Recall、F1-Score

| 指标 | 公式 | 说明 |
| - | - | - |
| Accuracy | $\dfrac{TP+TN}{TP+TN+FP+FN}$ | 整体正确比例 |
| Precision | $\dfrac{TP}{TP+FP}$ | 正类预测的准确性 |
| Recall | $\dfrac{TP}{TP+FN}$ | 正类召回率 |
| F1-Score | $2\cdot\dfrac{\mathrm{Precision}\cdot\mathrm{Recall}}{\mathrm{Precision}+\mathrm{Recall}}$ | 准确率与召回的调和平均 |
	2.	参考对齐度（Similarity to References）
	•	机器翻译：BLEU、TER
	•	文本摘要：ROUGE-L、BERTScore
	•	对话生成：Meteor、CIDEr 等
	3.	嵌入相似度（Embedding Similarity）
	•	将生成文本与多参考答案编码为向量，计算余弦相似度
	•	适合开放式回答任务：问答、对话

$$\cos(\theta) = \frac{\langle \mathbf{a},\, \mathbf{b} \rangle}{\lVert \mathbf{a} \rVert\, \lVert \mathbf{b} \rVert}$$

注意：以上方法依赖“参考答案集”，难以囊括所有合理输出。

⸻

4. 人工与代理评判（Human & AI-as-Judge）
	•	人工评判（Human Evaluation）
	•	维度：正确性、流畅度、连贯性、有用性、安全性
	•	打分方式：5 分制、A/B 测试、对比打分
	•	成本：耗时高、主观性大，需要严格设计问卷与评分指南
	•	AI 作为评判（AI-as-Judge）
	•	使用更强/专门的 LLM 对输出打分（如 Claude3、GPT-4）
	•	优点：成本低，速度快；可快速迭代
	•	缺陷：自身存在偏差，可能放大生成模型的错误

⸻

5. 比较评价（Comparative Evaluation）
	•	多个模型/版本同时生成输出，评测者只需选择更好的一条
	•	优势：相对简单，不要求给定“理想参考”；更契合 A/B 场景
	•	局限：对比集规模决定统计功效，需足够样本量

⸻

6. 事实校验与领域知识
	•	事实校验（Fact Check）
	•	集成外部知识源：检索实时百科、新闻、数据库
	•	自动校验：使用 QA 模型判断生成文本中的声称是否在可信文档中出现
	•	领域知识
	•	尽量使用专门 finetune/检索过的领域数据：医学、金融、法律
	•	引入领域专家反馈；在关键环节使用人机协同审校

⸻

7. 多维度指标监控与流水线设计
	1.	单次请求指标
	•	质量（Precision/Recall、困惑度）、安全（Toxicity）、连贯度
	2.	系统级指标
	•	响应时延（TTFT、TPOT）、成本（每请求 Token 数×API 单价）、可用性
	3.	用户体验指标
	•	满意度（CSAT）、留存率、任务完成率

流水线示例
	1.	离线评测：模型选择 → 离线基准测试（自动 & 人工）
	2.	灰度部署：少量真实流量，监控质量与安全警报
	3.	全量上线：开放更多反馈渠道，持续迭代（见第 10 章“User Feedback”）

⸻

8. 小结与实践建议
	•	指标组合：没有单一万能指标，需结合自动化与人工评估。
	•	分层评估：离线→灰度→在线，逐步放大流量与评估力度。
	•	领域对齐：在关键场景中引入领域专家与知识检索，降低幻觉风险。
	•	持续监控：质量、成本与使用行为多维度跟踪，快速定位回退或升级时机。
