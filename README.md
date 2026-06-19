# No Pain, Same Gain — IGA

**English** | [Português (BR)](README.pt-BR.md) | [Русский](README.ru.md) | [中文](README.zh-CN.md)

**Interference-Gated Aggregation (IGA)** — a growing-memory recurrent model whose recurrent state is constant in sequence length, at no perplexity cost, with recall matching full attention on the literature's hardest standard task. It builds on and improves the foundational growing-memory formulation GRM / Memory Caching (Behrouz et al., 2026).

**Author:** Fábio de Oliveira Peres — Independent Researcher, Rio Verde, Goiás, Brazil.
**Preprint, June 2026.** License: **CC BY 4.0**.

## Files
- `paper.pdf` — the manuscript (Abstract → References), compiled.
- `paper.tex` — LaTeX source.
- `references.bib` — bibliography (authors verified against arXiv records).

## The one-line claim
Growing-memory RNNs [Behrouz et al., 2026] reclaim recurrence but collapse on multi-query associative recall (MQAR), and their state grows without bound. IGA fixes the collapse (gate *unnormalized* per-segment state inside one global ratio, with the fixed memory as a provable initialization floor), routes by *learned identity* via a tensor-sketch polynomial router (no token IDs), and makes the state **constant in T** (a rank-≤S factorization removes the d² term exactly; capped memory caps growth) — 0.39 MB at d=64, 15× smaller than the KV cache projected to 1T parameters, at no recall loss on the headline task. On the official Zoology MQAR harness, hardest standard setting (512,64): IGA 99.2 vs Mamba 23.2 and Based 79.5, matching full softmax attention 100.0. On a real character-level language model (TinyStories, 3 seeds, two contexts) IGA matches a fixed-memory baseline of the same width in bits-per-character within seed noise — **constant memory at no perplexity cost**.

## What the title means
The title names exactly what the measurements support: *no pain* — the growing memory and the O(T²) step are removed, leaving a constant state and O(T) per-step compute; *same gain* — recall matches full attention on the hardest standard task and perplexity matches the fixed-memory baseline on real text.

## Honest limitations (paper §8)
The associative-recall advantage is task-specific (it does not transfer to a planted-fact probe on natural text at d=128); a preliminary ~0.016 bpc gain reported elsewhere does *not* reproduce under the 3-seed, 4000-step protocol; scaling d/ctx to surface a recall separation on real text is left to future work (GPU-budget-bound). Recall claims are at small scale (single layer, d=64); the language-model experiment is the one real-text setting.

## Reproducibility
This initial release contains the **manuscript only**. Code for reproducing the experiments will be released separately. The paper's §9 (Reproduction) lists the commands and corpus used (downloaded TinyStories; no synthetic language-model data is generated).

## License
Manuscript: **Creative Commons Attribution 4.0 (CC BY 4.0)**.