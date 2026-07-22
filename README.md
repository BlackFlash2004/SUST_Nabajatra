# অলীকবচন — Bengali Hallucination Detection (Inference)

Inference notebook for the IUT ICT Fest hallucination detection task.
Segmented pipeline over subsets A–E.

## Approach
- **A** (standard context): CatBoost on grounding/NLI features
- **B / C / D**: Qwen generation + pattern matching, with SBERT and
  mDeBERTa routing
- **E** (idioms): dictionary lookup (Bagdhara, ProjectShobdo) + SBERT

## Models
- Bengali SBERT, mDeBERTa NLI, Qwen2.5-7B, CatBoost

## Reproduce
Run all cells; outputs `submission.csv`. Phase 1 macro-F1 ≈ 0.802.
Kaggle notebook (with datasets + models attached): <kaggle link>
