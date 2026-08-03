# CEE MCQ Generator — Fine-Tuned Qwen3-1.7B

LoRA fine-tuned Qwen3-1.7B that generates multiple-choice questions for Nepal's CEE entrance exam (Physics, Chemistry, Biology), given a subject and topic. Each question comes with options, the correct answer, and a worked explanation.

🤗 **[Live Demo](https://huggingface.co/spaces/Celestial01/qwen3-cee-demo)**

---

## How it works

Real CEE questions (scraped with permission) → cleaned and QC'd → used to LoRA fine-tune Qwen3-1.7B. At generation time, every output passes through a structural check and an independent LLM-judge check before it's added to the validated pool. Nothing generated goes straight to students — only what clears both checks does.

## Dataset

~2,800 real CEE MCQs across 28 topics (Physics/Chemistry/Biology), collected with the source site owner's explicit permission, each with a worked explanation. Cleaned via automated QC (duplicate detection, malformed-option checks) before training.

## Training

- **Base model:** Qwen3-1.7B-Instruct
- **Method:** LoRA (rank 16, attention projections) — fits a 1.7B model on a free Kaggle T4 (16GB) without full fine-tuning's memory cost
- **Data:** ~5,600 prompt/completion pairs (prompt-phrasing augmentation), stratified train/val split by topic
- **Run:** 3 epochs, ~30 min, free-tier Kaggle GPU

## Evaluation

Validated via structural checks + an independent LLM judge (Llama-3.3-70B), two batches of n=140:

| Subject | Pass Rate |
|---|---|
| Biology | ~70–75% |
| Chemistry | ~60–65% |
| Physics | ~40–45% |
| **Overall** | **~64–69%** |

## Limitations

Physics lags mostly because of multi-step numerical questions (circuits, thermodynamics, wave mechanics) — a 1.7B model tends to get the right concept but slip on the arithmetic partway through. Descriptive content (most of Biology) doesn't have this failure mode. The LLM judge is a strong but imperfect filter and occasionally over-rejects valid questions, so the pass rate reflects "validated by this pipeline," not independently audited ground truth.

## Future work

- GRPO fine-tuning with a reward function targeting numerical consistency, aimed at the Physics gap
- Bayesian Knowledge Tracing to drive topic selection from real student performance
- Chain-of-thought training data for calculation-heavy questions

## Stack

`transformers` · `peft` (LoRA) · Qwen3-1.7B-Instruct · Kaggle T4 (free tier) · Groq (Llama-3.3-70B judge) · Gradio · HF Spaces (ZeroGPU)
