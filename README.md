Break SynthID
🔍 Overview

This project analyzes and empirically validates vulnerabilities in Google DeepMind’s SynthID-Text watermarking framework.

It is based on the ICLR 2026 workshop submission:

“Google’s LLM Watermarking System Is Intrinsically Vulnerable to Layer Inflation Attack”

The work provides:

Theoretical analysis of SynthID-Text detection performance

Proof that TPR@FPR is unimodal in tournament layers

A black-box Layer Inflation Attack

Empirical validation across multiple LLMs

🧠 Background

SynthID-Text is a production-ready watermarking system deployed in Gemini.

It works via:

Multi-layer Tournament Sampling

Randomized g-value functions

A Mean Score (MS) detection function

Detection metric: TPR@FPR

This project demonstrates that the mean score detection is provably vulnerable when the number of tournament layers increases.

🚨 Core Vulnerability
Theoretical Result

Using the Central Limit Theorem:

The mean score follows a normal distribution.

TPR@FPR is a function of the number of layers m.

TPR initially increases.

After a critical point M, TPR decreases.

As m → ∞, TPR → FPR.

This makes detection ineffective.

🛠 Layer Inflation Attack

We implement a black-box attack that:

Queries a watermarked LLM multiple times.

Collects 2N outputs.

Applies an additional N-layer tournament.

Produces a final token.

Lowers mean score below detection threshold.

Result

On 1,000 watermarked prompts:

Model	TPR After Attack
GPT-2B	0.05
Gemma-7B	0.00
Mistral-7B	0.01

Watermark detection collapses to near FPR=1%.

📂 Project Structure
.
├── Break_Synth_ID.ipynb        # Main experiments
├── 110_Google_s_LLM_Watermarking_(2).pdf  # Paper reference
├── README.md
⚙️ Requirements
pip install torch numpy matplotlib transformers bert-score

Optional:

pip install jupyter
🚀 Running Experiments

Launch Jupyter:

jupyter notebook

Open:

Break_Synth_ID.ipynb

Run cells sequentially.

📊 Experimental Setup

Dataset: ELI5

Tokens per sample: 100

FPR: 1%

Default layers: m = 30

Additional inflation layers: N = 15

Temperature: 1.0

📈 What This Project Shows

✔ SynthID-Text detection is unimodal in layers
✔ Detection collapses under layer inflation
✔ Vulnerability holds for Bernoulli and Uniform g-values
✔ Attack is black-box (no key required)

🔬 Related Work

Scott Aaronson – Early LLM watermarking ideas

Gemma: Open Models Based on Gemini Research and Technology

ICLR

Google

🎯 Research Implications

This exposes a structural weakness in:

Mean-score watermark detection

Multi-layer tournament sampling

Current production watermark deployments

It motivates:

Robust detection functions

Non-unimodal scoring rules

Provable watermark guarantees

 License

Research and educational use only.