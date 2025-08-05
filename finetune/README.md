##  Fine-Tuning: NeuReg QA Models

###  🧠 Overview
This module contains our fine-tuning experiments for open-ended question answering (QA) tailored to regulatory compliance in educational funding. We leverage both:

T5 variants: t5-small, t5-base, t5-large

FLAN-T5 instruction-tuned variants: flan-t5-small, flan-t5-base, flan-t5-large

Each model is fine-tuned on our domain-specific NeuReg QA datasets using three prompting strategies:

Zero-Shot (ZS)

One-Shot (OS)

Few-Shot (FS)

This results in a total of 18 fine-tuned models (6 model variants × 3 datasets) for comprehensive architectural and prompting evaluation.



---

### 🧪 Fine-Tuning Setup


Epochs: 25

Learning Rate: 3e-4

Batch Size: 8

Max Sequence Length: 256 tokens

Early Stopping: Based on validation loss

Each dataset is split as follows:

Train: 70%

Dev: 20%

Test: 10%

---

### 📊 Evaluation

All 18 models were evaluated using three standard metrics:

ROUGE-1

METEOR 

BLEU 

Fine-tuning led to consistent improvements across all metrics. The best performance was achieved by:

FLAN-T5-Large fine-tuned with One-Shot prompting:

ROUGE-1: 0.526

METEOR: 0.455

BLEU: 0.211

This highlights the effectiveness of combining instruction tuning with domain-specific adaptation in regulatory QA tasks.

-----

••Note••: Due to the large size of fine-tuned models such as T5 / FLAN-T5 variants, we have not included model checkpoints in this repository. Only the evaluation results and scripts are provided. For reproducibility, users may re-run fine-tuning locally if desired using the provided notebooks.
