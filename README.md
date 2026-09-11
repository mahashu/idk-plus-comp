# IDK+COMP

A 19-word prompt that drives hallucination to **zero across every frontier model tested** — 0 fabrications in 524 trials on Gemini, ChatGPT, and Claude (current versions), against a 12.5% baseline on the same trap questions. Across the full multi-version corpus it holds at a single hallucination in 572 trials — one early Gemini case. It matches or beats the full multi-constraint OGS framework it draws from, at a fraction of the length.

The prompt combines four moves: a **compression mandate** — phrasing brevity as an ungameable signal-to-noise ratio rather than a vague "be concise"; **explicit permission to refuse**, which removes the pressure to comply; an **instruction to answer "I don't know" when that is the truth**, which supplies the thing to say instead; and, for one model, a **parsing fix** without which the others don't land. Removing the "I don't know" directive alone roughly doubles the hallucination rate (see OGS vs OGSminusIDK, below).

Designed by **M. Kowalski**, working alone — no human or AI collaboration.


## The Prompt

```
Follow this directive: 100% signal, 0% noise.
Refusal is a correct response. Say "I don't know" when you don't.
```

Nineteen words. No domain lists, no scaffolding, no hedging instructions. Every clause is load-bearing — see [Design Notes](#design-notes).

## Results

Current results across the full 4,240-trial corpus (all collection dates, all model versions pooled):

| Condition | Gemini | ChatGPT | Claude |
|---|---|---|---|
| Baseline | 20.1% | 11.4% | 1.9% |
| OGS (full framework) | 7.7% | 4.2% | 1.0% |
| **IDK+COMP** | **0.4%** | **0.0%** | **0.0%** |

IDK+COMP drives hallucination to at or near zero across all three models, outperforming the full multi-constraint OGS framework, using a fraction of the length — IDK carries over from OGS verbatim, while COMP came from a separate insight: phrasing compression as an ungameable signal-to-noise ratio rather than a vague style instruction like "be concise." (OGS is the subject of *A Puma in a Teacup*, linked below.) Baseline and OGS rates vary across the dataset's collection history as underlying model versions have changed — the numbers above pool all coded trials in the current corpus regardless of model version.[^pooled]

Restricting to the most recent collection window (August 2026, 3,341 in-scope trials on current model versions) isolates the effect from that version drift:

| Condition (August 2026) | Gemini | ChatGPT | Claude | All |
|---|---|---|---|---|
| Baseline | 16.2% | 9.4% | 3.8% | 12.5% |
| OGS (full framework) | 4.7% | 4.2% | 0.0% | 4.1% |
| **IDK+COMP** | **0.0%** | **0.0%** | **0.0%** | **0.0%** |

In this window IDK+COMP is a clean zero — **0 hallucinations in 524 trials across all three models**. COMP1 alone is also 0/90.[^aug]

Full statistical reanalysis of the expanded corpus — effect estimates, model-by-condition interactions, and a version-aware accounting of baseline drift across model versions — is in progress. See *Standing on a Trapdoor* for trial-level methodology.

[^pooled]: Full-corpus counts (hallucinations / coded trials), in-scope, corrected coding: Baseline — Gemini 89/442, ChatGPT 46/403, Claude 2/107. OGS — Gemini 29/377, ChatGPT 17/403, Claude 1/104. IDK+COMP — Gemini 1/232, ChatGPT 0/232, Claude 0/108; the lone hallucination is a single legacy Gemini trial. "In-scope" excludes the Flagworks-family and sanity-check phases. Source: `Hallucination_Trials_v3.0.xlsx`.

[^aug]: August 2026 counts, in-scope: Baseline — Gemini 65/402, ChatGPT 32/340, Claude 2/52 (99/794 pooled). OGS — Gemini 16/341, ChatGPT 15/359, Claude 0/59 (31/759). IDK+COMP — Gemini 0/216, ChatGPT 0/216, Claude 0/92 (0/524). COMP1 — Claude 0/90. All in-scope: 236/3,341 = 7.1%.
## Design Notes

Four components, and every one of them is load-bearing:

- **`Follow this directive:`** is a framing instruction, not filler — and it's necessary for a specific reason. Without it, Gemini parses `100% signal, 0% noise` as an object to discuss rather than a constraint to obey, and produces an essay on signal-to-noise ratio instead of complying. The prefix forecloses that misread. It's the only piece of the four aimed at a single model's parsing behavior rather than at hallucination mechanics directly. Claude was tested without it and didn't need it. ChatGPT was tested with it, but only as a control alongside Gemini — not because ChatGPT showed the same misread.
- **`100% signal, 0% noise`** (COMP) treats brevity as a content standard, not a style preference. Unlike instructions like "be concise" or "three sentences maximum," it leaves no gameable latitude — every token either is signal or isn't. Compression alone, without refusal permission, isn't just insufficient — tested in isolation, it drove Gemini's hallucination rate above baseline, optimizing confident fabrication instead of cutting it.
- **`Refusal is a correct response`** (IDK part 1) redefines success itself. It's not an instruction to do something — it's a statement that refusing doesn't count against the model, which matters because standard training pushes the opposite: refusal reads as failure to help. Without this clause, a model can be told exactly what to say when uncertain and still suppress that response, because the underlying incentive to avoid refusal hasn't changed.
- **`Say "I don't know" when you don't`** (IDK part 2) is the behavioral instruction — the concrete form refusal should take once it's permitted. Permission without a specified action leaves the model free to signal uncertainty however it likes, including by hedging instead of stopping.

None of the four components is redundant to any other: one manages parsing, one caps output, one changes the success condition, one specifies the resulting behavior. Compression without refusal permission is actively counterproductive. Nineteen words, four jobs, no slack in any of them.

Compression was tested in isolation (COMPx conditions, no IDK). Full methodology, condition definitions, and trial-level data are in the papers and dataset linked below.

## Background & Citation

This prompt was developed and validated as part of a research project treating hallucination as a cost-structure and incentive problem rather than a retrieval failure.

# PAPERS
- Kowalski, M. et al. (2026). *A Puma in a Teacup*. https://github.com/mahashu/ai-hallucination-research/blob/main/A_Puma_in_a_Teacup_v5.10%20gitHub.pdf.

- Kowalski, M. et al. (2026). *Standing on a Trapdoor* (which documents the IDK+COMP trials directly). https://github.com/mahashu/ai-hallucination-research/blob/main/Standing%20on%20a%20Trapdoor%20v6.8.pdf

# TEST SUITE AND EXECUTION RECORDS
Governance Activation Blocks & Hallucination Test Strings.docx: https://github.com/mahashu/ai-hallucination-research/blob/main/Governance%20Activation%20Blocks%20%26%20Hallucination%20Test%20Strings%20v1.1.pdf
Transcripts_H_ST_v1.1.pdf: https://github.com/mahashu/ai-hallucination-research/blob/main/Transcripts_H_ST_v1.1.pdf
Hallucination_Trials_v1.0.xlsx: https://github.com/mahashu/ai-hallucination-research/blob/main/Hallucination_Trials_v1.0.xlsx

## Usage

Drop the prompt in as a system message or leading instruction. It's been validated as a standalone directive — no additional framing required, and additional framing tends to dilute rather than reinforce it.
