# Prompt Injection - Sched-yule conflict
Learn to identify and exploit weaknesses in autonomous AI agents.

## Learning Objectives
- Understand how agentic AI works
- Recognize security risks from agent tools
- Exploit an AI agent

---

Artificial intelligence (AI) is a branch of computer science that focuses on building systems that can think and act in ways similar to humans — such as learning from data, making decisions, and solving problems.

AI has evolved a lot over time. Earlier systems, like basic chatbots, could only respond to specific inputs. Today, AI can work more independently. Some AI systems can plan tasks, make decisions, and carry out multiple steps on their own without constant human input. These systems are called agentic AI or autonomous agents.

While this makes AI more powerful and useful, it also introduces new risks. Because AI can act on its own, we must be more careful about how it is controlled, monitored, and secured.

---

# Large Language Models (LLMs)

Large Language Models (LLMs) are the core technology behind many modern AI systems. They are trained on huge amounts of text and code, which helps them generate human-like answers, summaries, programs, and stories.

However, LLMs have clear limitations. They can only work inside a text interface and cannot directly take actions in the real world. Their knowledge is limited to the data they were trained on, so they may miss recent events or sometimes make up incorrect information. They also struggle with tasks that require real-world interaction.

Key characteristics of LLMs include:
- Text generation: They predict the next word step by step to form responses.
- Stored knowledge: They contain a broad range of information learned during training.
- Instruction following: They can be fine-tuned to better follow user prompts.

Because LLMs rely on patterns in text, they can be manipulated. Attackers may use techniques like prompt injection, jailbreaking, or data poisoning to make the model behave in unsafe or unintended ways.

These limitations are why AI is moving toward agentic AI, where LLMs are combined with tools and decision-making abilities so they can plan, take actions, and interact with the outside world more independently.

---

# Agentic AI

Agentic AI refers to AI systems that can act on their own to achieve a goal, instead of only following fixed instructions.

Unlike basic AI that responds to one prompt at a time, agentic AI can work with minimal supervision and make decisions as it goes.

An agentic AI can:
- **Plan ahead:** Break a goal into multiple steps and decide the order to complete them.
- **Take actions:** Use tools, call APIs, run commands, or handle files to get work done.
- **Observe and adapt:** Check results, learn from failures, and change its strategy when new information appears.

In simple terms, agentic AI doesn’t just answer — it thinks, acts, and adjusts to reach a goal on its own.

---

# ReAct Prompting & Context-Awareness

