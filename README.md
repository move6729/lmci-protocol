# Local Model Weights & Context Isolation Protocol (LMCI-v1.0)

**Classification:** Open Standard / Bare-Metal Inference & Memory Isolation  
**Canonical Reference:** `LMCI-v1.0`  
**Target Infrastructure:** Autonomous Edge Nodes, Local Quantized Models, Local Vector Storage  

---

## 1. System Topology

[ ATN Task Graph ] ──► [ LMTI Transport Layer ]
│
▼
┌─────────────────────────────────────┐
│ LMCI Local Runtime & Memory Engine │
└──────────────────┬──────────────────┘
│
┌─────────────────────────┼─────────────────────────┐
│ (Local Inference) │ (Encrypted Vector DB) │ (Bare-Metal HW)
▼ ▼ ▼
[ llama.cpp / vLLM ] [ LanceDB / Local Vector ] [ Apple Silicon / GPU ]
GGUF/EXL2 Quantized AES-256 Encrypted Disk Zero Cloud API Telemetry


---

## 2. Core Protocol Invariants

1. **Zero Cloud LLM Dependency:** Inference must execute locally via open-weight quantized models (GGUF, EXL2, AWQ) using engines like `llama.cpp` or `vLLM`. Cloud API fallback is forbidden for core task execution.
2. **Local-First Encrypted Vector Memory:** Embeddings and context state must reside on local disk in encrypted stores (e.g., LanceDB, DuckDB). No third-party vector cloud services allowed.
3. **Hardware-Direct Acceleration:** Execution targets bare-metal consumer hardware (Metal/Apple Silicon, CUDA, ROCm) to maximize performance-per-watt without remote compute dependencies.
4. **Context Isolation:** Memory stores and model activations are sandboxed per execution task. Memory vectors must not leak cross-task telemetry.

---

## 3. Reference Implementation

- Model Weights & Memory Isolation Engine: `proofs/weight_isolation.py`

