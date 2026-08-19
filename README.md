# Double Pendulum – Chaos Analysis

**Author:** Fadi Ayoub
**Project:** Numerical simulation and physical experimentation investigating when a double pendulum's motion transitions from stable to chaotic behavior, as a function of its initial angles.

## Overview

A double pendulum — two rods and masses connected in series — is a classic nonlinear, chaotic system: tiny changes in initial angle can produce completely different long-term motion. This project asks: **at what initial angles does that chaotic behavior actually kick in?**

The equations of motion were derived from scratch using the Euler–Lagrange method (full derivation in the write-up), solved numerically with the Euler method, and checked against a real physical double pendulum tracked on video.

**Main finding:** θ₁ (the upper pendulum's angle) is the dominant factor. Chaos appears almost every time θ₁ ≥ 90°, regardless of θ₂, while θ₂ alone rarely triggers it. Symmetric angles (θ₁ ≈ θ₂) tend to stay more stable than asymmetric ones with the same sum.

## Repository structure

```text
double-pendulum-chaos/
├── README.md
├── LICENSE
├── requirements.txt
├── code/
│   ├── simulation_sweep.py       # Euler-method simulation across 30 angle pairs, with
│   │                              # perturbation-based chaos detection (delta = 0.1°)
│   ├── experiment_comparison.py  # Digitized video-tracking data (Tracker software) from
│   │                              # the physical double pendulum, compared across two
│   │                              # camera angles (A/B), for 9 tested angle combinations
│   └── animation.py              # Matplotlib animation comparing 5 simulated pendulums
│                                  # with slightly perturbed initial angles (built for
│                                  # Colab; see note in that file to save locally)
├── results/
│   ├── simulation_angle_sweep.png        # all 30 simulated angle pairs, Sim A vs Sim B
│   └── experiment_theta1_*_theta2_*.png  # 9 physical-experiment trajectory plots
└── docs/
    ├── double_pendulum_english.docx      # full write-up: derivation, methodology, results
    └── מטוטלת_כפולה_הערות.docx            # original Hebrew notes
```

## Running it

```bash
pip install -r requirements.txt
python code/simulation_sweep.py         # regenerates results/simulation_angle_sweep.png
python code/experiment_comparison.py    # regenerates the 9 experiment comparison plots
```

Both scripts were verified to run standalone, top to bottom, with no Colab dependencies.

**Note on `experiment_comparison.py`:** the version originally exported from Colab was missing a variable-unpacking step before its first plot — it only worked in Colab because those variables happened to be left over in memory from a later cell that had been run out of order during interactive development. That's fixed here (see the comment marked `NOTE` near the top of the first block) so the script now runs correctly as a linear script.

## Method summary

- **Simulation:** for each of 30 initial-angle pairs, two nearly identical runs (initial angles differing by 0.1°) are simulated with the Euler method. If the two trajectories' tip positions diverge by more than 0.1 m, that's logged as the onset of chaos.
- **Physical experiment:** a real double pendulum (L₁ = 24 cm, L₂ = 20 cm) was built, released from 9 different angle combinations, and filmed from two angles. Motion was digitized with Tracker software; the two camera views (A/B) are plotted together as a consistency check.
- Full derivation of the equations of motion, the complete results discussion, and conclusions are in `docs/double_pendulum_english.docx`.

## Key results

| Setup | Outcome |
|---|---|
| θ₁ ≤ 75°, any θ₂ | Stable |
| θ₁ ≥ 90° | Chaotic almost always, even for small θ₂ |
| θ₁ = θ₂ (symmetric) | More stable than asymmetric pairs with the same sum |
| θ₁ + θ₂ = constant | Not a reliable predictor by itself — e.g. (60°,90°) stable, (30°,120°) chaotic |

See `docs/double_pendulum_english.docx` for the full results and discussion, including the physical-experiment findings, which matched the simulation.

## Sources

- [Wolfram ScienceWorld: Double Pendulum](https://scienceworld.wolfram.com/physics/DoublePendulum.html)
- [MyPhysicsLab: Double Pendulum](https://www.myphysicslab.com/pendulum/double-pendulum-en.html)
