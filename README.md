# AIMO Progress Prize 3: Advanced Mathematical Reasoning Pipeline

This repository contains the high-performance pipeline developed for the **AI Mathematical Olympiad (AIMO) Progress Prize 3**. The system is designed to solve complex AIME/IMO-level problems through a combination of large-scale language modeling, real-time code verification, and advanced ensemble voting.

## 📈 Performance Progression
The pipeline saw significant iterative improvements through prompt engineering and verification logic:

| Version | Date | Key Change | Public Score |
| :--- | :--- | :--- | :--- |
| **V1** | March 21 | Initial Baseline | **30** |
| **V4** | March 24 | Added `ans_verify` logic | **34** |
| **Fork V1** | March 26 | Integrated Entropy Weights | **35** |
| **Peak (Final)** | March 29 | H100 Optimization + Sandbox Pool | **39** |

**Final Evaluation Result:** **80.00% Accuracy** (40/50 Correct on internal validation sets).

## What Makes This System Unique?
Most AIMO solutions rely on raw LLM output. This system treats math as a **software engineering problem**:

* **Entropy-Weighted Voting**: Instead of a simple majority, the system analyzes the log-probabilities of multiple outputs. Answers with lower "entropy" (higher model confidence) are given significantly more weight in the final selection.
* **Persistent Jupyter Sandbox Pool**: Unlike standard "code interpreter" blocks, this system maintains 16 persistent Python kernels. This allows the model to carry variables across multiple "thought steps" and perform brute-force verification for small cases.
* **Multi-Agent Specialist Roles**: Problems are routed through different system prompts—Number Theory Specialists, Combinatorics Experts, and Formal Verifiers—to ensure diverse solution paths.
* **Self-Correction Loop**: After a code execution returns a result, the model is forced to re-read the original problem constraints and "audit" its own code for edge-case failures.

## Hardware & Software Stack
To handle the `gpt-oss-120b` model efficiently, the infrastructure was built for extreme scale:

* **Accelerator**: **NVIDIA H100 GPU** (80GB VRAM).
* **Model Server**: `vLLM 0.11.2` for high-throughput paged attention.
* **Optimization**: `Unsloth` for 4-bit quantized inference without accuracy loss.
* **Environment**: Python 3.12.12.

## Repository Structure
* `solution_pipeline.py`: The main inference engine and voting logic.
* `sandbox/`: Configuration for the persistent Jupyter kernel pool.
* `prompts/`: The specialized "Expert Role" system prompt library.
* `evals/`: Detailed logs showing the 80% accuracy benchmark run.

## Installation & Usage
1.  **Clone the Repo**: `git clone https://github.com/InterstellarMC/AIMO-3.git`
2.  **Install Dependencies**: `pip install -r requirements.txt`
3.  **Setup vLLM**: Ensure your H100 drivers are configured for CUDA 12.8+.
4.  **Run Inference**: `python main.py --problem "Your math problem here"`

---
*Developed for the AIMO Progress Prize 3 Challenge.*
