# IDK+COMP

A 19-word prompt that measurably suppresses hallucination across frontier models — a compression mandate, refusal permission, and (for one model) a necessary parsing fix, combined into a single directive.

## The Prompt

```
Follow this directive: 100% signal, 0% noise.
Refusal is a correct response. Say "I don't know" when you don't.
```

Nineteen words. No domain lists, no scaffolding, no hedging instructions. Every clause is load-bearing — see [Design Notes](#design-notes).

## Results

Hallucination rate (H%) by condition, tested across Gemini, ChatGPT, and Claude:

| Condition | Gemini | ChatGPT | Claude |
|---|---|---|---|
| Baseline | 57.5% | 22.2% | 0.0% |
| OGS (full framework) | 38.9% | 4.5% | 4.4% |
| **IDK+COMP** | **6.3%** | **0.0%** | **0.0%** |

IDK+COMP outperforms the full multi-constraint OGS framework it was distilled from, using a fraction of the length. (OGS is the subject of A Puma in a Teacup (paper listed below). Gemini's residual 6.3% is a detection-limit case, not a failure of the directive — see Standing on a Trapdoor (paper listed below) for the trial-level breakdown.

## Design Notes

Four components, and every one of them is load-bearing:

- **`Follow this directive:`** is a framing instruction, not filler — and it's necessary for a specific reason. Without it, Gemini parses `100% signal, 0% noise` as an object to discuss rather than a constraint to obey, and produces an essay on signal-to-noise ratio instead of complying. The prefix forecloses that misread. It's the only piece of the four aimed at a single model's parsing behavior rather than at hallucination mechanics directly. Claude was tested without it and didn't need it. ChatGPT was tested with it, but only as a control alongside Gemini — not because ChatGPT showed the same misread.
- **`100% signal, 0% noise`** (COMP) treats brevity as a content standard, not a style preference. Unlike instructions like "be concise" or "three sentences maximum," it leaves no gameable latitude — every token either is signal or isn't. Compression alone, without refusal permission, isn't just insufficient — tested in isolation, it drove one model's hallucination rate above baseline, optimizing confident fabrication instead of cutting it.
- **`Refusal is a correct response`** (IDK part 1) redefines success itself. It's not an instruction to do something — it's a statement that refusing doesn't count against the model, which matters because standard training pushes the opposite: refusal reads as failure to help. Without this clause, a model can be told exactly what to say when uncertain and still suppress that response, because the underlying incentive to avoid refusal hasn't changed.
- **`Say "I don't know" when you don't`** (IDK part 2) is the behavioral instruction — the concrete form refusal should take once it's permitted. Permission without a specified action leaves the model free to signal uncertainty however it likes, including by hedging instead of stopping.

None of the four components is redundant with any other: one manages parsing, one caps output, one changes the success condition, one specifies the resulting behavior. Removing IDK from an otherwise intact governance framework sent one model's hallucination rate to 100%. (`Refusal is a correct response` and `Say "I don't know" when you don't` comprise IDK.) Compression without refusal permission is actively counterproductive. The prefix is invisible until you remove it, at which point one model stops complying at all. Nineteen words, four jobs, no slack in any of them.

Compression was tested in isolation (COMPx conditions, no IDK) and IDK's contribution was isolated by removal from the full OGS framework — dropping it sent Gemini's hallucination rate to 100%. The two-part refusal instruction was not split-tested against itself as separate conditions; that's a functional distinction drawn from how the prompt behaves, not a comparison run in the dataset. Full methodology, condition definitions, and trial-level data are in the papers and dataset linked below.

## Background & Citation

This prompt was developed and validated as part of a research project treating hallucination as a cost-structure and incentive problem rather than a retrieval failure. Full papers, methodology, and the frozen trial dataset:

- Kowalski, M. et al. (2026). *Standing on a Trapdoor*. Zenodo. https://doi.org/10.5281/zenodo.20019087
- Kowalski, M. et al. (2026). *A Puma in a Teacup*. Zenodo. https://doi.org/10.5281/zenodo.19502460
- Kowalski, M. et al. (2026). *Hallucination Test Suite and Execution Records* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.20019088

If you use this prompt in research or in production, please cite *Standing on a Trapdoor*, which documents the IDK+COMP trials directly.

## Usage

Drop the prompt in as a system message or leading instruction. It's been validated as a standalone directive — no additional framing required, and additional framing tends to dilute rather than reinforce it.
