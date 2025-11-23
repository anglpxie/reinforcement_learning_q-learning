# Gridworld Q-Learning: Reinforcement Learning Implementation

**Final Exam Project - Reinforcement Learning Course**

The implementation and comparison of Q-Learning and SARSA algorithms in a 4×4 gridworld environment, featuring tracking, visualization, and analysis tools.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Environment Description](#environment-description)
- [Implemented Algorithms](#implemented-algorithms)
- [Key Features](#key-features)
- [Results Summary](#results-summary)
- [Project Structure](#project-structure)
- [Installation & Usage](#installation--usage)
- [Technical Implementation](#technical-implementation)
- [Visualizations](#visualizations)
- [References](#references)

---

## Project overview

This project implements Q-learning in a custom gridworld environment and compares it to another fundamental Temporal Difference (TD) learning algorithm - SARSA.

- **Q-Learning** (Off-Policy TD control)
- **SARSA** (On-Policy TD control)

### Learning Objectives Demonstrated

- Understanding of TD Learning methods
- Implementation of value-based RL algorithms
- Policy evaluation and improvement
- Exploration-exploitation trade-off (ε-greedy)
- On-policy vs Off-policy learning
- Algorithm comparison and analysis

---

## Environment description

### Gridworld layout

```
[·, ·, ·, G]  ← Goal at (0,3), reward +10
[·, X, ·, ·]  ← Wall at (1,1), reward -1
[·, ·, ·, ·]
[S, ·, ·, ·]  ← Start at (3,0)
```

### Environment characteristics

- **State space:** 16 states (4×4 grid)
- **Action space:** 4 actions (Up, Right, Down, Left)
- **Rewards:**
  - Goal state: +10
  - Wall state: -1
  - All other transitions: 0
- **Dynamics:** Deterministic (no stochasticity)
- **Optimal path:** 6 steps from start to goal

### State transitions

The agent cannot move outside grid boundaries (actions at edges result in staying in place).

---

## Implemented algorithms

### 1. Q-Learning (Off-Policy) - MAIN

**Update Rule:**
```
Q(s,a) ← Q(s,a) + α[R + γ·max_a' Q(s',a') - Q(s,a)]
```

**Characteristics:**
- **Off-policy:** Learns optimal policy while following ε-greedy policy
- **Optimistic:** Updates assume the best possible next action
- **Faster convergence:** In deterministic environments

### 2. SARSA (On-Policy)

**Update Rule:**
```
Q(s,a) ← Q(s,a) + α[R + γ·Q(s',a') - Q(s,a)]
```
where `a'` is the actual next action chosen by the current policy.

**Characteristics:**
- **On-policy:** Learns about the policy it's following
- **Conservative:** Updates reflect actual exploration
- **Safer training:** In risky environments
- **More realistic:** Accounts for exploration in value estimates

### Common Parameters

| Parameter | Symbol | Value | Description |
|-----------|--------|-------|-------------|
| Learning Rate | α | 0.1 | Step size for Q-value updates |
| Discount Factor | γ | 0.9 | Importance of future rewards |
| Initial Epsilon | ε₀ | 1.0 | Initial exploration rate |
| Final Epsilon | ε_min | 0.01 | Minimum exploration rate |
| Decay Factor | - | 0.995 | Epsilon decay per episode |
| Episodes | - | 1000 | Total training episodes |

---

## Project features

### 1. Tracking

- **Temporal Difference (TD) error monitoring**
  - Tracks mean TD error per episode
  - Detects Q-table convergence (TD error < 0.001)

- **Optimal policy detection**
  - Automatically identifies when agent finds optimal 6-step path
  - Monitors consistency over 50-episode window

- **Performance metrics**
  - Rewards per episode
  - Steps to goal per episode
  - Epsilon decay trajectory

### 2. Visualizations

#### Training analysis (2×2 Grid)
- Learning curve: Rewards over episodes
- Learning curve: Steps to goal
- Epsilon decay with optimal policy marker
- TD Error on log scale (convergence indicator)

#### Policy & value visualization
- Policy arrows showing learned direction at each state
- State value function heatmap (V*)
- Color-coded special states (Start, Goal, Wall)

#### Algorithm comparison
- Side-by-side reward comparison
- Side-by-side steps comparison
- Statistical summary with convergence metrics

### 3. Q-Table Analysis

Flattened Q-table display showing:
- All 16 states with (row, col) coordinates
- Q-values for each action
- Best action at each state
- Maximum Q-value per state

---

## Results Summary

### Q-Learning performance

```
============================================================
  Total Episodes: 1000
  Optimal Policy Found: Episode 905
  Q-Table Converged: Episode XXX
  Final Epsilon: 0.0100
  Final Avg Reward (last 50 ep): 10.00
  Final Avg Steps (last 50 ep): 6.06 ± 0.31
============================================================
```

### SARSA performance

```
============================================================
  Total Episodes: 1000
  Optimal Policy Found: Episode 866
  Final Epsilon: 0.0100
  Final Avg Reward (last 50 ep): 10.00
  Final Avg Steps (last 50 ep): 6.04 ± 0.28
============================================================
```

### Algorithm comparison

| Metric | Q-Learning | SARSA | Winner |
|--------|------------|-------|--------|
| **Convergence Speed** | Episode 905 | Episode 866 | SARSA |
| **Final Reward** | 10.00 ± 0.00 | 10.00 ± 0.00 | same |
| **Final Steps** | 6.06 ± 0.31 | 6.04 ± 0.28 | SARSA |
| **Stability** | Lower | Higher | SARSA |

**Key finding:** In this deterministic gridworld, both algorithms converge to the optimal policy. Although Q-learning is theoretically more suitable for this kind of RL problem, SARSA shows slightly faster convergence and lower variance.
