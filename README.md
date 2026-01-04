# 360° Multi-Modal Velocity Fusion for Autonomous Vehicles

A high-performance **mid-fusion neural network** for ego-vehicle velocity estimation using **360° surround-view cameras** and **LiDAR Bird’s Eye View (BEV)** representations.  
This project demonstrates robust sensor fusion, real-time preprocessing, and scalable perception design inspired by modern autonomous driving stacks.

---

## 📌 Project Overview

This system predicts vehicle velocity (m/s) by fusing data from **7 asynchronous sensors**:
- **6 surround-view RGB cameras**
- **1 LiDAR sensor projected into BEV space**

Unlike single-sensor approaches, the model performs **feature-level (mid) fusion**, enabling accurate ego-motion estimation even under occlusions, sparse LiDAR returns, and complex urban dynamics.

The pipeline is built on **nuScenes-mini** and optimized for **real-time preprocessing and inference**.

---

## 🚀 Key Features

- **360° Surround-View Perception**
  - Six camera feeds: Front, Front-Left, Front-Right, Back, Back-Left, Back-Right
  - Shared **ResNet-18** backbone with time-distributed encoding

- **LiDAR BEV Voxelization**
  - Raw `.bin` point clouds converted into **2-channel BEV maps**
    - Height Map
    - Density Map
  - Fully vectorized NumPy implementation for sub-millisecond preprocessing

- **Mid-Level Feature Fusion**
  - Concatenates semantic visual features with geometric LiDAR representations
  - Preserves spatial context while remaining computationally efficient

- **Real-Time Command Center Dashboard**
  - 360° stitched camera HUD
  - BEV LiDAR visualization
  - Velocity prediction overlay

---

## 🛠 Tech Stack

| Component        | Tools / Libraries |
|------------------|------------------|
| Framework        | PyTorch, TorchVision |
| Dataset          | nuScenes SDK (mini) |
| Data Processing  | NumPy, PIL |
| Visualization    | OpenCV, Matplotlib |
| Optimization     | Huber Loss |

---

## 🧠 Model Architecture

The network consists of three main components:

### 1️⃣ Vision Branch
- Shared **ResNet-18** encoder
- Processes each camera independently
- Outputs **512-dim feature vectors × 6 cameras**

### 2️⃣ LiDAR Branch
- 2D CNN over BEV grid
- Learns spatial occupancy and structure
- Outputs a **32 × H × W** feature embedding

### 3️⃣ Fusion & Regression Head
- Feature concatenation (Vision + LiDAR)
- **3,104-dimensional fused representation**
- Fully connected regressor
- Outputs **scalar ego-velocity (m/s)**

---

## 📊 Results & Visualization

- Stable convergence using **Huber Loss**
- Robust to:
  - Noisy LiDAR returns
  - Ego-pose drift in nuScenes metadata
  - High-acceleration frames



---

## 📂 Project Structure

```plaintext
├── data/                  # nuScenes-mini dataset
├── notebooks/
│   └── fusion_main.ipynb  # Training and inference pipeline
├── models/
│   └── kodiak_fusion.pth  # Trained model weights
├── outputs/               # Visualizations and HUD recordings
├── requirements.txt
└── README.md
