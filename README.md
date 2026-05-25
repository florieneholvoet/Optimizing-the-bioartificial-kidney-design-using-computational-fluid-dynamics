# Computational Modeling of Protein-Bound Uremic Toxin Transport in Bioartificial Kidney Hollow-Fiber Systems

## Project Overview

This project investigates the transport and clearance of the protein-bound uremic toxin (PBUT) indoxyl sulfate (IS) in bioartificial kidney hollow-fiber systems using computational fluid dynamics (CFD) simulations in COMSOL Multiphysics.

The work compares inside-out and outside-in hollow-fiber configurations under static, cocurrent, and countercurrent dialysate conditions. Active cellular uptake was incorporated using Michaelis–Menten kinetics to represent organic anion transporter (OAT1)-mediated transport.

The simulations evaluate:
- geometric effects,
- blood-volume scaling,
- dialysate flow conditions,
- multifiber interactions,
- and the contribution of active transport mechanisms.

Post-processing and transport analysis were performed in Python.

---

# Background

Protein-bound uremic toxins (PBUTs), such as indoxyl sulfate (IS), are poorly removed by conventional dialysis therapies because only the free toxin fraction can diffuse across the dialysis membrane. In the native kidney, PBUT removal occurs primarily through active tubular secretion mediated by organic anion transporters (OATs) expressed in proximal tubule epithelial cells (PTECs).


<p align="center">
  <img src="figures/bioartificial_kidney%20(3).png" width="700">
</p>

<p align="center">
  <em>
  Schematic representation of a bioartificial kidney integrated with a conventional dialysis filter. 
  After conventional dialysis removes small and medium-sized solutes, blood enters the bioartificial kidney, 
  where OAT1-expressing proximal tubule epithelial cells actively transport protein-bound uremic toxins 
  across hollow-fiber membranes before the blood is returned to the patient.
  Adapted from Ramada et al. (2023) [1].
  </em>
</p>


Bioartificial kidney systems aim to restore this missing transport functionality by integrating living renal cells with hollow-fiber membrane technology. However, optimizing such systems requires understanding the coupled effects of:
- geometry,
- flow conditions,
- membrane transport,
- and active cellular uptake.

This work uses CFD-based transport modeling to systematically investigate how hollow-fiber configuration and flow conditions influence PBUT transport and clearance performance.

---

# Research Questions

This project addresses the following research questions:

1. How do inside-out and outside-in hollow-fiber configurations influence indoxyl sulfate transport and clearance?

2. What is the effect of cocurrent and countercurrent dialysate flow on clearance performance?

3. How does blood-volume scaling affect the comparison between inside-out and outside-in configurations?

4. What is the contribution of active Michaelis–Menten transport relative to passive diffusion?

5. How do multifiber configurations influence concentration gradients and overall transport behavior?

---

# Model Overview

## Geometry

Three hollow-fiber configurations were investigated:

| Configuration | Description |
|---|---|
| Inside-out | Blood flows through the lumen while dialysate occupies the outer shell |
| Outside-in | Blood occupies the outer shell while dialysate flows through the lumen |
| Adjusted outside-in | Volume-matched variant of the outside-in geometry |

The computational domain consisted of four compartments:
- blood,
- membrane,
- epithelial cell layer,
- dialysate.

Single-fiber simulations were modeled using a 2D axisymmetric geometry to reduce computational cost while preserving the dominant radial transport mechanisms.

Multifiber simulations were later extended to 3D configurations containing five repeated fiber units embedded within a shared compartment.

---

## Flow Configurations

Three dialysate conditions were investigated:
- stagnant dialysate,
- cocurrent flow,
- countercurrent flow.

Countercurrent flow was introduced to maintain concentration gradients along the fiber length and enhance solute transport.
### Inside-out countercurrent configuration
<p align="center">
  <img src="figures/inside_out_countercurrent (1).png" width="750">
</p>

<p align="center">
  <em>
  Figure: Inside-out countercurrent flow configuration, with blood flow in the lumen and dialysate flow imposed in the opposite direction in the outer compartment.
  </em>
</p>

### Outside-in countercurrent configuration
<p align="center">
  <img src="figures/outside_in_countercurrent (1).png" width="750">
</p>

<p align="center">
  <em>
  Figure: Outside-in countercurrent flow configuration, with blood flow in the outer domain and dialysate flow imposed in the opposite direction through the lumen.
  </em>
</p>

---

## Modelling Scenarios

A stepwise modeling strategy was adopted, progressing from single-fiber axisymmetric simulations to multifiber 3D configurations.

