# Proposed Report Structure

## Chapter 1: Introduction (2–3 pages)

- Motivation: increasing RAN complexity and the need for intelligent scheduling.
- Problem statement.
- Objectives.
- Contributions.

Keep this chapter concise and focused.

---

## Chapter 2: Background (8–12 pages)

Build the radio concepts from the bottom up.

### 2.1 NR Resource Grid

- Frame → Subframe → Slot
- Resource Element (RE)
- Physical Resource Block (PRB)

Include one figure showing the NR resource hierarchy.

### 2.2 Radio Resource Scheduling

Explain:

- Round Robin (RR)
- QoS scheduler
- Proportional Fair (PF) metric
- Buffer-aware scheduling

### 2.3 srsRAN Architecture

Cover:

- gNB
- MAC scheduler
- Open5GS
- Near-RT RIC
- xApp

Include architecture figures such as:

- srsRAN + Open5GS architecture
- O-RAN Near-RT RIC architecture
- O-RAN protocol stack

### 2.4 Scheduler Data Flow

Show how one scheduling decision becomes one replay sample.

Example workflow:

```text
UE reports (CQI, Buffer)
          │
          ▼
    MAC Scheduler
          │
          ▼
 Scheduling Decision
          │
          ▼
 Replay Dataset
```

---

## Chapter 3: Literature Review (5–7 pages)

Keep this chapter focused.

### 3.1 Classical Scheduling

- Round Robin
- QoS / Proportional Fair scheduling

### 3.2 AI-Based Scheduling

Summarize recent work on:

- DQN schedulers
- Conservative Q-Learning (CQL)
- Implicit Q-Learning (IQL)
- Offline Reinforcement Learning

### Research Gap

> Existing work evaluates offline RL schedulers, but limited comparisons exist between supervised ranking, CQL, and IQL under identical srsRAN replay data.

---

## Chapter 4: Methodology

This is the core chapter containing your contribution.

### 4.1 Testbed

Include:

- srsRAN
- Open5GS
- Near-RT RIC
- xApp
- 2T2R and 4T4R configurations

Include a testbed architecture figure.

### 4.2 Data Collection

Explain:

- Scheduler logs
- Replay buffer creation
- State variables

Example state representation:

| Feature | Meaning |
|---------|---------|
| CQI | Channel quality |
| Buffer | Queue size |
| Avg Rate | Historical throughput |
| Estimated Rate | Instantaneous achievable rate |
| PF Metric | Fairness indicator |

### 4.3 Model Architectures

Split this section into three subsections.

#### Priority Ranker

- Supervised learning
- Neural network: `5 → 128 → 128 → 1`

#### Pairwise CQL

- Offline Reinforcement Learning
- Q-network
- Pairwise ranking objective

#### Pairwise IQL

- Q-network
- Value network
- Actor network

### 4.4 Training Pipeline

Include one workflow diagram.

Example:

```text
Replay Buffer
      │
      ▼
Feature Processing
      │
      ▼
Model Training
(CQL / IQL)
      │
      ▼
Export Weights
```

### 4.5 Model Optimization

Document the final hyperparameter tuning.

| Parameter | Tested | Selected |
|-----------|---------|----------|
| Learning Rate | `1e-4` – `1e-3` | `3e-4` |
| Expectile | `0.6` – `0.9` | `0.8` |
| Beta | `3` – `5` | `3` |
| Epochs | `25` – `75` | `30` |

---

## Chapter 5: Experimental Results

Follow the planned evaluation scenarios.

### Test Scenarios

| Scenario |
|----------|
| Fixed Buffer |
| Varying Buffer |
| Slow Fading |
| Homogeneous CQI (≈6–9.5) |
| 5 UEs |
| 10 UEs |
| 20 UEs |
| 2T2R |
| 4T4R |

### Evaluation Metrics

Define each metric once.

- Top-1 Recovery
- Spearman Correlation
- Average Reward
- Average PRBs
- Scheduled Precision

### Results Organization

For every scenario compare:

1. Round Robin
2. QoS Scheduler
3. Priority Ranker
4. Pairwise CQL
5. Pairwise IQL

Follow each table with a discussion of the observed behavior.

### Final Comparison Table

| Scheduler | Top-1 | Spearman | Avg Reward | Avg PRBs |
|-----------|--------|----------|------------|----------|
| Round Robin | — | — | — | — |
| QoS | — | — | — | — |
| Priority Ranker | — | — | — | — |
| Pairwise CQL | **95.5%** | **0.971** | **0.774** | **39.3** |
| Pairwise IQL | ~45% | ~0.55 | ~0.59 | ~29 |

---

## Chapter 6: Conclusion

Include:

- Summary of findings.
- Why Pairwise CQL performs best.
- Limitations of the current implementation.
- Future work.

### Future Work

- Online reinforcement learning.
- Larger antenna configurations (4T4R and beyond).
- Real-time xApp deployment.
- Multi-cell scheduling.
- Additional traffic models and mobility scenarios.
