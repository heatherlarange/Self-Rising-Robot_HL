# SelfRisingRobot

Robot that executes stand-up motion using two servos and reinforcement learning in MuJoCo

source:
https://homemadegarbage.com/rl13

## Contents

- `3Dmodel/` - 3D print models for the physical robot
- `RL/` - MuJoCo model, reinforcement learning environment, and trained PPO model
- `Arduino/` - M5Atom sketch and exported policy network

Please refer to the README in each folder for detailed usage instructions.

## 3D Model

The `3Dmodel/` folder contains STL files for building the physical robot.

- `footP.stl`
- `arm1P.stl`
- `arm2P.stl`
- `armhornP.stl`

## Reinforcement Learning

The `RL/` folder contains the simulation and trained model.

Key contents:

- MuJoCo model
- Gymnasium environment
- Trained Stable-Baselines3 PPO model
- Playback and evaluation scripts

## Arduino

The `Arduino/` folder contains the M5Atom sketch for controlling the physical robot.

Key contents:

- `robo03.ino`
- `policy_network.h`

`policy_network.h` is a C header file containing the trained policy, used to execute the stand-up motion on the M5Atom.