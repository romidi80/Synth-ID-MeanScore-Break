# Break-Synth-ID-text

## Overview

This repository implements a **layer inflation attack** against SynthID-style tournament-based LLM watermarking ("layer inflation"). 
The attack reduces detectability of the Mean score detector by increasing the number of tournament layers while preserving text quality (perplexity/fluency).

## Key files

- `Synth_ID_Break.ipynb` — main notebook with experiment code, attack implementation, and evaluation.

## How the attack works (brief)


1. The SynthID watermark uses a tournament sampling over token logits with multiple layers (depth) to bias selected tokens into a green list.
2. The Mean score detector aggregates per-token signals across layers to produce a detection Z-score; as layers increase, the detector's mean score can be diluted.
3. The layer inflation attack increases tournament layers (or simulates extra layers) while applying small perturbations to logits that maintain fluency but reduce the expected detection score shift.
4. Practical implementation strategies:
   - Inject shifted Gumbel/logistic noise to logits to cancel watermark bias.
   - Re-sample using more tournament layers but with adjusted PRF or shifted logits.
   - Maintain low KL divergence / perplexity by constraining per-token logit changes.


## Usage (quick)

Open `Synth_ID_Break.ipynb` in Jupyter or Colab and run the cells top-to-bottom. The notebook contains:
- Setup and dependencies
- Data / prompts used
- Watermark generation + detection baseline
- Layer inflation attack implementation
- Evaluation scripts (Z-score, perplexity, sample outputs)

## Dependencies

Typical Python stack used in the notebook:
```
python>=3.8
numpy
scipy
torch
transformers
tqdm
scikit-learn
matplotlib
seaborn
```

## Notes on reproducibility


- The notebook includes random seeds for reproducibility — set seeds for NumPy, Torch, and Python `random` before running experiments.
- If you use a proprietary LLM or API, replace the model-call cells with your provider's API wrappers and ensure deterministic sampling where possible.
- Results may vary with model and tokenizer versions. Report exact model names and seed values when sharing results.


## Suggested next steps / experiments

- Sweep over number of inflated layers and measure Z-score decay.
- Constrain KL/perplexity budget to observe trade-off curves.
- Test on multiple models (GPT-family, LLaMA, etc.) and tokenizers.
- Try hybrid attacks that combine noise-shifting with re-application of an independent PRF.

## Contact

If you use this code for research, please cite the notebook and contact the author for collaboration or questions.
