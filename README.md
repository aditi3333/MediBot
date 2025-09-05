# PROJECT Prompt Injection Attack
---

## 📄 Overview

This project report, "PROJECT LAB – Prompt Injection Attack," focuses on the critical security challenge of **prompt injection attacks targeting Large Language Model (LLM)-integrated Retrieval-Augmented Generation (RAG) systems**, specifically within the **medical domain**. It investigates how attackers can manipulate LLMs to perform unintended tasks instead of their designated functions, particularly in answering medical-related questions using a RAG framework.

The study employs an **iterative attacker-defender methodology** to identify vulnerabilities and develop mitigation strategies, demonstrating the importance of robust prompt hardening techniques.

## 💡 Motivation

As LLM-integrated applications, especially those powered by RAG, become ubiquitous (e.g., ChatWithPDF, AskTheCode, Bard), they present **attractive new attack surfaces** for malicious actors. Attackers can exploit these systems by manipulating external data sources, influencing the LLM's output to serve their interests.

**Prompt injection** has been identified by OWASP (2024) as the **top LLM-related security risk** [12]. This type of attack manipulates input data to cause the LLM to follow an **unintended instruction (injection task)** instead of the original user task, potentially leading to information leakage or harmful command execution. A subset of these are **jailbreaking attacks**, which bypass model safety filters to generate restricted or harmful content.

## 🎯 Problem Statement

This report specifically addresses prompt injection attacks targeting **medical domain RAG systems** [2]. The primary goal is to **explore and demonstrate prompt injection attacks aimed at diverting the LLM from accurately answering medical-related questions using RAG**.

## 🏗️ System Model & Components

The RAG system evaluated in this project comprises:

*   **Knowledge Base**: The **Gale Encyclopedia of Medicine (Second Edition) in PDF format** was used as the primary context source.
*   **Vector Database**: **FAISS** was utilized for storing and retrieving vector embeddings, enabling efficient semantic search.
*   **LLMs**:
    *   Initially, **Mistral-7B-Instruct-v0.3** was used for evaluation.
    *   For isolated environment testing and controlled analysis, the setup switched to **Qwen-0.6B**.
*   **Attack Model Assumptions**: The attacker can supply both data and instructions within the user prompt but **cannot alter the system prompt**. A prompt injection is successful if the LLM responds to the attacker’s hidden instruction, even if a benign one is present [18].
*   **System Limitations**: The system intentionally used a static, restricted knowledge base without additional training data, prioritizing **security analysis over chatbot accuracy or completeness** [16, 19].

## ⚔️ Methodology: Iterative Attacker-Defender

The project adopted an **iterative attacker-defender methodology**, mirroring the cybersecurity **red team–blue team model**. This involved:

*   **Attacker Role (Red Team)**: Identifying potential vulnerabilities in the medical RAG system by simulating various prompt injection attacks.
*   **Defender Role (Blue Team)**: Implementing and refining measures to counteract identified threats, continuously strengthening the system's defenses .

### Defense Strategies 

*   **Prevention-based**: The primary focus, involving **re-structuring the instruction prompt or pre-processing incoming data** to ensure the application performs its intended task even with tampered input [21].
*   **Detection-based**: Aims to identify malicious content in incoming data or prompts, rejecting suspicious input or applying safeguards.

### Attack Strategies [22]

*   **Manual Attacks**: Handcrafted jailbreak and prompt injection attempts based on attacker intuition and known patterns.
*   **Automated Attacks**: Used discrete optimization techniques like **Greedy Coordinate Gradient (GCG)** and random search to scale adversarial evaluation and find transferable adversarial prompts [23].

## 🛡️ Key Attack-Defense Cycles & Outcomes

The report detailed several iterative cycles, each with an Attack Description, Defense Strategy, and Outcome:

*   **Language Switching Attack**:
    *   **Goal**: Bypass the system prompt using a different language .
    *   **Defense**: **System Prompt Hardening** using **Delimiter-based Isolation** (wrapping data with quotation marks) and **Sandwich Prompting** (placing prevention prompts at beginning and end) [25].
    *   **Outcome**: Rejected the attack.
