# double-pendulum-chaos
An overview of the initial angle's effect on a double pendulum's chaos
# Double Pendulum – Chaos Analysis

**Author:** Fadi Ayoub  
**Project:** Research project analyzing chaotic behavior in a double pendulum using numerical simulation and physical experimentation.

---

##  Overview

This project investigates when a double pendulum transitions from **stable motion** to **chaotic behavior**, by analyzing:

- The **nonlinear differential equations** governing the system
- A **numerical simulation** (Euler method)
- A **physical experiment** using video tracking
- Systematic comparison between **symmetric and asymmetric initial angles**

**Main observation:**

- θ₁ (upper pendulum angle) is the dominant factor that triggers chaos.  
- Chaos occurs almost always when θ₁ ≥ 90°, even if θ₂ is small.

---

##  Repository Structure

double-pendulum-chaos/
│
├── code/
│ ├── double_pendulum_simulation.py
│ ├── utils.py
│ └── examples/
│ ├── sim_theta1_60_theta2_120.png
│ ├── sim_theta1_75_theta2_75.png
│ └── ...
│
├── experiment/
│ ├── tracker_data.csv
│ ├── experiment_examples/
│ │ ├── exp_theta1_90_theta2_0.png
│ │ ├── exp_theta1_75_theta2_15.png
│ │ └── ...
│ └── description.md
│
├── report/
│ ├── Double_Pendulum_Chaos_Fadi_Ayoub.pdf
│ └── derivations_appendix.pdf
│
└── README.md

yaml
Copy code

---

##  Background

A double pendulum consists of two masses connected by rigid rods:

- **M₁, M₂** — masses  
- **L₁, L₂** — rod lengths  
- **θ₁, θ₂** — angles from the vertical  

The system is **non-linear and often chaotic**:

- Tiny changes in initial angles → completely different long-term behavior  
- No closed-form analytical solution  
- Numerical simulations diverge exponentially due to sensitivity

---

## 🧮 Mathematical Model

The dynamics were derived using the **Euler–Lagrange equation**:

dtd​(∂θ˙i​∂L​)−∂θi​∂L​=0      i=1,2

where:


L = T - V


After full derivation, the equations of motion are:

θ₁''= (-g*(2M1+M2)*sin(θ1)
 - M2*g*sin(θ1-2θ2)
 - 2*sin(θ1-θ2)*M2*(θ2_dot^2*L2 + θ1_dot^2*L1*cos(θ1-θ2)))
 / (L1*(2M1+M2 - M2*cos(2θ1 - 2θ2)))


θ₂'' =
(2*sin(θ1-θ2)*(θ1_dot^2*L1*(M1+M2)
 + g*(M1+M2)*cos(θ1)
 + θ2_dot^2*L2*M2*cos(θ1-θ2)))
 / (L2*(2M1+M2 - M2*cos(2θ1 - 2θ2)))

---

##  Numerical Simulation

**Euler method** is used:

```python
v += a * dt
theta += v * dt
Simulation details:

Runs two pendulums with θ + δθ (δ = 0.1°)

Compares their trajectories

Detects divergence > 0.1 m → chaos detection

🔍 Simulation Results
Key Findings:

Chaos begins when θ₁ ≳ 90°

Even if θ₂ small → chaos

θ₁ controls energy input

θ₂ alone does not cause chaos

θ₁ = 30° or 60°, even large θ₂ stays stable

θ₁ = 90° → chaos almost guaranteed

Symmetry reduces chaos

θ₁ = 75°, θ₂ = 75° → stable

θ₁ = 75°, θ₂ = 15° → chaotic

Sum of angles doesn’t predict chaos

Example: (60°, 90°) — stable

(30°, 120°) — chaotic

📹 Physical Experiment
Built a real double pendulum: L₁ = 24 cm, L₂ = 20 cm

Angles measured using Pythagorean-based geometry

Motion recorded and analyzed using Tracker software

Only 5 seconds analyzed (noise + air resistance + mass distribution)

Findings match simulation:

θ₁ = 90° → chaos always

θ₁ = 75°, θ₂ = 75° → stable

θ₁ = 75°, θ₂ = 15° → chaotic

Even with real-world imperfections, patterns repeat.

Conclusions:
θ₁ is the dominant variable: large θ₁ injects enough energy to destabilize

Symmetry stabilizes: balanced initial conditions reduce torque differences

Chaos requires specific combinations, not just big angles

Running the Simulation
Requirements:

numpy

matplotlib

Run:

bash
Copy code
python code/double_pendulum_simulation.py
The script runs angle sweeps, produces chaos/stability plots, and exports figure results.
