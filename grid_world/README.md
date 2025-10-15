# Reinforcement Learning in Grid World Environment

## 1. Overview
This project implements and compares **Q-learning** and **SARSA** algorithms within a static 11×11 Grid World environment.  
It further introduces a **Teacher–Student Reinforcement Learning Framework**, where a pre-trained agent provides probabilistic guidance to a student agent.  
The study investigates how teacher advice availability and accuracy influence learning speed and overall policy performance.

---

## 2. Project Structure
```text
GRID_WORLD/
│
├── env.py                     # GridWorld environment definition
├── grid.ipynb                 # Main notebook (Q-learning, SARSA, Teacher Framework)
│
├── images/                    # Visual assets for rendering the environment
│   ├── grid_agent.png
│   ├── grid_goal_position.png
│   └── grid_obstacles.png
├── .gitignore
└── README.md
```
---

## 3. Algorithms Implemented

### (1) Q-learning
A value-based reinforcement learning algorithm using the **max-Q target** update rule:

\[
Q(s, a) \leftarrow Q(s, a) + \alpha [r + \gamma \max_{a'} Q(s', a') - Q(s, a)]
\]

- Uses ε-greedy action selection with exponential decay  
- Tracks reward, success rate, and average steps per episode  
- Produces reward curves and success metrics  

---

### (2) SARSA
Implements on-policy temporal difference learning:

\[
Q(s, a) \leftarrow Q(s, a) + \alpha [r + \gamma Q(s', a') - Q(s, a)]
\]

- Shares identical hyperparameters with Q-learning  
- Used for direct performance comparison  
- Outputs include learning curves and average success rate  

---

### (3) Teacher–Student Framework
A pre-trained “teacher” agent provides guidance to the “student” agent based on two key factors:

| Parameter | Description | Values Tested |
|------------|--------------|----------------|
| **Availability** | Probability that the teacher gives advice | {0.1, 0.3, 0.5, 0.7, 1.0} |
| **Accuracy** | Probability that advice is correct | {0.1, 0.3, 0.5, 0.7, 1.0} |

Behavior:
- With probability = availability, the teacher offers advice.  
- The advice is correct with probability = accuracy.  
- Incorrect advice leads to suboptimal exploration.  

Outputs include:
- Reward curves
- Success rate trends
- Heatmaps showing combined teacher impact

---

## 4. Environment Specification

- Grid size: **11 × 11**
- Start position: **(0, 0)**
- Goal position: **(10, 10)**
- Obstacles: fixed 10-cell configuration
- Step penalty: −1  
- Goal reward: +25  
- Obstacle penalty: −10  
- Episode termination: reaching goal or max steps

The environment logic is implemented in `env.py` as a class `GridWorldEnv`, supporting:
- `reset()` – resets environment  
- `step(action)` – executes one move and returns next state, reward, done flag  
- `render()` – visualizes the grid using images in `/images`  

---

## 5. Hyper-Parameters
Identical across all tasks:

| Parameter | Symbol | Value |
|------------|---------|-------|
| Learning rate | α | 0.1 |
| Discount factor | γ | 0.95 |
| Epsilon (start → end) | ε | 1.0 → 0.05 |
| Episodes | – | 450 |
| Max steps per episode | – | 100 |

---

## 6. Evaluation Metrics
For each run, the following are computed:
- **Average reward per episode**
- **Success rate (%)**
- **Average steps to reach goal**

---

## 7. Visualizations
- Raw episode rewards (faded for clarity)
- 50-episode moving averages
- Success-rate rolling windows
- Heatmaps: availability × accuracy (teacher influence)
- Teacher impact summary (best vs baseline)

---

## 8. Results Summary
| Model | Teacher | Avg Reward | Success Rate | Observation |
|--------|----------|-------------|---------------|--------------|
| Q-learning | None | Moderate | Stable | Fast convergence, higher variance |
| SARSA | None | Slightly lower | Stable | Conservative learning curve |
| Q-learning | With Teacher | Higher | ↑↑ | Teacher accelerates convergence |
| SARSA | With Teacher | Medium | ↑ | Benefits less from advice noise |

---

## 9. How to Run

### Step 1 — Install Dependencies
```bash
pip install numpy matplotlib seaborn pygame tqdm
```
### Step 2 — Run the Notebook

10. Notes
	•	The environment visualization (env.render()) requires a local graphical session (e.g., macOS, Windows).
	•	Disable rendering if running in headless mode (e.g., remote Linux).
	•	Random seeds are fixed for reproducibility.
	•	Teacher parameters can be adjusted in the nested loops for further experiments.

⸻

11. License

Copyright (c) 2025 Benson Liu

All rights reserved.

This repository is for educational and demonstration purposes only.
The implementation details and source code are withheld to prevent academic misuse.
Reproduction, redistribution, or adaptation of the materials in this repository
is strictly prohibited without explicit permission from the author.
