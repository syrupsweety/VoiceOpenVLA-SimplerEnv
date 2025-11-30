# VoiceOpenVLA: An Intelligent Control System for Robot Manipulators using Natural Language

This repository contains the implementation of a voice and text-based intelligent control system for robot manipulators, developed as part of my Bachelor's thesis at Peter the Great St. Petersburg Polytechnic University.

## Overview

VoiceOpenVLA is an integrated software and hardware system that enables a robotic manipulator to understand and execute commands given in natural language, either via text input or spoken voice. The system combines state-of-the-art models from computer vision, natural language processing, and robotics:

- **OpenVLA**: An open-source Vision-Language-Action (VLA) model used to interpret multimodal inputs and generate robotic actions.
- **SLAM-ASR**: A speech recognition method based on HuBERT and a trainable linear projector, enabling robust voice command interpretation without full retraining of the large language model.

The integration allows for real-time, zero-shot control of a robot in dynamic environments, with applications in household assistance and medical robotics.

## Key Features

- Support for both **text and voice commands**
- End-to-end pipeline from audio input to robot action
- Integration of OpenVLA with SLAM-ASR for unified multimodal understanding
- Optimizations including model quantization, caching, and noise filtering
- High task completion success rate:
  - **84.1%** with text input
  - **78.8%** with voice input
- Tested in SimplerEnv virtual environment under various conditions:
  - Known tasks and objects
  - Distracting objects
  - Unseen tasks and novel object combinations

## Architecture

The system processes input through the following stages:

1. **Audio Processing**:
   - Audio is processed using the HuBERT self-supervised speech model.
   - A two-layer linear projector maps audio embeddings into the LLM’s latent space (4096-dimensional).
2. **Multimodal Fusion**:
   - Visual data (from camera feed) is encoded using ViT.
   - Textual instruction (or transcribed speech) is processed by the LLM (Llama 2 backbone).
   - Audio embeddings are injected via attention masking to avoid modality conflict.
3. **Action Generation**:
   - The fused representation is used by OpenVLA to predict robot actions.
   - Actions include 3D world vector (`x`, `y`, `z`), rotation delta (`roll`, `pitch`, `yaw`), and gripper state (`grasp`).


## Usage

### Running Inference

```python
# Before running, download linear projector from HF
hf download syrupsweety/SLAM-ASR-VoiceOpenVLA-projector model.pt
```

## Results

Evaluation was performed across multiple categories:

| Category | Text Input Success | Voice Input Success |
|--------|---------------------|----------------------|
| Training Tasks | 8.6 / 10 | 8.3 / 10 |
| With Distractors | 7.6 / 10 | 6.9 / 10 |
| Novel Tasks/Objects | 9.1 / 10 | 8.8 / 10 |

Overall success rates:
- **Text Input**: 84.1%
- **Voice Input**: 78.8%

## Publications

This work is based on the following research:

- Kim M. J. et al. *OpenVLA: An Open-Source Vision-Language-Action Model* ([arXiv:2406.09246](https://arxiv.org/abs/2406.09246))
- Hsu W. N. et al. *HuBERT: Self-Supervised Speech Representation Learning* ([arXiv:2106.07447](https://arxiv.org/abs/2106.07447))
- Ma Z. et al. *An Embarrassingly Simple Approach for LLM with Strong ASR Capacity* ([arXiv:2402.08846](https://arxiv.org/abs/2402.08846))

## Author

**Nikita Buraev**  
Bachelor's Student, Mechatronics and Robotics  
Institute of Mechanical Engineering, Materials and Transport  
Higher School of Automation and Robotics  
Peter the Great St. Petersburg Polytechnic University
