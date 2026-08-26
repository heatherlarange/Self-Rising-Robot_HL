# robo1 getup PPO

A trained PPO model enabling the 2-DOF robot `robo1` (in MuJoCo) to stand up from a fallen position.

## Files

The following files are required to run this repository:

- `robo1_getup_ppo.zip` - Trained Stable-Baselines3 PPO model
- `robo1_env.py` - Gymnasium/MuJoCo environment
- `robo1.xml` - MuJoCo model definition
- `assets/` - STL meshes referenced by `robo1.xml`
- `play_robo1_policy.py` - Playback of the trained model
- `eval_robo1_policy.py` - Evaluation from various fallen postures
- `search_all_getup.py` - Searches for candidate get-up trajectories for each fallen posture and outputs `BEST_SEQ` for use in `getup_reference.py`
- `getup_reference.py` - Sequences of servo target angle waypoints for each fallen posture, used for generating teacher data
- `scripted_getup.py` - Verifies playback of the get-up trajectories defined in `getup_reference.py` using the MuJoCo viewer
- `pretrain_robo1_from_scripted.py` - Generates observation-teacher action pairs from the trajectories in `getup_reference.py` to pre-train the PPO policy
- `train_robo1.py` - Additional training using PPO
- `export_policy_header.py` - Generates a C header file for Arduino from the trained model
- `requirements.txt` - Python dependency packages

## Setup

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Play

```powershell
python play_robo1_policy.py
```

To play back from a specific initial pose:

```powershell
python play_robo1_policy.py --pose roll_pos
python play_robo1_policy.py --pose roll_neg
python play_robo1_policy.py --pose pitch_pos
python play_robo1_policy.py --pose pitch_neg
```

## Evaluate

```powershell
python eval_robo1_policy.py
```

## Training

Pre-train an initial policy using the get-up demonstration data defined in `getup_reference.py`.

To re-search for get-up trajectories, run the following:

```powershell
python search_all_getup.py
```
Update the `getup_sequence_for_pose()` function in `getup_reference.py` with the generated `BEST_SEQ` or `REFERENCE_CANDIDATES` array.


Pre-train the initial policy using the demonstration data:

```powershell
python pretrain_robo1_from_scripted.py
```

This generates `robo1_getup_ppo.zip`.

Next, perform additional training using PPO:

```powershell
python train_robo1.py --model-in robo1_getup_ppo.zip --timesteps 200000 --n-envs 6 --model-out robo1_getup_ppo
```

To train with PPO from scratch, omit the `--model-in` argument:

```powershell
python train_robo1.py --timesteps 200000 --n-envs 6 --model-out robo1_getup_ppo
```

## Export for Arduino

Generate `policy_network.h` from the trained model.

```powershell
python export_policy_header.py robo1_getup_ppo.zip -o policy_network.h
```

Place the generated `policy_network.h` in your Arduino sketch to use it.

## Notes

- `robo1_getup_ppo.zip` depends on the environment definitions in `robo1.xml` and `robo1_env.py`.
- The MuJoCo model cannot be loaded without the STL files located in `assets/`.
- Please execute the command from the repository root directory.
