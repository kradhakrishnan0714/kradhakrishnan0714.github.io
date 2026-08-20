---
layout: single
title: "Vision-Edge: A Privacy-Preserving On-Device Accessibility Agent"
classes: wide
featured: true
venue: "SMU · DS 6210 Capstone"
thumb_label: "SVLM · On-device VQA"
card_summary: "An on-device visual question answering system for blind and low-vision users. Three fine-tuned vision-language models — quality gate, scene understanding, document reading — run locally in the browser with speech output, so images never leave the device."
tech: ["PyTorch", "Moondream2", "Florence-2", "MobileNetV3", "LoRA", "Transformers.js"]
highlights:
  - value: "73.8%"
    label: "VizWiz accuracy for scene understanding, up from 2.75% at baseline"
  - value: "91.3%"
    label: "Correctly identifies questions the image cannot answer"
  - value: "411 ms"
    label: "Mean inference — 47% faster than the pretrained baseline"
---

**Description**: A browser-based assistive system that answers spoken questions about what a camera sees, built for blind and low-vision users. All inference runs on the user's own device, so images are never uploaded.

**Tech Stack**: PyTorch, Moondream2, Florence-2, MobileNetV3-Small, LoRA, Transformers.js, WebGPU, Web Speech API

**Capstone research** with Robert Slater, M.S. Data Science, Southern Methodist University.

## The problem

Cloud-based AI assistants for visually impaired users share three structural weaknesses: they need connectivity that isn't always there, they add round-trip latency to time-sensitive tasks, and they send photographs of a user's home, mail, and medication to someone else's server.

Running the models locally removes all three constraints at once — but only if the models are small enough to run in a browser and accurate enough to be trusted. That tension is what this project set out to test.

## System design

Vision-Edge routes each capture through three task-specific models rather than asking one general model to do everything:

1. **Quality gate — MobileNetV3-Small.** Images taken by blind users are frequently blurred, dark, badly framed, rotated, or obstructed by a finger. A lightweight multi-label classifier screens for seven such flaws *before* any expensive generative inference runs, and returns corrective feedback so the user can recapture rather than receive a confident wrong answer.
2. **Scan mode — Moondream2.** A compact vision-language model fine-tuned for scene understanding and visual question answering.
3. **Read mode — Florence-2.** A document-capable model for printed text, forms, and structured layouts.

Both language models were adapted using **low-rank adaptation (LoRA)**, training a small set of task-specific parameters while the pretrained backbone stayed frozen — which is what made fine-tuning feasible without full retraining.

The interface runs on **Transformers.js v3** with WebGPU acceleration where available, and speaks results through the Web Speech API. After the initial model download, the system works offline.

## Datasets

| Dataset | Purpose | Scale |
|---|---|---|
| VizWiz Image Quality Issues | Quality gate training | 39,181 images (23,431 train / 7,750 val / 8,000 test) |
| VizWiz VQA | Scene understanding | Images captured by blind users, with spoken questions and 10 crowd answers each |
| DocVQA | Document reading | ~50,000 questions over 12,000+ document images |

The VizWiz datasets matter specifically because the images were captured by blind photographers in genuine information-seeking situations — lower quality, conversational questions, and crucially a large share of questions that *cannot* be answered from the image at all.

## Results

Fine-tuning helped every component, but by very different margins.

### Quality gate (MobileNetV3-Small)

| Metric | Baseline | Fine-tuned | Change |
|---|---|---|---|
| Exact multi-label accuracy | 1.2% | 51.2% | 44× |
| Macro F1 | 0.164 | 0.564 | 3.4× |
| Mean inference time | 4.55 ms | 4.52 ms | unchanged |

The gain came at no latency cost, which matters for a component that gates everything downstream.

### Scene understanding (Moondream2)

| Metric | Baseline | Fine-tuned | Change |
|---|---|---|---|
| VizWiz accuracy | 2.75% | 73.8% | 27× |
| Exact match | 3.2% | 82.9% | 26× |
| BLEU-1 | 0.124 | 0.885 | 7× |
| BLEU-4 | 0.030 | 0.194 | 6× |
| Unanswerable accuracy | 2.53% | 91.3% | 36× |
| Mean inference time | 780 ms | 411 ms | 47% faster |

This is the headline result. The pretrained model was essentially unusable on images captured by blind users; after fine-tuning it became practically useful — including learning **when not to answer**, which for an assistive tool matters as much as answering correctly.

### Document reading (Florence-2)

| Metric | Baseline | Fine-tuned | Change |
|---|---|---|---|
| ANLS | 0.4% | 19.2% | 45× |
| Exact match | 0% | 1.12% | 0 → positive |
| Mean inference time | 932 ms | 60 ms | 15× faster |

Document understanding improved but remains the weakest component. The model produces approximately correct answers far more often than before, yet rarely recovers exact target strings — document reasoning depends on layout structure and character-level fidelity, a stricter burden than scene description.

## Browser deployment

Deployment used two runtime paths: a **quantized** model for faster startup and lower memory (the default interactive mode), and a **normal-precision** path retained for cases where answer fidelity mattered more than speed. Treating deployment as a speed-quality trade-off rather than a single fixed mode measurably improved practical usability on commodity hardware.

## What I'd take further

- **Document reasoning** needs stronger supervision or a larger training budget to become reliable.
- **Rare quality-flaw categories** remain hard; threshold calibration and data balancing should help.
- **Browser performance** — more aggressive quantization, WebGPU scheduling, model warmup and caching, and progressive loading to cut first-response latency.

The broader finding: adapting compact vision-language models to data actually generated by blind users beats relying on generic pretrained behaviour, and it does so by a margin large enough to change whether the system is usable at all.
