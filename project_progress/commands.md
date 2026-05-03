# Initital Tiny Baseline Command

## First Attempt
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

## 2. Without PSL (Isolate RL + MuJuCo + Robosuite)

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

## 3. Tiny Smoke Test (to test if RL runs, seed 2)

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

## 4. Tiny Test (OSMesa, psl=true, seed 2)

MUJOCO_GL=osmesa HYDRA_FULL_ERROR=1 python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=2 \
debug=True \
save_video=False \
num_train_frames=1000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=500 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=True \
path_length=25 \
action_repeat=1 \
experiment_id=smoke_lift_psl_true_1k_seed2_osmesa \
device=cpu

## 5. psl=true, 6000 frames, eval=1000
MUJOCO_GL=osmesa HYDRA_FULL_ERROR=1 python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=2 \
debug=True \
save_video=False \
num_train_frames=6000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=1000 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=True \
path_length=25 \
action_repeat=1 \
experiment_id=lift_psl_true_6k_seed2_osmesa_eval1k \
device=cpu

## 6. Start training at 1000 (num_seed_frames=1000) + diagnose file
MUJOCO_GL=osmesa HYDRA_FULL_ERROR=1 python planseqlearn/train.py \
agent=drqv2 \
use_wandb=False \
seed=2 \
debug=True \
save_video=False \
num_train_frames=2000 \
num_seed_frames=1000 \
camera_name=robot0_eye_in_hand \
eval_every_frames=500 \
num_eval_episodes=1 \
task=robosuite_Lift \
psl=True \
path_length=25 \
action_repeat=1 \
experiment_id=diagnose_psl_train_start_seed1k \
device=cpu 2>&1 | tee diagnose_psl_train_start_seed1k.log