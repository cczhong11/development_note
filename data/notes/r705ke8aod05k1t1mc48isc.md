

3.8 Test & Compute（测试与评分）
	•	多候选生成
	•	Beam Search：保持 k 条最优路径（候选），直到生成结束，再从中选取分数最高的一条。
	•	Fixed-# Sampling（固定数目采样）：先生成 N 条不同的完整响应（可用温度和 Top-k/Top-p 控制多样性），然后对这 N 条依次打分，选出最高者。
	•	Score Function（评分函数）
	•	语言模型概率：直接用模型自身给出的对数概率和（log-likelihood）。
	•	任务指标：如 BLEU/ROUGE/F1、实体准确率、结构合法性检测器等。
	•	外部评估：接入专门的质量判别模型（如奖励模型、分类器）对每个候选做评分。
	•	Binary Search?
	•	有时也会对候选数 N 做“二分”式尝试：先小 N，若评分差距不明显再翻倍，直到满意为止，从而在效率与质量之间做平衡。

⸻

3.9 结构化输出（Structured Output）生成技巧
	1.	Prompt 指令化
	•	在 Prompt 中明确告诉模型输出格式：

请以 JSON 格式回答：
{
  "name": "...",
  "age": ...,
  "skills": ["...", "..."]
}


	•	常配合示例（few-shot）或模板（template）一起使用。

	2.	后处理脚本（Post-Processing）
	•	对模型自由文本输出做解析、校验与修正：
	•	用正则或 JSON/YAML 解析器检查格式。
	•	若解析失败，自动补齐括号、逗号，或按规则重构。
	3.	约束解码（Constrained Decoding）与采样过滤
	•	Logit Masking：在每一步采样前，把不合法 token 的 logits 置为 −∞，确保生成过程只沿合法文法走。
	•	Sampling + Filter：先按常规采样生成一批候选，再用校验函数（validator）去除不符合结构的结果，仅保留合法的。
	•	结合动态禁止词表（dynamic banlist）和允许词表，实时控制输出空间。
