# RoboPour 🦾  
**Learning Accurate Pouring Behaviors for Warehouse Automation**

RoboPour is a **Physical AI system** that learns accurate and repeatable pouring behaviors using **real robotic hardware**.  
The system targets **warehouse automation scenarios**, specifically **dispensing screws and small parts** through controlled pouring motions.

We collect teleoperation demonstrations on real robots using **[SOLO CLI](chatgpt://generic-entity?number=0)** and **[LeRobot](chatgpt://generic-entity?number=1)**, and train an **ACT (Action Chunking with Transformers)** policy to enable stable, high-precision pouring in the real world.

---

## ✨ Key Features

- 🤖 Real-robot learning (no simulation-only training)
- 🎮 Teleoperation data collection with SOLO CLI
- 🧠 Transformer-based ACT policy
- 📦 Designed for warehouse dispensing tasks (e.g. screws, small components)
- 🔁 Accurate, smooth, and repeatable pouring behaviors
- 🔐 Safe and fast checkpoint loading via Safetensors

---

## 🧠 System Overview

### Data Collection
- Teleoperation demonstrations are collected on real robots
- Data is recorded using LeRobot-compatible pipelines
- Demonstrations include:
  - Robot observations
  - Continuous pouring actions
  - Long-horizon motion sequences required for dispensing

### Policy Learning
- We train an **ACT (Action Chunking with Transformers)** policy
- ACT enables:
  - Temporal abstraction over long action sequences
  - Stable execution of pouring motions
  - Reduced error accumulation compared to step-wise policies

### Deployment
- The trained policy is deployed back onto real robotic hardware
- Observation preprocessing and action postprocessing ensure:
  - Correct normalization
  - Safe action scaling
  - Reliable real-world execution

The result is a **robust and repeatable pouring behavior** suitable for warehouse automation.

---

## 📁 Repository Structure

├── model.safetensors                 # Trained ACT policy weights
├── config.json                       # Policy configuration
├── train_config.json                 # Training configuration (reproducibility)
├── policy_preprocessor.json          # Observation preprocessing config
├── policy_postprocessor.json         # Action postprocessing config
├── policy_preprocessor_step_.safetensors
├── policy_postprocessor_step_.safetensors
└── README.md

### File Descriptions

- **`model.safetensors`**  
  Trained ACT policy weights.

- **`config.json`**  
  Policy architecture and inference configuration.

- **`train_config.json`**  
  Full training configuration used to reproduce the policy.

- **`policy_preprocessor.json`**  
  Observation normalization and preprocessing rules.

- **`policy_postprocessor.json`**  
  Action scaling, limits, and postprocessing rules.

---

## 🚀 Usage (Inference)

> This repository contains **model checkpoints and configuration files only**.  
> Robot drivers, environment setup, and policy loaders should be implemented in your own system.

### Example (PyTorch-style loading)

```python
import json
from safetensors.torch import load_file

# Load policy configuration
with open("config.json", "r") as f:
    policy_cfg = json.load(f)

# Build policy (user-defined)
policy = build_policy_from_config(policy_cfg)

# Load weights
state_dict = load_file("model.safetensors")
policy.load_state_dict(state_dict)
policy.eval()
