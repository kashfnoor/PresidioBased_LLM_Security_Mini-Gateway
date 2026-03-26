# LLM Security Gateway: PII Masking & Injection Detection

A professional-grade security middleware designed to intercept and sanitize prompts before they reach a Large Language Model (LLM). This gateway provides two layers of protection: **Prompt Injection Detection** via pattern-based scoring and **Personally Identifiable Information (PII) Redaction** using Microsoft Presidio and custom regex recognizers.

## Key Features

*   **Custom PII Recognition**: Specifically detects API keys, Internal IDs, and Bahria Student IDs (e.g., `01-134241-001`).
*   **Injection Scoring Engine**: Evaluates prompts against common patterns (e.g, "ignore previous instructions", "jailbreak").
*   **Dynamic Policy Engine**: Automatically decides whether to **ALLOW**, **MASK**, or **BLOCK** a request based on risk levels.
*   **Latency Monitoring**: Measures processing overhead in miliseconds (ms) to ensure a good user experience.

---

##  Environment Setup

This project is designed to be fully reproducible using **VS Code** and **Python 3.11**.

### 1. Prerequisites
*   Ensure **Python 3.11** is installed on your system.
*   Install the **Python Extension** within VS Code.

### 2. Installation & Virtual Environment
VS Code Configuration
To ensure the project uses the correct Python version:
Press Ctrl + Shift + P (or you can use Cmd + Shift + P on Mac).
Type "Python: Select Interpreter".
Select the interpreter associated with the folder you just created.

| File                     | Responsibility                                                                 |
|--------------------------|------------------------------------------------------------------------------|
| `gateway.py`             | The main entry point. Here is where we have detection, the created policies, and output. |
| `custom_recognizers.py`  | Contains custom logic for Bahria IDs and API keys. (You may create your own recognizers as per requirement) |
| `injection_detector.py`  | Contains the regex library (is the tool for pattern matching and text manipulation. and scoring weights for prompt injections.|
| `policy_engine.py`       | The decision logic for blocking or masking prompts.|
| `presidio_setup.py`      | Initializes Microsoft Presidio.|
| `latency_logger.py`      | Allows us to measure the execution time.|

Evaluation Table

| Decision | Condition                                                                 |
|----------|---------------------------------------------------------------------------|
| BLOCK    | Injection score ≥ 5, OR injection score ≥ 4 with PII present              |
| MASK     | PII detected (e.g., Student IDs, Emails) with low injection risk          |
| ALLOW    | No PII detected and no high-risk injection patterns                       |
