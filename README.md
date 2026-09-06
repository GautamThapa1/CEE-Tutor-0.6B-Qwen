# CEE Exam Question Generator — Fine-Tuned Qwen3-0.6B
LoRA fine-tuned Qwen3-0.6B for Nepal's CEE entrance exam prep — generates exam questions, answers, and explanations for a given subject and topic (Physics, Chemistry, Biology).

🤗 **[Model](https://huggingface.co/Celestial01/qwen3-0.6b-cee-tutor-merged)**

---

## How it works
Synthetic CEE-style questions (generated with DeepSeek, covering Physics/Chemistry/Biology topics) → used to LoRA fine-tune Qwen3-0.6B → merged and quantized (llama.cpp) for faster, lower-memory inference.

## Dataset
Synthetic dataset generated using DeepSeek, ~1200 diverse examples covering CEE-relevant topics across Physics, Chemistry, and Biology.

## Training
- **Base model:** Qwen3-0.6B-Instruct
- **Method:** LoRA (rank 16, attention projections) — fits on a free Kaggle T4 (16GB) without full fine-tuning's memory cost
- **Run:** 2 epochs, ~30 min, free-tier Kaggle GPU

## Evaluation
Base vs. fine-tuned, scored 1–5 by an independent LLM judge (Llama-3.3-70B via Groq):

| Criterion | Base | Fine-tuned | Delta |
|---|---|---|---|
| Correctness | 3.55 | 4.33 | +0.78 |
| Relevance | 4.45 | 4.87 | +0.42 |
| Clarity | 3.14 | 4.58 | +1.45 |

## Limitations
Physics lags mostly because of multi-step numerical questions (circuits, thermodynamics, wave mechanics) — a 0.6B model tends to get the right concept but slip on the arithmetic partway through. Descriptive content (most of Biology) doesn't have this failure mode. The LLM judge is a strong but imperfect filter, so scores reflect this pipeline's evaluation, not independently audited ground truth.

## Stack
`transformers` · `peft` (LoRA) · Qwen3-0.6B-Instruct · Kaggle T4 (free tier) · Groq (Llama-3.3-70B judge) · llama.cpp (quantization) · Gradio · HF Spaces (ZeroGPU)
