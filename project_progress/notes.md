# Debug Test 1 (2. Initial Baseline Command, without PSL (Isolate RL + MuJuCo + Robosuite))

**System Crashed at ~8000 frames**

# Debug Test 2 (3. Tiny Smoke Test (to test if RL runs))

## Seed 2 Control Run — robosuite_Lift, psl=False

Ran to approximately 6000 frames on CPU. Evaluation returns stayed around 0.37–0.40 before training updates began. After replay buffer warmup, training started around frame 4025 and episode returns increased, reaching values above 3.0 and up to about 4.17. Success rate was not shown as positive in the evaluation logs, but reward improvement confirms the base RL loop is functioning. Run ended with an OpenGL/EGL cleanup error related to missing libGLU.so.0, not a core training failure.

**FPS consistently around ~1.3, as a result of CPU running**

# Debug Test 3 (4. Tiny Test (OSMesa, psl=true, seed 2))

## frames=1000
Ran a tiny 1000-frame robosuite_Lift smoke test. Successfully used OSMesa (elimnating the previous library failure) and generated the PSL text plan ('red cube', 'grasp'), completing the first evaluation cycle. This confirms the clean copied repo can execute PSL locally, the run is still only a smoke test and does not prove meaningful RL learning.

## frames=6000, eval=1000
6000 frame psl=true run completed the seed/exploration phase. It completed evaluation checkpoints at 1000, 2000, 3000, and 4000 frames and reached the first training update at frame 4025. The crash occurred after training began (at 4000 frames), likely around the transition from seed collection to gradient updates.

For future runs, will capture everything into a file so we can analyze the crash. "tee *file_name*.log"

## frames=2000, num_seed_frames=1000, eval=500
From previous run, we know training starts at 4000 frames by default. We set that line to 1000 frames to confirm training can start succesfully, which it did; at 1500 frames, train calls (output) = 500.

The current setup can run PSL, collect data, evaluate, and perform gradient updates locally. Results are still not meaningful due to the extremely small frame count and CPU-limited setup.