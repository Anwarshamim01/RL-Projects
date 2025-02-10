---
title: "Autonomous Drone Navigation in 3D Grid World using Deep Q-Learning (DQN)"
author: Anwar Shamim
date: "Last Updated: `r Sys.Date()`"
output:
  html_document:
    toc: true
    toc_float: true
    theme: cosmo
    highlight: tango
---

# Overview

This project implements an **autonomous drone navigation system** in a **3D grid world** using **Deep Q-Networks (DQN)** with GPU support. The drone learns to navigate from a starting position to a goal while avoiding obstacles. The implementation includes:

- A custom Gym environment (`DroneNavigationEnv`) that simulates the 3D grid world.
- A DQN agent with experience replay and a target network for stable training.
- Visualization of training progress and the learned navigation trajectory.

The goal is to demonstrate how reinforcement learning can be applied to solve complex navigation problems in structured environments.

---

# Table of Contents

1. [Introduction](#introduction)
2. [Environment Description](#environment-description)
3. [Methodology](#methodology)
4. [Implementation Details](#implementation-details)
5. [Training Process](#training-process)
6. [Results and Visualization](#results-and-visualization)
7. [Dependencies](#dependencies)
8. [How to Run](#how-to-run)
9. [Future Work](#future-work)

---

# Introduction

Reinforcement Learning (RL) has emerged as a powerful tool for solving decision-making problems in dynamic environments. In this project, we apply RL to train a drone to navigate a **3D grid world** with obstacles. The drone's objective is to reach a predefined goal position while avoiding collisions and minimizing travel time.

We use a **Deep Q-Network (DQN)** to approximate the Q-function, enabling the drone to learn optimal policies through trial and error.

---

# Environment Description

The environment is a **3D grid world** with the following characteristics:

- **Grid Size**: Configurable (default is 10x10x10).
- **Start Position**: `(0, 0, 0)`.
- **Goal Position**: `(grid_size-1, grid_size-1, grid_size-1)`.
- **Obstacles**: Predefined positions (e.g., `(3, 3, 3)`, `(5, 5, 5)`).

The drone can move in six discrete directions: forward, backward, left, right, up, and down. The environment provides rewards/penalties based on the drone's actions:
- **+10** for reaching the goal.
- **-20** for hitting an obstacle.
- **-5** for going out of bounds.
- **-1** for each step (to encourage faster navigation).

---

# Methodology

We employ a **Deep Q-Learning (DQN)** approach to train the drone. Key components include:

1. **Policy Network**: A neural network that approximates the Q-function.
2. **Target Network**: A separate network used to stabilize training by providing consistent Q-value targets.
3. **Experience Replay**: Stores past experiences to break temporal correlations and improve sample efficiency.
4. **Epsilon-Greedy Exploration**: Balances exploration and exploitation during training.

The DQN is trained using the following hyperparameters:
- **Discount Factor (γ)**: 0.99
- **Learning Rate**: 0.001
- **Batch Size**: 32
- **Epsilon Decay**: 0.995
- **Target Network Update Frequency**: Every 10 episodes

---

# Implementation Details

The implementation consists of the following components:

1. **Custom Gym Environment**: `DroneNavigationEnv` defines the 3D grid world and handles state transitions, rewards, and rendering.
2. **DQN Model**: A three-layer fully connected neural network implemented in PyTorch.
3. **Replay Buffer**: Stores and samples experiences for training.
4. **Training Function**: Handles the training loop, including epsilon decay and periodic updates to the target network.
5. **Testing Function**: Evaluates the trained policy and generates a trajectory visualization.

---

# Training Process

The training process involves running the drone through multiple episodes in the environment. During each episode:
1. The drone starts at the initial position.
2. It selects actions using an epsilon-greedy policy.
3. Experiences are stored in the replay buffer.
4. The policy network is updated using sampled batches from the replay buffer.
5. The target network is periodically synchronized with the policy network.

Training progress is visualized by plotting the total reward per episode over time.

---

# Results and Visualization

After training, the drone successfully learns to navigate the grid world. The results include:

1. **Training Progress Plot**: Shows the total reward per episode, indicating how the drone improves over time.
2. **Trajectory Visualization**: Displays the drone's path in 3D space, highlighting obstacles, the start position, and the goal.

Below is an example of the trajectory visualization:

![Trajectory Visualization](path_to_image.png)

---

# Dependencies

This project requires the following Python libraries:
- `numpy`
- `gym`
- `matplotlib`
- `torch`
- `random`
- `collections`

To install the dependencies, run:
```bash
pip install numpy gym matplotlib torch