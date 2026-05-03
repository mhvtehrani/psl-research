# Test Command Log

## 1. First Attempt, Initital Baseline Command

python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=1 \
debug=True \
save_video=False \
num_train_frames=10000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=2000 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=True \
path_length=25 \
action_repeat=1 \
experiment_id=test_lift_psl

## 2. Initial Baseline Command, without PSL (Isolate RL + MuJuCo + Robosuite)

HYDRA_FULL_ERROR=1 python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=1 \
debug=True \
save_video=False \
num_train_frames=10000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=2000 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=False \
path_length=25 \
action_repeat=1 \
experiment_id=test_lift_no_psl \
device=cpu 

**System Crashed at ~8000 frames**

## 3. Tiny Smoke Test (to test if RL runs)

HYDRA_FULL_ERROR=1 python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=2 \
debug=True \
save_video=False \
num_train_frames=6000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=2000 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=False \
path_length=25 \
action_repeat=1 \
experiment_id=repeat_lift_no_psl_6k \
device=cpu

