---
layout: post
title:  "Why a DNN's \"eyesight\" matters, not just its brain"
date:   2025-12-18
topic: ml
---

Paper 1. Unexplored Faces of Robustness and Out-of-Distribution: Covariate Shifts in Environment and Sensor Domains - Eunsu Baek, Keondo Park, Jiyoon Kim, Hyung-Sin Kim

**Problem statement and Background**

Improving the ‘brain’ (DNN) might be inefficient if input data is altered during acquisition.
- Vision system captures light and brain interpreting it afterwards. Glasses can fix eyesight problems here than cognitive enhancements. 
- Similarly, may neglect distribution shifts occur during initial image capture by camera. 

**Proposal: New Benchmark Dataset (ImageNet-ES)**

directly captures images with variations in environmental (lighting) and camera sensor (ISO, shutter speed, aperture) factors 
- Differentiation from existing benchmarks: ImageNet-C, -P simulate such shifts → ImageNet-ES = direct investigation of how real-world 
- covariate shift affects model robustness, out-of-distribution (OOD) detection  

**ImageNet-ES Dataset, ES-Studio**

ES-Studio: controllable testbed to create ImageNet-ES 
- reproductible setup: dark room with a screen to display images from TinyImageNet (camera with adjustable sensor parameters) and controlled ceiling lamps for manipulating environmental light. 

ImageNet-ES Dataset
- 202,000 images from 2,000 samples from TinyImageNet validation set 
- variations 
(1) environmental factor (lights on/ off) 
(2) camera sensor parameters (ISO, shutter speed, aperture) using auto exposure (captured 5 times to account for variability), manual settings (64 variations for validation, 27 for testing) 

**Experiment 1. Out-of-Distribution (OOD) Detection**

SOTA OOD detection technique evaluation (EfficientNet-B0 as underlying)

- ViM, ReAct, ASH, MSP, ODIN 
- results: traditional semantics-centric framework for ODD detection (aims to distinguish between in-distribution (ID) data and semantically shifted OOD (S-OOD) data) struggles with ImageNet-ES (Covariate-shifted / C-OOD) 

Model-Specific OOD evaluation: defines OOD based on whether a sample is correctly / incorrectly classified by model 
- older methods (MSP, ODIN) showed more desirable correlation between accuracy and OOD score. 
- newer methods (ASH, ReAct, ViM) effective for S-OOD but accepted misclassified C-OOD samples as in distribution. 
→ potential bias in recent OOD search to handle semantic shifts at the expense of robustness to covariate shifts. 


**Experiment 2. Domain Generalization**

finetuned a ResNet-50 model on a subset of ImageNet by 
- digital augmentations (color jitter, DeepAugment, AugMix) 
- include image from ImageNet-ES validation set 

Results: 
- Digital augmentations improved robustness on ImageNet-C (simulated corruptions), but degraded performance on ImageNet-ES (real-world shifts) 
- Learning from the real-world perturbations in ImageNet-ES in addition to digital augmentations improved model robustness on ImageNet-ES, and even led to gains on conventional ImageNet-C benchmark 


**Experiment 3. Sensor Parameter Control**

- Evaluated various models (ResNet-50, ResNet-152, EfficientNet, SwinV2) on different subsets of ImageNet-ES test set (auto exposure settings, all manual parameter settings, and the single best manual parameter setting for each original image) 
- Results: 
    - domain generalization techniques and pretraining on larger datasets improved robustness on ImageNet-ES. However, tuning sensor parameters proved to be remarkably effective. It surpassed benefits of larger model sizes, more training data, and advanced architectures. 
    - Auto exposure mode didn’t provide optimal parameters for model prediction, often degrading performance. 
    - lightweight model like EfficientNet-B0 could outperform much larger, domain-generalized models like OpenCLIP-h 
    - Images visually preferred by humans didn’t correspond to best model prediction yielding images (e.g. overexposed is better..) 

**Conclusion & Future Work**

- ImageNet-ES: first benchmark focusing on real-world covariate shifts in environments and sensor domains 
- limitations in current OOD detection methods when faced with such shifts 
- potential of camera sensor control as a means to improve model performance 
- Future work: 
    - replacing displayed images with real 3D objects.
    - combining sensor control with digital post-processing. 
    - address the challenge of training neural networks for sensor control without requiring extra photos