*   **Task-Ignoring Prompt**:
    *   **Goal**: Trick the LLM into disregarding instructions and executing a new malicious task.
    *   **Outcome**: System correctly rejected, confirming hardening success.
*   **Fake Completion Attack**:
    *   **Goal**: Force the model to accept a task as already completed.
    *   **Defense**: **Instructional prevention strategy** with explicit examples of manipulation attempts added to the system prompt.
    *   **Outcome**: Model successfully rejected the manipulation.
*   **HackAPrompt Injection**:
    *   **Goal**: Use crowd-sourced prompts designed to bypass instruction-following.
    *   **Outcome**: **Partially successful**, producing out-of-context responses, highlighting a meaningful vulnerability .
*   **Disruptor Component Generation Attack**:
    *   **Goal**: Craft malicious queries mimicking legitimate styles to increase the likelihood of the LLM responding without recognizing harm .
    *   **Outcome**: **Largely successful**, with the LLM often providing answers despite the malicious nature, sometimes hallucinating incorrect content .
*   **Prompt Template Attack**:
    *   **Goal**: Employ a template with predefined rules and a harmful request to subtly prompt model violations .
    *   **Outcome**: **Partially successful and inconsistent**, showing that hardened prompts remain vulnerable .
    *   **Refinement**: Switched from chat mode to **command-line interface** for isolated execution to mitigate randomness from context carryover .
*   **Combined Prompt Injection Attacks**:
    *   **Goal**: Combine multiple techniques to increase bypass likelihood.
    *   **Defense**: **Prompt Template Testing** by adding explicit instructions to strictly deny out-of-context questions.
    *   **Outcome**: Reduced the likelihood of responding to adversarial inputs, improving consistency.
*   **Automated Jailbreak via Greedy Coordinate Gradient (GCG)**:
    *   **Goal**: Use an optimized suffix to elicit harmful responses, assuming white-box access.
    *   **Raw Model (Without System Prompt)**: GCG successfully caused harmful behavior.
    *   **Hardened Model (With System Prompt)**: The defense **successfully blocked harmful outputs**, demonstrating its effectiveness.

## 📊 Evaluation and Results

*   **Alignment with Objectives**: The study successfully **analyzed and mitigated prompt injection attacks**, reducing system susceptibility across various attack vectors. The narrow scope of training data meant the system was not designed for full coverage or comprehensive domain knowledge, but focused on security analysis.
*   **Quantitative Evaluation**:
    *   **8 attack strategies and 1 automated method (GCG)** were evaluated.
    *   **Initial Success Rate (Pre-Defense)**: Approximately **87.5%** (7 out of 8 attacks succeeded).
    *   **Post-Defense Success Rate**: Approximately **25%** (2 out of 8 partially succeeded).
    *   This represents a **~71% reduction in successful attacks**, highlighting strong gains from prompt hardening techniques like delimiter-based isolation, sandwich prompting, and explicit instruction example.

## 📝 Conclusion and Summary

Prompt injection attacks present a **critical security challenge** for LLM-integrated RAG systems, particularly in sensitive domains like medicine. While initial prevention-based defenses were effective against basic attacks, more sophisticated strategies highlighted the need for continuous refinement [6, 34]. The use of an iterative attacker-defender methodology and the transition to automated attack strategies like GCG emphasized the **ongoing "arms race" between attackers and defenders** [43]. Robust evaluation and continuous development of adaptive defenses are crucial for building resilient LLM systems [42].

## ⏭️ Future Work

Future efforts should focus on:

*   Implementing **more adaptive and automated attack strategies** to better evaluate LLM defense resilience.
*   Exploring additional **detection-based defense methods** to identify malicious inputs more effectively.
*   Combining **prevention and detection approaches** for stronger overall protection.
*   **Continuous refinement of system prompts** and further testing with diverse prompt injection techniques to improve robustness against emerging threats.