| Scenario | Purpose |
|---|---|
| Inside-out single fiber | Reference configuration |
| Outside-in single fiber | Geometric inversion |
| Adjusted outside-in | Volume-matched comparison |
| Cell-layer split model | Separate basolateral/apical transport behavior |
| Multifiber models | Fiber interaction effects |

Each configuration was evaluated under:
- static dialysate conditions,
- cocurrent flow,
- countercurrent flow.

---

# Mathematical Model

## General Transport Equation

Transport of indoxyl sulfate (IS) was modeled using the convection–diffusion–reaction equation:

dC/dt + u·∇C = D∇²C + R(c,Vmax,Km)

where:
- c = IS concentration
- D = diffusion coefficient
- u = velocity field
- R(c,Vmax,Km) = cellular uptake term

The simulations were implemented using the *Transport of Diluted Species* interface in COMSOL Multiphysics 6.3.

---

## Domain-Specific Transport Mechanisms

| Domain | Convection | Diffusion | Reaction |
|---|---|---|---|
| Blood | Yes | Yes | No |
| Membrane | No | Yes | No |
| Cell layer | No | Yes | Michaelis–Menten uptake |
| Dialysate | Optional | Yes | No |

Transport in the blood and dialysate compartments included both convection and diffusion, while the membrane and cell layer were modeled as diffusion-dominated regions.

---

## Flow Model

Fluid motion in the blood and dialysate compartments was modeled using the incompressible Navier–Stokes equations under laminar conditions.

Countercurrent and cocurrent dialysate configurations were investigated. Reynolds number analysis confirmed laminar flow behavior for all simulated cases.

Fully developed parabolic inlet velocity profiles were imposed at the blood and dialysate inlets.

---

## Active Michaelis–Menten Transport

Active uptake within the epithelial cell layer was modeled using Michaelis–Menten kinetics:

R_uptake = Vmax · c / (Km + c)

- Vmax = 10^6 μmol·L^-1·min^-1
- Km = 20 μM

Additional simulations were performed with varying \(V_{\max}\) values to evaluate the influence of active transport capacity on overall clearance.

A split cell-layer model was also investigated to distinguish between basolateral and apical transport behavior.

---

# Numerical Implementation

Simulations were performed in COMSOL Multiphysics 6.3 using time-dependent studies.

The nonlinear systems were solved using:
- fully coupled formulation,
- PARDISO direct solver,
- tolerance-controlled convergence.

For multifiber simulations, a two-step strategy was used:
1. stationary solution of the laminar flow field,
2. time-dependent solution of species transport.

Mesh independence was verified through mesh sensitivity analysis for all configurations.

Post-processing and transport analysis were performed in Python.

---

# Quantification of Transport and Clearance

## Clearance Definition

Clearance was quantified using the total molar transport rate across the blood–membrane interface:

ṅ_M(t) = ∫ J_n dΓ

A time-averaged clearance metric was defined as:

CL_avg(t) = (1 / (t·A·Cin)) · ∫ ṅ_M(τ) dτ

- A = membrane surface area
- Cin = inlet IS concentration

All clearance values were normalized by membrane surface area to enable comparison between geometries.

Additional transport metrics were evaluated to identify transport bottlenecks between:
- blood,
- membrane,
- cell layer,
- dialysate.

---

## Contribution of Active Transport

To quantify the contribution of active transport, simulations with and without Michaelis–Menten uptake were compared.

The passive transport contribution ratio was defined as:

R_Φ(t) = |Φ_(Vmax=0)| / |Φ_full|

- RΦ = 1 → active transport has negligible effect
- RΦ < 1 → active transport enhances overall transport

This analysis was used to distinguish passive diffusive transport from transporter-mediated uptake.

---

# Main Findings

Key observations from the simulations include:

- Countercurrent dialysate flow substantially enhances indoxyl sulfate clearance.
- Inside-out multifiber configurations exhibit near-linear scaling behavior.
- Outside-in configurations are more sensitive to concentration-gradient limitations.
- Active Michaelis–Menten transport only dominates overall transport at sufficiently high uptake capacities.
- Membrane-area normalization is essential for fair comparison between geometries.
- Multifiber interactions significantly influence local concentration gradients and transport efficiency.

---

# Repository Structure

```text
├── COMSOL_models/
├── Python_postprocessing/
├── Figures/
├── Mesh_sensitivity/
├── Results/
└── README.md


## Software

- COMSOL Multiphysics 6.3
- Python
- LaTeX

---


## References

---

## References

1. Ramada, M., et al. *Portable, wearable and implantable artificial kidney systems: needs, opportunities and challenges.*  
   **Nature Reviews Nephrology** 19(8), 481–490 (2023).  
   https://doi.org/10.1038/s41581-023-00726-9

