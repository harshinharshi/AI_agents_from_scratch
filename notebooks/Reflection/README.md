# 1. Reflection Pattern: When Agents Think Twice
![Reflection AI Agent](images/reflection.png)
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

## From-Scratch Implementation: Bubble Sort  

As part of showcasing the **Reflection Pattern**, this project leverages **Llama 3 70B (Groq-hosted)** to generate a Python implementation of the **Bubble Sort algorithm**.  

References:  
- [Malayalam Explanation](https://youtu.be/LZ_II3Kub-E?si=8Ap0E8wmmgiuD5IW)  
- [Animated Walkthrough](https://youtu.be/nmhjrI-aW5o?si=KNiWX2BpQ5OUV6It)  

### Implementation Steps:
1.  **Generation Step**:
    *   Initialize a Groq client and relevant libraries.
    *   Start a chat history with a system prompt, instructing the LLM to act as a Python programmer eager for feedback.
    *   Ask the LLM to implement Bubble Sort and generate the first version of the code.
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