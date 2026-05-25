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

$$
\frac{\partial c}{\partial t} + \mathbf{u}\cdot\nabla c
= D\nabla^2 c + R(c,V_{\max},K_m)
$$

where:
- \(c\) = indoxyl sulfate (IS) concentration
- \(D\) = diffusion coefficient
- \(\mathbf{u}\) = velocity field
- \(R(c,V_{\max},K_m)\) = cellular uptake term

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

$$
R_{\mathrm{uptake}} = V_{\max}\frac{c}{K_m+c}
$$

with:
- \(V_{\max}=10^6\ \mu\mathrm{mol\,L^{-1}\,min^{-1}}\)
- \(K_m=20\ \mu\mathrm{M}\)

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

$$
\dot{n}_M(t)=\int_{\Gamma_M} J_n\, d\Gamma
$$

A time-averaged clearance metric was defined as:

$$
\overline{CL}(t)=
\frac{1}{tAC_{in}}
\int_0^t \dot{n}_M(\tau)\,d\tau
$$

where:
- \(A\) = membrane surface area
- \(C_{in}\) = inlet indoxyl sulfate concentration

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

$$
R_{\Phi}(t)=
\frac{
|\Phi_{V_{\max}=0}(t)|
}{
|\Phi_{\mathrm{full}}(t)|
}
$$

where:
- \(R_{\Phi}=1\): active transport has negligible effect
- \(R_{\Phi}<1\): active transport enhances overall transport

This analysis was used to distinguish passive diffusive transport from transporter-mediated uptake.

---

# Main Findings

Key findings from the simulations include:

- Under static dialysate conditions, the inside-out configuration maintained the transmembrane concentration gradient substantially longer than the outside-in configuration due to the larger surrounding dialysate volume, resulting in higher sustained clearance.

- Countercurrent dialysate flow strongly enhanced transport performance by suppressing dialysate saturation and maintaining concentration gradients along the fiber length. However, only minor differences were observed between cocurrent and countercurrent operation because the investigated fiber lengths were relatively short.

- Under flow conditions, the original outside-in configuration achieved the highest area-normalized clearance (~9.5 μL/(min·cm²)) due to the strongest local blood-to-membrane concentration gradients.

- The adjusted outside-in configuration exhibited the largest total transport capacity because of its substantially increased membrane and cell-layer surface area, despite weaker local concentration gradients.

- Membrane-area normalization strongly influenced the interpretation of clearance performance. Configurations with larger membrane areas showed lower area-normalized clearance despite higher total solute transport.

- The transport behavior remained predominantly diffusion-driven over most investigated parameter ranges. The inlet indoxyl sulfate concentration had the strongest influence on clearance, followed by the dialysate flow rate, whereas blood flow rate and moderate changes in Vmax produced comparatively limited effects.

- Active Michaelis--Menten transport contributed only minimally at Vmax = 10^6 μmol/(L·min), but became increasingly important once Vmax ≥ 10^8 μmol/(L·min).

- Relative to the corresponding single-fiber configuration, the inside-out multifiber geometry showed an approximately linear increase in total clearance with the number of fibers, whereas the outside-in multifiber geometry exhibited substantially weaker scaling due to reduced blood-to-membrane concentration gradients in the multifiber configuration.

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

