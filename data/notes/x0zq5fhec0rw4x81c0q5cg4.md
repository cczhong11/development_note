下面按不同场景，对模型在内存中的占用做一个系统化分析，并结合 7B 和 MoE (8×7B，激活 2 experts≈12.9B) 两个例子给出量化估算。

⸻

一、参数存储（Weights）

| 数据类型 | 每参数字节数 | 7B 模型 | MoE(12.9B 激活) |
| - | - | - | - |
| FP32 (32-bit) | 4 bytes | $7\times10^9\times 4\,\mathrm{B} = 28\,\mathrm{GB}$ | $12.9\times10^9\times 4\,\mathrm{B} = 51.6\,\mathrm{GB}$ |
| FP16/BF16 | 2 bytes | $7\times10^9\times 2\,\mathrm{B} = 14\,\mathrm{GB}$ | $12.9\times10^9\times 2\,\mathrm{B} = 25.8\,\mathrm{GB}$ |
| INT8 | 1 byte | $7\times10^9\times 1\,\mathrm{B} = 7\,\mathrm{GB}$ | $12.9\times10^9\times 1\,\mathrm{B} = 12.9\,\mathrm{GB}$ |

	•	FP32 是最直观的精度，但显存开销最大。
	•	FP16/BF16 是大多数训练和推理的常用格式，显存减半。
	•	INT8 量化 推理时可进一步压缩到 1 B/参数。

⸻

二、推理时（Inference）内存

推理内存 $\approx$ 权重存储 + 激活（activations）缓存 + 临时缓冲
	1.	权重存储：如上表所示。
	2.	激活缓存
	•	单层：$L$ 个位置 × 隐层维度 $d$ × 数据类型字节数
	•	整个模型：≈ 模型层数 × 单层激活
	3.	批量大小 $B$ 影响激活总量：总激活 $\approx B \times L \times d \times \#\text{layers} \times \text{bytes}$

示例估算（粗略）：
	•	假设 7B 模型，隐藏维度 $d=4096$，层数 $32$，序列长 $L=2048$，FP16。
	•	单层激活 $\approx 2048\times4096\times2\,\mathrm{B} \approx 16\,\mathrm{MB}$
	•	全模型激活 $\approx 16\,\mathrm{MB}\times32 \approx 512\,\mathrm{MB}$
	•	加上权重 $14\,\mathrm{GB}$，总 $\approx 14.5\,\mathrm{GB}$（批量 $B=1$）

⸻

三、训练时（Training）内存

训练内存 $\approx$
$$\underbrace{\text{权重}}_{W} + \underbrace{\text{梯度}}_{G} + \underbrace{\text{优化器状态}}_{O} + \underbrace{\text{激活}}_{A} + \underbrace{\text{额外缓冲}}_{B}$$

以常用 Adam 为例：
	•	梯度 $G$：与权重同量级，$\approx W$。
	•	优化器状态 $O$：Adam 需要维护一阶矩 $m$ 和二阶矩 $v$，共 $\approx 2W$。
	•	激活 $A$：与推理时相似，但反向传播需缓存更多中间结果，通常 $\approx 2\text{–}3$ 倍推理激活。
	•	额外缓冲 $B$：梯度累积、通信缓冲等，$\approx 0.1\text{–}0.2 \times W$。

总计（粗略）：
$$\text{Total} \approx W + W + 2W + (2\text{–}3)A + 0.1W$$
$$= (4.1\,W) + (2\text{–}3)A$$

示例：7B FP16 训练
	•	$W=14\,\mathrm{GB},\ G=14\,\mathrm{GB},\ O=28\,\mathrm{GB}$ → 权重相关 $\approx 56\,\mathrm{GB}$
	•	激活按前面 $512\,\mathrm{MB}$ 估算，$\times 3 \approx 1.5\,\mathrm{GB}$
	•	总 $\approx 56\,\mathrm{GB} + 1.5\,\mathrm{GB} \approx 57.5\,\mathrm{GB}$

⸻

四、MoE 模型额外开销
	•	路由器（Router）状态：每 token 需计算专家分配，额外激活≈少量矩阵乘法开销。
	•	专家梯度 & 优化器状态：只有激活的专家会反向梯度累积，每步额外状态 $\approx$ 激活参数量的 $3\times W_{\text{active}}$。
	•	切片/通信：分布式训练时，各卡间通信缓冲也会增加数 GB 级别内存。

粗略看，MoE 训练内存 $\approx$ 同规模 Dense 模型的 $1.1\text{–}1.3$ 倍。

⸻

五、量化与分布式优化
	1.	混合精度训练（Mixed-Precision） → FP16/BF16 减半
	2.	Optimizer Sharding（如 ZeRO-1/2/3）
	•	将梯度、优化器状态分片到多卡，单卡实占显存 $\approx$ Dense 模型的 $0.3\text{–}0.6$ 倍
	3.	激活检查点（Activation Checkpointing）
	•	缓存中间激活点减少至线性层数 × 一个激活，大幅省 A

通过以上手段，7B 训练单卡显存可降至 20–30 GB，大模型集群训练也更可行。

⸻


总结
	•	Inference：主要看权重存储 + 激活缓存。
	•	Training：权重相关状态 $\approx 4\times$ 参数大小 + 激活 $2\text{–}3\times$ 推理量。
	•	MoE：激活专家多带来梯度和状态开销，但总体仍低于同总参数 Dense 模型。
	•	优化技术：混合精度、Optimizer Sharding、Checkpointing 是降低显存的关键。

这样，你可以根据不同模型架构、精度、训练/推理场景，快速估算所需内存规模。
