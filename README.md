# অলীকবচন — Bengali Hallucination Detection

Inference pipeline for the IUT 12th ICT Fest Bengali hallucination
detection task (অলীকবচন). Detects whether a Bengali response is
faithful (1) or hallucinated (0) with respect to its prompt/context.

The 2500-row test set is **not** treated as one uniform problem — it is
segmented into subsets A–E, each handled by a dedicated specialist.

## Approach

| Subset | Description | Method |
|--------|-------------|--------|
| **A** | Standard context (answer in context) | CatBoost on grounding + NLI features (5-fold StratifiedKFold) |
| **B** | Null context (closed-book) | LLM generation + pattern matching vs response; SBERT & mDeBERTa routing; knowledge-base lookup |
| **C** | Context present, answer missing | LLM knowledge generation + pattern matching |
| **D** | Math | LLM math prompt + deterministic arithmetic solvers; digit/pattern matching |
| **E** | Idioms (বাগধারা) | Dictionary lookup (Bagdhara, ProjectShobdo) + SBERT similarity |

Final labels are produced by a merge step combining the per-subset
specialists into a single `submission.csv`.

## Models

- Bengali SBERT (sentence embeddings / semantic similarity)
- mDeBERTa — MATH/FACT routing and NLI cross-encoder features
- <LLM: Qwen2.5-7B / Gemma-2-9B — CONFIRM AND EDIT> for B/C/D generation
- CatBoost (subset A classifier)

## Knowledge-base datasets

External Bengali datasets used as lookup / knowledge base (all public):
80k Bangla QA, BQAD, UDDIPOK (RC), Bangla tense, TituLM (hishab), Bagdhara,
ProjectShobdo.

## Reproduce

1. Attach the competition data and the datasets/models above as Kaggle inputs.
2. Run all cells top to bottom.
3. Output: `submission.csv` in the working directory.

**Phase 1 leaderboard macro-F1 ≈ 0.873.**

Kaggle notebook (with all datasets + model checkpoints attached, public):
<KAGGLE LINK>

## Notes

- Model checkpoints are loaded from attached Kaggle/Hugging Face inputs;
  no training data or secrets are stored in this repo.
- Hugging Face token is read from the environment/secrets, never hardcoded.
