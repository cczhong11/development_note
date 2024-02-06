
# ReAct

通过将行动空间扩展为特定任务离散行动和语言空间的组合，在 LLM 中整合了推理和行动。前者使 LLM 能够与环境互动（例如使用维基百科搜索 API），而后者则促使 LLM 以自然语言生成推理痕迹。

The ReAct prompt template incorporates explicit steps for LLM to think, roughly formatted as:

Thought: ...
Action: ...
Observation: ...
... (Repeated many times)

# Reflexion

![](/assets/images/2023-09-21-22-37-31.png)

自我反思是通过向 LLM 展示two shot示例来创建的，每个示例都是一对（失败的轨迹、用于指导未来计划变更的理想反思）。然后将反思添加到代理的工作记忆中，最多可添加三个，作为查询 LLM 的上下文。