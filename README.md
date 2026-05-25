# Optimizing the bioartificial kidney design using computational-fluid dynamics


## Overview

This project investigates indoxyl sulfate (IS) transport and clearance in a bioartificial kidney (BAK) using computational fluid dynamics (CFD).

The work focuses on:
- Protein-bound uremic toxin (PBUT) transport
- OAT1-mediated active transport
- Hollow-fiber bioartificial kidney geometries
- Coupled convection–diffusion–reaction modeling
- CFD-based transport optimization

The developed framework combines:
- Fluid flow
- Solute diffusion
- Michaelis–Menten uptake kinetics
- Hollow-fiber membrane transport

The objective is to improve understanding of how geometry, flow configuration, and active transport influence PBUT clearance in future bioartificial kidney devices.

---

## Background

Conventional dialysis therapies remain inefficient at removing protein-bound uremic toxins because only the free toxin fraction can cross dialysis membranes.

Bioartificial kidneys aim to restore active tubular secretion by integrating proximal tubule epithelial cells (PTECs) expressing OAT1 transporters into hollow-fiber systems.

<p align="center">
  <img src="figures/bioartificial_kidney%20(3).png" width="700">
</p>

<p align="center">
  <em>
  Schematic representation of a bioartificial kidney integrated with a conventional dialysis filter. 
  After conventional dialysis removes small and medium-sized solutes, blood enters the bioartificial kidney, 
  where OAT1-expressing proximal tubule epithelial cells actively transport protein-bound uremic toxins 
  across hollow-fiber membranes before the blood is returned to the patient.
  </em>
</p>

This project develops a CFD framework to investigate:
- Flow behavior
- Concentration gradients
- Active toxin uptake
- Dialysate washout
- Geometric optimization
---

## Research Questions

### Main Research Question

> Can the incorporation of an OAT1-expressing proximal tubule cell layer in hollow-fiber dialysis geometries improve indoxyl sulfate transport and clearance in a bioartificial kidney, as investigated using computational fluid dynamics?

### Subquestions

- How do inside–out and outside–in hollow-fiber configurations differ?
- What is the effect of stagnant, cocurrent, and countercurrent dialysate conditions?
- How do flow rate and uptake kinetics influence clearance?
- What is the contribution of active uptake relative to passive diffusion?
- How does scaling from single-fiber to multifiber systems affect transport?

---

## Methodology

A two-dimensional axisymmetric CFD model was developed in COMSOL Multiphysics 6.3.

The model includes:
- Blood compartment
- Porous membrane
- Cell monolayer
- Dialysate compartment

Transport mechanisms:
- Convection
- Diffusion
- Michaelis–Menten active uptake

## Governing Transport Equation

```math
\frac{\partial c}{\partial t} + \mathbf{u}\cdot\nabla c = D\nabla^2 c + R(c)
```

## Active Uptake Model

```math
R_{\mathrm{uptake}} = V_{\max}\frac{c}{K_m+c}
```
---

## Geometries

### Inside–Out Configuration

Blood flows through the lumen while dialysate flows externally.

![Inside-Out](figures/configurations_BAK.png)

### Outside–In Configuration

Blood flows through the extracapillary space while dialysate flows through the lumen.

---

## Simulation Cases

The following scenarios were investigated:

- Static dialysate
- Cocurrent flow
- Countercurrent flow
- Split cell-layer kinetics
- Multifiber geometries

Furthermore, a sensitivity analysis was performed to identify the dominant parameters governing indoxyl sulfate transport and clearance performance within the bioartificial kidney system.

---
## Simulation Settings

A time–dependent study was performed for 240 minutes. 
The nonlinear system was solved fully coupled using the PARDISO direct linear solver. Convergence of each time step was controlled using a tolerance-based termination technique. Relative tolerances were physics–controlled.

For the multifiber models, a two-step solution strategy was adopted. First, the laminar flow equations were solved using a stationary study to obtain a stable velocity field. Subsequently, the Transport of Diluted Species equations were solved using a time–dependent study, with the previously computed velocity field imposed. This approach is justified by the fact that the velocity field remains stable and is not significantly influenced by solute transport, allowing a decoupled solution procedure.

---

## Key Results

### Static Dialysate Conditions

- Transport is diffusion dominated
- Dialysate saturation strongly limits clearance
- Geometry significantly affects concentration-gradient persistence

### Countercurrent Flow

- Dialysate refreshment maintains concentration gradients
- Quasi-steady transport behavior develops rapidly
- Countercurrent operation improves sustained transport

### Geometric Effects

- Outside–in geometries achieved the highest area-normalized clearance
- Membrane surface area strongly influences total transport
- Compartment-volume distribution affects gradient collapse

---

## Example Results

### Velocity Profiles

![Velocity](figures/velocity_profiles.png)

### Pressure Profiles

![Pressure](figures/pressure_profiles.png)

### Radial Concentration Profiles

![Concentration](figures/radial_concentration_profiles.png)

---


## Software

- COMSOL Multiphysics 6.3
- Python
- LaTeX

---


## References

Key references include:
- Refoyo et al.

