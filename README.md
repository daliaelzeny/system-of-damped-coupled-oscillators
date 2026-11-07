# system-of-damped-coupled-oscillators
Analytical and numerical (Euler, RK4) study of two masses coupled by three springs with damping, including normal-mode solutions, energy decay, and Python simulations. Math 202: ODEs project.
 
## Overview
 
This project studies two masses connected by three springs, with linear (velocity-proportional) damping. The motion of each mass depends on the other through the central coupling spring, which gives a system of coupled second-order ODEs. We:
 
1. Derive the equations of motion with Newton's second law.
2. Decouple them with normal coordinates and solve analytically (constant-coefficients method).
3. Check the approach numerically with the Euler method.
4. Simulate the full system in Python with a fourth-order Runge–Kutta (RK4) solver.
5. Discuss how masses, spring constants and damping affect the dynamics.

## The Model
 
For equal masses `M`, damping `b`, wall springs `κ` and coupling spring `κ₁₂`:
 
```
M x1'' + b x1' + (κ + κ12) x1 − κ12 x2 = 0
M x2'' + b x2' + (κ + κ12) x2 − κ12 x1 = 0
```
 
**Assumptions:** 1D horizontal motion, ideal massless Hooke springs, linear damping, point masses, no external forces, constant parameters.
 
## Analytical Solution
 
Define the normal coordinates `u = x1 + x2` and `v = x1 − x2`. The system decouples into two damped harmonic oscillators:
 
```
M u'' + b u' + κ u = 0               (symmetric mode)
M v'' + b v' + (κ + 2κ12) v = 0      (antisymmetric mode)
```
 
In the underdamped case, with `γ = b / 2M`:
 
```
u(t) = e^(−γt) [A cos(Ω1 t) + B sin(Ω1 t)],   Ω1 = sqrt(κ/M − γ²)
v(t) = e^(−γt) [C cos(Ω2 t) + D sin(Ω2 t)],   Ω2 = sqrt((κ + 2κ12)/M − γ²)
```
 
and `x1 = (u + v)/2`, `x2 = (u − v)/2`.
 
The motion is a superposition of two damped normal modes: a **symmetric mode** (`x1 = x2`, frequency Ω1) and an **antisymmetric mode** (`x1 = −x2`, frequency Ω2). Both decay as `e^(−γt)`.
 
## Energy
 
In normal coordinates:
 
```
T = (M/4)(u'² + v'²)
V = (κ/4) u² + ((κ + 2κ12)/4) v²
E = T + V
```
 
Damping removes energy, so the total energy decays roughly as `e^(−2γt)`.
 
## Numerical Methods
 
### Euler method
Used on the decoupled mode equations (step size `h = 0.1`) and compared against the analytical solutions, with absolute error tabulated. Euler is only first-order accurate, so the error grows quickly for oscillatory modes.
 
### RK4 simulation (Python)
The full coupled system is rewritten as first-order ODEs with state vector `Y = [x1, v1, x2, v2]` and integrated with RK4.
 The code computes kinetic and potential energy for each mass and spring, stores everything in a pandas DataFrame, and plots displacement and total energy.
 
**Libraries:** `numpy`, `pandas`, `matplotlib`
 
## Results
 
- Both masses oscillate with amplitudes that shrink over time (Figure 1 in the report).
- Energy is exchanged between the masses through the coupling spring.
- Total mechanical energy decays monotonically toward zero (Figure 2), and the system settles at equilibrium.
- 
## Parameter Effects
 
| Parameter | Effect |
|-----------|--------|
| **Masses** (m1, m2) | Larger mass gives lower natural frequencies and longer periods. Equal masses give symmetric, balanced energy exchange. |
| **Springs** (k1, k2, k3) | Stiffer springs give higher frequencies. The central spring k2 sets the coupling strength; k1 and k3 mainly set each mass's own behavior. |
| **Damping** (c1, c2) | More damping gives faster decay and can move the system from underdamped to critically damped or overdamped. |
 
## Repository Structure
 
```
.
├── README.md
├── System_of_Damped_Oscillators.pdf   # full project report
├── src/
│   └── coupled_oscillators.py         # RK4 simulation
└── figures/
    ├── coupled_oscillator_rk4.png
    └── energy_decay.png
```
## References
 
1. J. Polking, D. Arnold, and A. Boggess, *Differential Equations with Boundary Value Problems*, 2nd ed., Pearson, 2006.
2. J. B. Marion and S. T. Thornton, *Classical Dynamics of Particles and Systems*, 5th ed., Brooks Cole, 2003.
