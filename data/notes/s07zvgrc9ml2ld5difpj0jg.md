
![](/assets/images/2023-09-21-22-34-13.png)

In a LLM-powered autonomous agent system, LLM functions as the agent’s brain, complemented by several key components:

# Planning

Subgoal and decomposition: The agent breaks down large tasks into smaller, manageable subgoals, enabling efficient handling of complex tasks.
Reflection and refinement: The agent can do self-criticism and self-reflection over past actions, learn from mistakes and refine them for future steps, thereby improving the quality of final results.

[[development.ml.LLM.agent.planning]]
[[development.ml.LLM.agent.self_reflection]]

# Memory

Short-term memory: I would consider all the in-context learning (See Prompt Engineering) as utilizing short-term memory of the model to learn.
Long-term memory: This provides the agent with the capability to retain and recall (infinite) information over extended periods, often by leveraging an external vector store and fast retrieval.

[[development.ml.LLM.agent.memory]]

# Tool use

The agent learns to call external APIs for extra information that is missing from the model weights (often hard to change after pre-training), including current information, code execution capability, access to proprietary information sources and more.


[[development.ml.LLM.agent.tool]]

[reference](https://lilianweng.github.io/posts/2023-06-23-agent/)