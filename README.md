# 5G Downlink Power Control to Maximize Sum Throughput Using Reinforcement Learning

This project implements a reinforcement learning (RL) approach for downlink (DL) power control in 5G networks to mitigate interference, enhance signal-to-interference-plus-noise ratio (SINR), and maximize sum throughput.

**Applicable Release**: NetSim v14.1 or higher  
**Project Link**: [Download Project](https://github.com/NetSim-TETCOS/RL_Based_Tx_Power_Control_v14.1/archive/refs/heads/main.zip)

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Implementation in NetSim](#implementation-in-netsim)
3. [Reinforcement Learning Algorithms](#reinforcement-learning-algorithms)
4. [Interfacing with OpenAI Gymnasium](#interfacing-with-openai-gymnasium)
5. [Running the Simulation](#running-the-simulation)
6. [System Requirements and Tips](#system-requirements-and-tips)
7. [Troubleshooting](#troubleshooting)
8. [License](#license)

---

## Project Overview

Due to the broadcast nature of wireless communication, signals interfere with each other, potentially degrading the performance of the 5G Radio Access Network (RAN). Downlink power control can help mitigate interference, ensure spectral reuse, and improve user experience. However, practical implementations often face limitations due to dependencies on various simplifying assumptions like full knowledge of channel gains, user positions, and resource allocation.

In this project, we use reinforcement learning for downlink power control to boost SINR and maximize sum throughput.

### Key Reference
- Ghadimi et al., “A Reinforcement Learning Approach to Power Control and Rate Adaptation in Cellular Networks,” arXiv:1611.06497v1 [math.OC].

---

## Implementation in NetSim

The power control mechanism in this project is implemented in the NetSim environment, which provides a TCP socket connection to facilitate interaction with an RL agent written in Python. Each simulation episode consists of multiple steps where the agent updates power levels in response to SINR feedback, allowing it to learn optimal control policies over time.

### Agent-Environment Interactions

1. **Initialize a Listening Socket** in NetSim that binds to any available port.
2. **For Each Episode**:
   - Start a new simulation and create a client socket in Python that binds to the same port.
   - Exchange power levels and SINR values with NetSim, allowing the RL agent to update the power control policy.
   - NetSim updates SINRs based on the received power control values, calculates rewards, and sends this data back to Python.
3. **End of Episode**: Terminate the client connection.

---

## NetSim-Python Interfacing

Python's `socket` module is used to facilitate communication with NetSim's C functions over a TCP connection.

#### Example Python Function for Interfacing:
```python
def NETSIM_interface(gNB_powers):
    # Sends the gNB powers to NetSim and receives SINR values and rewards
