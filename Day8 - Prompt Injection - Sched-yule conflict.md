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

Agentic AI can do complex tasks because it uses a reasoning method called Chain-of-Thought (CoT).

Chain-of-Thought (CoT) means the AI thinks step by step instead of jumping straight to an answer. This helps it solve problems that need logic, math, or careful reasoning.

However, CoT has a limitation:
- It works only inside the AI’s own knowledge.
- It cannot check facts in real time.
- It can make mistakes, repeat errors, or invent information (hallucinate).

> ReAct: Reason + Act

ReAct improves on Chain-of-Thought by allowing the AI to both think and act.

Instead of just reasoning internally, a ReAct-based AI:
- Thinks about what to do
- Takes an action (like searching the web, calling an API, or running code)
- Observes the result
- Updates its plan based on what it learned

This happens in a loop, similar to how humans work.

## How ReAct Works

A ReAct-enabled AI alternates between:
- **Reasoning:** Explaining its current thinking
- **Action:** Performing a real-world operation (searching, querying data, executing tools)

## Why ReAct Is Important

ReAct allows the AI to:
- Adjust its plan when something fails
- Use up-to-date or external information
- Reduce fake or incorrect answers
- Combine thinking and doing in a smarter way

In simple terms, ReAct helps AI behave more like a human:
> Think → Act → Observe → Improve → Repeat

---

# Tool Use/User Space

Modern AI models can use tools to do things they cannot do on their own, like searching the web, calling APIs, or working with files. This feature is often called function calling.

## How Tool Use Works

**1. Developers define tools**
- Tools are described to the AI using a structured format (like JSON).
- Each tool has:
-- A name
-- A description
-- A list of inputs it accepts

This teaches the AI what tools exist and how to use them.

**2. The AI decides when to use a tool**
- If a user asks something that needs real-time or external data (e.g., recent news),
- The AI understands it should **use a tool instead of guessing.**

**3. The AI calls the tool**
- The AI sends a structured request to the tool with the required input.
- **Example:** searching the web for recent information.

**4. The external system responds**
- The tool (like Google or Bing search) runs outside the AI.
- It returns real-world results.

**5. The AI uses the results**
- The AI reads the tool’s output.
- It combines this information with its reasoning.
- Then it gives the user a clear, natural-language answer.

