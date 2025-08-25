# AI_agents_from_scratch
![AI agent](images\AI_agent.png)
---

# Understanding Agents: The Fundamentals

This section provides a foundational understanding of what an **Agent** is, a concept that is currently "everywhere" across platforms like LinkedIn, Twitter, and Medium. Thanks to frameworks such as LangChain, LlamaIndex, and CrewAI, building functional agents has become accessible with minimal code. However, it's crucial to understand what truly happens "under the hood".

## What is an Agent?
At its core, an **agent is essentially a set of functionalities or abstractions layered on top of a Large Language Model (LLM)**. Without the powering LLM, the agent would be "useless". You can think of the LLM as the agent's "brain", but like a brain, it needs additional components to interact with the external world.

## Key Components of an Agent
To function effectively, an agent combines its LLM "brain" with several crucial components:

*   **LLM (The Brain)**: This is the fundamental component, the "brain" that powers the entire agent.
*   **Planning Capabilities**: These allow an agent to **break down complex tasks into smaller ones, manage subgoals, and even reflect on its results and perform self-criticism**. There is a growing array of techniques for planning and reasoning, including:
    *   Chain of Thoughts (CoT)
    *   CoT-SC
    *   Tree of Thoughts (ToT)
    *   **ReAct**: Highlighted as "one of the most important planning techniques".
*   **Memory Capabilities**: An agent needs to "remember" previous thoughts or interactions. This can be categorized into:
    *   **Short-term memory**: For an LLM, this primarily corresponds to its **context window**.
    *   **Long-term memory**: This allows the agent to **retain information for a (potentially) infinite amount of time** and access information not stored in the LLM's weights. **Vector Databases** such as Qdrant, Weaviate, or Pinecone are excellent examples of solutions for long-term memory.
*   **Tools**: These enable the agent to **interact with the external world**. With tools, an agent can:
    *   Call external APIs and services to retrieve information missing from its model weights (e.g., current events).
    *   Act on external systems (e.g., adding data to a database).

## Conclusion
Despite the hype, agents are fundamentally "pretty simple". All that's required is an **LLM as the "brain," a mechanism to reason (planning), a way to "remember" things (memory), and tools to interact with the external world**. This understanding forms "Lesson 0" of a series designed to explore agentic patterns from scratch, covering both theory and practical implementation.

---

# 1. Reflection Pattern: When Agents Think Twice

## Introduction
This repository explores the **Reflection Pattern**, an Agentic Pattern introduced by Andrew Ng in his DeepLearning.ai series, which allows a Large Language Model (LLM) to evaluate and improve its own outputs. Despite being the simplest of the patterns, the Reflection Pattern provides **surprising performance gains** for LLM responses.

This project implements the Reflection Pattern from scratch using pure Python and Groq LLMs, without relying on frameworks like LlamaIndex or CrewAI, to provide a deeper understanding of its underlying mechanisms.

## What is the Reflection Pattern?
The Reflection Pattern enables an Agent to **reflect on its generated output, provide feedback, and progressively improve the final result**. This mechanism is as simple as a loop.

## How the Reflection Loop Works
The reflection loop is divided into **four core steps**:
*   **Generation Process**: The LLM generates an initial output.
*   **Reflection Process**: The LLM corrects the output generated in the previous step.
*   **Modification**: The LLM takes these corrections and uses them to modify the original output accordingly.
*   **Iteration**: A new iteration starts with the updated output.

### Stopping Criteria for the Loop
The reflection loop doesn't run forever; there are typically **two stopping criteria**:
*   **Fixed Number of Iterations**: The loop runs for a predetermined number of steps.
*   **Stop Sequence**: The LLM generates a specific stop sequence (e.g., "OK", "Correct") indicating the result is satisfactory.

## From Scratch Implementation Example: Merge Sort
To demonstrate the Reflection Pattern, the project uses **Llama3 70B (Groq hosted version)** to code the **Merge Sort algorithm** in Python.

### Implementation Steps:
1.  **Generation Step**:
    *   Initialize a Groq client and relevant libraries.
    *   Start a chat history with a system prompt, instructing the LLM to act as a Python programmer eager for feedback.
    *   Ask the LLM to implement Merge Sort and generate the first version of the code.
    *   An example of the initial generated code is provided in the source.

2.  **Reflection Step**:
    *   Define another system prompt, instructing the LLM to act as **Andrej Karpathy**, a computer scientist and Deep Learning wizard, to provide critique.
    *   Create a reflection chat history where the user message is the code generated in the previous generation phase.
    *   Call the completions endpoint to get Karpathy's suggestions for improvement.
    *   Example suggestions include consistent whitespace (PEP 8), type hints, clearer variable naming, comments for logic, example usage comments, and edge case handling documentation.

3.  **Incorporating Feedback**:
    *   The critique from the reflection phase is added to the original `generation_chat_history`.
    *   The generation block restarts, incorporating this feedback to produce an updated result.
    *   This updated result is then passed back to the reflection phase for additional feedback, continuing the loop.

This process results in a **significantly improved final code** that includes classes, unit tests, and adheres to better coding practices compared to the initial version.

## The Reflection Agent
For a more robust and production-ready implementation, an **abstraction called the Reflection Agent encapsulates the reflection loop**, allowing for simpler and cleaner interaction.

### Installation
You can install the `agentic-patterns` library, which contains the Reflection Agent implementation, using pip:
```bash
pip install -U agentic-patterns
```
Ensure all environment variables are set as described in the accompanying repository.

## Conclusion
The Reflection Pattern, despite its simplicity, is remarkably effective in **enhancing the quality of LLM outputs** by enabling self-correction and iterative refinement.

---