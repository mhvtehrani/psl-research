## Debug Run (2. Initial Baseline Command, without PSL (Isolate RL + MuJuCo + Robosuite))

**System Crashed at ~8000 frames**

## Debug Run (2) (3. Tiny Smoke Test (to test if RL runs))

## Seed 2 Control Run — robosuite_Lift, psl=False

Ran to approximately 6000 frames on CPU. Evaluation returns stayed around 0.37–0.40 before training updates began. After replay buffer warmup, training started around frame 4025 and episode returns increased, reaching values above 3.0 and up to about 4.17. Success rate was not shown as positive in the evaluation logs, but reward improvement confirms the base RL loop is functioning. Run ended with an OpenGL/EGL cleanup error related to missing libGLU.so.0, not a core training failure.

**FPS consistently around ~1.3, as a result of CPU running**