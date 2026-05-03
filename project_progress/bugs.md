# PSL Debug Log

# First Test (RL runs?)

## Run 1
**Command:**
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

**Error:**
ModuleNotFoundError: No module named 'dm_env'

**Cause (guess):**
Missing dependency

**Fix Attempted:**
Ran:
pip install dm_env

**Result:**
Bug fixed, pending rerun

## Run 2

**Error**
ModuleNotFoundError: No module named 'planseqlearn' 

**Cause**
package not properly installed into active conda environment

**Fix**
ran pip install -e

**Result**
installed package successfully, error resolved, pending rerun.

## Run 3

ModuleNotFoundError: No module named 'dm_control'

Also:
RuntimeError: module compiled against ABI version 0x1000009 but this version of numpy is 0x2000000

dm_control dependacny missing, NumPy incompatible with older version (?)

**Fix**
pip install dm_control

**Result**
installed successfully, pip reports PyOpenGL version conflict with pyrender, but proceeding to rerun.

## Run 4

ModuleNotFoundError: No module named 'd4rl.kitchen'

dependency error

**Fix**
cd ~/PSL/planseqlearn/d4rl
pip install -e .

## Run 5

ImportError: numpy.core.multiarray failed to import
NumPy version incompatible

**Fix**
pip install "numpy<2"
Downgraded NumPy from 2.2.6 to 1.26.4.

## Run 6
MuJoCo version incompatible

**Fix**
mkdir -p ~/.mujoco
cd ~/.mujoco
wget https://mujoco.org/download/mujoco210-linux-x86_64.tar.gz
tar -xvf mujoco210-linux-x86_64.tar.gz

Set environments in bash config ~/.bashrc

## Run 7
missing OSMesa
fatal error: GL/osmesa.h: No such file or directory

**Fix**
sudo apt-get update
sudo apt-get install -y libosmesa6-dev libgl1-mesa-dev libglfw3 patchelf

Cleared mujoco_py build cache.

## Run 8
ModuleNotFoundError: No module named 'groundingdino'

This is not an RL dependency, this is a part of a vision pipeline.
PSL command too complex! Current command activates PSL pipeline instead of baseline RL.

**Fix**
Run test command with psl=false to isolate RL+mujoco+robosuite for now, come back to psl=true later.

**New command**
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
experiment_id=test_lift_no_psl

**Result**
groundingdino imported even with psl=false

**Fix**
Installed local GroundingDINO package:
cd ~/PSL/planseqlearn/Grounded-Segment-Anything/GroundingDINO
pip install -e .

**Result**
groundingdino succesfully installed

## Run 9
ModuleNotFoundError: No module named 'rlkit.envs'

**Fix**
pip install -e rlkit

**Result**
successfully installed rlkit

## Run 10
ImportError: cannot import name 'Mapping' from 'collections' (/home/mhvtehrani/miniconda3/envs/psl/lib/python3.10/collections/__init__.py)

networkx is too old for Python 3.10

**Fix**
pip install "networkx>=2.6,<3"

## Run 11
ModuleNotFoundError: No module named 'mopa_rl'
missing folder in active psl environment

**Fix**
pip install -e mopa-rl

## Run 12
AttributeError: module 'robosuite' has no attribute 'make'

local robosuite does not expose make()

**Fix**
changed suite.make() to robosuite_make() imported from `robosuite.environments.base`.

**Result**
Change successful, proceeded to next run

## Run 13
RuntimeError: Found no NVIDIA driver on your system.

Important:
self.encoder = DrQV2Encoder(obs_shape).to(device)

**Cause**
RL agent is trying to moce itself onto CUDA/GPU, WSL setup does not have an NVIDIA GPU driver available

**Fix**
add device=cpu to command

**Result**
SUCCESS, RL is actively training. This means Robosuite + DrQ-v2 training pipeline works on this laptop in WSL using CPU

However, CPU RL training is too heavy for active computer, **system crashed at ~8000 frames**. Solution is needed for processing unit.

## Run 14 - Decrease to 6000 frames

**Result**
Decreasing to 6000 frames allowed an output to be achieved, an increasing rate is seen in the rewards outputs after 4000 frames



# Second Test (works with pls=true ?)

## Run 1

**Command** 
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

**Error**
MUJOCO_GL=osmesa was being ignored and the run kept using EGL.

**Cause**
planseqlearn/train.py was hardcoding the rendering backend:
os.environ["MUJOCO_GL"] = "egl"

**Fix**
Changed line 23 in planseqlearn/train.py to:
os.environ.setdefault("MUJOCO_GL", "egl")

**Result**
MUJOCO_GL=osmesa was respected on rerun. The clean psl=True smoke test used OSMesa, generated the expected text plan ('red cube', 'grasp'), reached evaluation, and avoided the previous EGL cleanup error.

