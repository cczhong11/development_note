下面给出常用的 FLOPs（Floating-Point Operations）估算方法，并基于此对 7B 模型和 MoE (8×7B，激活 2 experts) 模型的训练成本做个对比。

⸻

1. 常用 FLOPs 估算公式
	1.	训练（Training）
总 FLOPs ≈ 6 × 参数量 $N_{p}$ × 训练 Tokens 数 $N_{t}$

	•	这里的“6”来自一次前向（≈2）+一次反向（≈4）的粗略计数。
	2.	推理（Inference）
	•	单 token FLOPs ≈ 2 × 参数量 $N_{p}$
	•	若考虑序列长度 $L$，则单序列 FLOPs ≈ $2 × N_{p} × L$

⸻

2. GPT-3 基准 (175B 参数，≈300B 训练 Tokens)

$$\mathrm{FLOPs}_{\text{GPT-3}} \approx 6 \times 175\times10^9 \times 300\times10^9$$
$$= 3.15\times10^{23}$$
与公开报道的 $3.14\times10^{23}$ 基本一致。

⸻

3. 7B 模型训练 FLOPs
	•	参数量：$7\times10^9$
	•	训练 Tokens：同样假设 $300\times10^9$

$$\mathrm{FLOPs}_{7B} \approx 6 \times 7\times10^9 \times 300\times10^9$$
$$= 1.26\times10^{22}$$

相当于 GPT-3 的 $\tfrac{1.26\times10^{22}}{3.15\times10^{23}}\approx 4\%$。

⸻

4. MoE 模型 (8×7B，2 experts 激活)
	•	总参数量：约 $46.7\times10^9$
	•	每次激活有效参数：$2 \times 7\times10^9 = 14\times10^9$，实际要考虑 shared parameters，取 $12.9\times10^9$
	•	训练 Tokens：同样假设 $300\times10^9$

$$\mathrm{FLOPs}_{\text{MoE}} \approx 6 \times 12.9\times10^9 \times 300\times10^9$$
$$= 2.322\times10^{22}$$

相当于 GPT-3 的 $\tfrac{2.32\times10^{22}}{3.15\times10^{23}}\approx 7.4\%$。

⸻

5. 小结
	•	7B 模型 训练约需 $1.26\times10^{22}$ FLOPs（≈4% GPT-3）
	•	MoE 8×7B (2 experts) 训练约需 $2.32\times10^{22}$ FLOPs（≈7.4% GPT-3）
	•	推理成本可按「单 token $\approx 2\times$ 参数量」快速估算，若序列长 $L$ 则乘以 $L$。
