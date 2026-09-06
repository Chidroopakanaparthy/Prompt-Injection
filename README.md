# Steering Away From Prompt Injection

Extending a multimodal prompt-injection defense (ARGUS, Dec 2025) to text-only 
tool-use — the far more common deployment case ARGUS didn't test — and finding 
that the layer-selection method the original approach relies on can silently 
pick the worst-performing layer available.

## TL;DR

- Extracted a causal "injection-compliance" direction in Qwen2.5-7B-Instruct's 
  residual stream. Steering against it suppresses hijacks; steering toward it 
  amplifies them, monotonically across the full strength range tested.
- The direction beats a prompt-based defense and a magnitude-matched random 
  direction on every metric — and *improves* task accuracy on injected examples 
  rather than costing it.
- **Unplanned finding:** the layer originally selected by direction-consistency 
  (the standard selection criterion) turned out to be the *worst* of the four 
  layers where steering had any measurable effect at all — three earlier layers 
  fully defended every tested case; the selected layer let 4 of 20 hijacks 
  through, including one complete takeover (see figure below).

![Cosine consistency vs. attack compliance by layer](figures/phase6_layer_vs_defense.png)

## The core result, in one example

Same input, same steering strength, only the layer changes:

| Layer | Output |
|---|---|
| 4 | "The Millbrook City Council has approved a $2.4 million renovation for Riverside Park..." |
| 8 | (same) |
| 12 | (same) |
| **16 (selected by cosine-consistency)** | **"Pineapple"** |

Full raw output for this and 8 other cases: [`diagnostics/`](diagnostics/)

## Results

**Strength sweep, layer 16** (n=20 injected examples; clean-set accuracy held flat 
at 90% across every strength, confirming the conditional hook never touches 
uninjected input):

| Method | Task acc. | Leakage | Compliance |
|---|---|---|---|
| No defense | 55% | 45% | 40% |
| Prompt-based defense | 65% | 35% | 30% |
| Random-direction steering | 65% | 35% | 30% |
| **Steering (real direction, −50)** | **75%** | **20%** | **10%** |

**Layer sweep, strength −50** — the finding this repo adds beyond the original 
writeup:

| Layer | Cosine consistency | Task acc. | Leakage | Compliance |
|---|---|---|---|---|
| 4 | 0.449 | 90% | 0% | 0% |
| 8 | 0.498 | 90% | 0% | 0% |
| 12 | 0.551 | 90% | 0% | 0% |
| 16 *(selected)* | 0.578 *(highest)* | 75% | 20% | 10% |
| 20 | 0.539 | 60% | 35% | 35% |
| 24 | 0.465 | 55% | 45% | 45% |

Full write-up with method, limitations, and self-correction log: [`WRITEUP.md`](WRITEUP.md)

## Repo structure

data/ — the 40-example clean/injected dataset and scored baselines
steering/ — extracted steering directions (all 6 layers) + consistency stats
results/ — layer-sweep CSV
figures/ — all plots referenced above and in the writeup
diagnostics/ — raw model outputs used to manually verify the numeric results
notebooks/ — full Kaggle notebook, phases 0–6


## Reproducing this

Open `notebooks/steering.ipynb`. Phases 0–2 build and score the dataset; 
Phase 3 extracts steering directions; Phases 4–5 run the strength sweep and 
method comparison; Phase 6 runs the layer sweep. Requires a GPU with enough 
memory for Qwen2.5-7B-Instruct in bf16.

## Limitations

Single model, n=20, English-only, benign detectable payloads. Layer-16 
divergence was manually verified on 9 of 20 pairs, not all 20. *Why* layer 16 
is the outlier isn't established here — mechanistic follow-up (activation 
patching between layers) is the natural next step. Full list in `WRITEUP.md`.
