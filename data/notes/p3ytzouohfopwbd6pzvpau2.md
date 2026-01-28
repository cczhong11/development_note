
⸻

3.6 Scaling Laws（扩展）
	•	核心结论（Kaplan et al., 2020）：
模型性能（一般用交叉熵损失 $L$ 表示）与模型参数量 $N$、训练 Token 数量 $D$ 以及总计算量 $C$ 之间满足幂律关系：
$$L(N, D) \approx L_0 + \left(\frac{N_c}{N}\right)^{\alpha_N} + \left(\frac{D_c}{D}\right)^{\alpha_D}$$
	•	典型指标：$\alpha_N \approx 0.076$、$\alpha_D \approx 0.095$
	•	计算最优路径：给定总 FLOPs $C$，模型规模与数据规模的配比有最优比率。
	•	瓶颈：数据稀缺
	•	当互联网抓取的公开语料（如 C4/Common Crawl）基本用尽时，继续扩大模型规模（$N$）所带来的收益会因为数据质量与多样性不足而快速递减。
	•	解决思路：
	1.	引入高质量、领域特定语料（多模态、代码、专业文档）
	2.	合成数据（Synthetic Data Generation）和数据增强
	3.	更高效的算法：微调现有大模型，而不是无限制预训练

⸻

3.7 Pre-Train → Post-Train 流程
	1.	预训练（Pre-Training）
	•	自监督目标：自回归（GPT）或双向 MLM（BERT）
	•	大规模数据（C4、OpenWebText、GitHub、图片+文本等）
	2.	监督微调（Supervised Fine-Tuning, SFT）
	•	基于标注好的指令—响应对（Instruction–Response Pairs），如 FLAN、Alpaca、ChatGPT 数据集
	•	目标：最小化交叉熵
$$\mathcal{L}{\text{SFT}}(\theta) = -\mathbb{E}{(x,y)\sim\mathcal{D}}\sum_{t}\log\; p_\theta(y_t\mid y_{<t},x)$$
	3.	偏好微调（Preference Fine-Tuning）
	•	RLHF（Reinforcement Learning from Human Feedback）
	1.	收集偏好对：给定同一指令 $x$，生成多条响应 $\{y_i\}$，由人类标注“更好”/“较差”
	2.	训练奖励模型（Reward Model, $r_\phi$）：
$$\max_\phi \sum_{(y_A\succ y_B)}\log \sigma\bigl(r_\phi(x,y_A)-r_\phi(x,y_B)\bigr)$$
	3.	PPO 优化策略模型 $\pi_\theta$：
$$\max_\theta \mathbb{E}{y\sim \pi\theta(\cdot\mid x)}\bigl[r_\phi(x,y)\bigr] - \beta\,\mathrm{KL}\bigl[\pi_\theta\,\|\,\pi_{\text{ref}}\bigr]$$
	•	DPO（Direct Preference Optimization）
	•	直接以偏好对为监督，优化类似“对比学习”损失，规避 RL 算法的高方差与不稳定。
	•	主要优势：训练更加简洁，收敛更快。
	4.	进一步优化
	•	RLAIF：从 AI 而非人类反馈中学习偏好
	•	反事实对比：在同一指令上给出相似但细微不同的响应以增强模型鲁棒性

⸻

小结
	•	Scaling Laws 指导我们在模型规模、数据规模和计算预算间做最优分配；但数据稀缺迫使社区转向更高质量或合成数据。
	•	训练全流程：
	1.	Pre-Train（大规模自监督）
	2.	SFT（指令微调）
	3.	Preference-Fine-Tuning（RLHF / DPO / RLAIF）
	•	Reward Model 本质上是一个打分函数，通过偏好对训练得到，用于指导最终策略模型的参数更新。

以上笔记在原有架构下补充了 Scaling Law 与后训练流程的核心内容，帮助你全面把握模型性能提升的路径与策略。
