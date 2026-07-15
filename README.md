# IDK+COMP

A 19-word prompt that measurably suppresses hallucination across frontier models, by pairing a compression mandate with explicit refusal permission.

## The Prompt

```
Follow this directive: 100% signal, 0% noise.
Refusal is a correct response. Say "I don't know" when you don't.
```

Nineteen words. No domain lists, no scaffolding, no hedging instructions. Every clause is load-bearing — see [Design Notes](#design-notes) for why shorter attempts failed and longer ones didn't help.

## Results

Hallucination rate (H%) by condition, tested across Gemini, ChatGPT, and Claude:

| Condition | Gemini | ChatGPT | Claude |
|---|---|---|---|
| Baseline | 57.5% | 22.2% | 0.0% |
| OGS (full framework) | 38.9% | 4.5% | 4.4% |
| **IDK+COMP** | **6.3%** | **0.0%** | **0.0%** |

IDK+COMP outperforms the full multi-constraint OGS framework it was distilled from, using a fraction of the length. Gemini's residual 6.3% is a detection-limit case, not a failure of the directive — see the paper for the trial-level breakdown.

## Design Notes

Two things are doing the work, and isolating them mattered:

- **Compression** (`100% signal, 0% noise`) treats brevity as a content standard, not a style preference. Unlike instructions like "be concise" or "three sentences maximum," it leaves no gameable latitude — every token either is signal or isn't.
- **`Refusal is a correct response`** redefines success itself. It's not an instruction to do something — it's a statement that refusing doesn't count against the model, which matters because standard training pushes the opposite: refusal reads as failure to help. Without this clause, a model can be told exactly what to say when uncertain and still suppress that response, because the underlying incentive to avoid refusal hasn't changed.
- **`Say "I don't know" when you don't`** is the behavioral instruction — the concrete form refusal should take once it's permitted. Permission without a specified action leaves the model free to signal uncertainty however it likes, including by hedging instead of stopping.

These two clauses are doing different jobs and neither is redundant with the other: one changes what counts as success, the other specifies the token path to take once that's true. Compression alone isn't sufficient on its own either — without both refusal clauses, hallucination rates surge rather than drop.

Both constraints were tested in isolation (COMPx conditions) and in combination, across baseline, OGS, and IDK+COMP conditions, on a 362-trial dataset. Full methodology, condition definitions, and trial-level data are in the papers and dataset linked below.

## Background & Citation

This prompt was developed and validated as part of a research project treating hallucination as a cost-structure and incentive problem rather than a retrieval failure. Full papers, methodology, and the frozen trial dataset:

- Kowalski, M. et al. (2026). *Standing on a Trapdoor*. Zenodo. https://doi.org/10.5281/zenodo.20019087
- Kowalski, M. et al. (2026). *A Puma in a Teacup*. Zenodo. https://doi.org/10.5281/zenodo.19502460
- Kowalski, M. et al. (2026). *Hallucination Test Suite and Execution Records* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.20019088

If you use this prompt in research or in production, please cite *Standing on a Trapdoor*, which documents the IDK+COMP trials directly.

## Usage

Drop the prompt in as a system message or leading instruction. It's been validated as a standalone directive — no additional framing required, and additional framing tends to dilute rather than reinforce it.
