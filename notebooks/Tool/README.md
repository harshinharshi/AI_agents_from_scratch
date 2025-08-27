# Agent Tools: The Bridge to the Outside World 🌍🛠️

This README provides an overview of **Agent Tools**, a crucial concept for enhancing the capabilities of Large Language Models (LLMs) within the "Agentic Patterns From Scratch" series, specifically Lesson 2. It explains why tools are necessary, how they function, and how to build them from scratch using **Python** and **Groq LLMs**.

## Introduction

Initially, the **Reflection Pattern** (from Lesson 1) was thought to make agents unbeatable. However, even a Reflection Agent has a significant **weakness**: it cannot provide accurate, fresh, or context-specific answers to real-time questions, such as "NVIDIA’s stock price right now" or "yesterday’s temperature in Madrid". This is because the information encoded in an LLM's weights is static and insufficient for such queries.

**Agent Tools** solve this problem by providing LLMs with **ways to access the outside world**. At their core, tools are like functions that the LLM can call to enhance its capabilities, allowing it to retrieve dynamic and external information.

## Why Agent Tools Are Needed

*   **Overcome LLM limitations:** LLMs lack real-time and context-specific information.
*   **Access external data:** Tools enable agents to query external sources for up-to-date and dynamic data.
*   **Enhance agent capabilities:** By using tools, agents can provide more accurate, relevant, and timely responses to user queries.

## Building a Tool from Scratch

The process of making a Python function available to an LLM involves several key steps:

1.  **System Prompt for Function Calling:**
    *   To make an LLM aware of a function, you must provide its relevant information (name, attributes, description) within the **System Prompt**.
    *   The System Prompt instructs the LLM to behave as a **function calling AI model**, selecting which function to use based on a list of function signatures provided within **XML tags**.

2.  **LLM Output Processing:**
    *   When the LLM is asked a question like "What’s the current temperature in Madrid in Celsius?", it will output a function call, often encased in XML tags.
    *   This output needs to be **parsed** (e.g., deleting XML tags).
    *   The parsed output is then **loaded as a proper Python dictionary**.
    *   With the arguments in place, the specified function (e.g., `get_current_weather`) can be **run**.
    *   The result of the function execution (e.g., `{"temperature": 25, "unit": "celsius"}`) is then added to the **chat_history** so the LLM has the information to return to the user.
    *   Finally, the LLM can generate a human-readable response, such as "The current temperature in Madrid is 25 degrees Celsius".

## The Tool Decorator

While the basic method works, frameworks like **LlamaIndex, CrewAI**, or **LangChain** use a more elegant solution: the **tool decorator**.

*   **Purpose:** The tool decorator streamlines the process by automatically transforming any Python function into a description suitable for the System Prompt and signaling to the agent that the function is a tool.
*   **Implementation:** It involves creating a `tool_decorator` that transforms a Python function into a **Tool object**.
*   **Tool Object Parameters:** A `Tool` object typically has the following parameters:
    *   `name`: The name of the tool.
    *   `fn_signature`: The automatically generated function signature.
    *   `fn`: The actual Python function to be called (e.g., `fetch_top_hacker_news_stories`).

### Example: Hacker News Tool

A practical example involves building a tool that interacts with Hacker News to **fetch the top n stories**. The `tool_decorator` is applied to a function like `fetch_top_hacker_news_stories`, making it a fully functional Tool for the agent.