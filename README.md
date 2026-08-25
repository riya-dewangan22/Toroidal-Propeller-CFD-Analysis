# Transient CFD Analysis of a 3-Bladed Toroidal Propeller

## Project Overview

This project investigates the aerodynamic performance of a 3-bladed toroidal propeller using transient Computational Fluid Dynamics (CFD) simulations in **ANSYS Fluent**. The study focuses on evaluating the propeller's thrust, torque, power consumption, surface pressure distribution, and downstream flow-field characteristics under rotating operating conditions.

## Objective

To numerically evaluate the aerodynamic performance of a 3-bladed toroidal propeller through transient CFD simulations, specifically quantifying thrust, torque, power, pressure distribution, and velocity-field evolution.

## Propeller Specifications

| Parameter | Value |
|---|---|
| Propeller Type | Toroidal |
| Number of Blades | 3 |
| Propeller Diameter ($D$) | 100 mm (0.1 m) |
| Rotational Speed ($N$) | 10,000 RPM |
| Axis of Rotation | Y-axis |
| Inlet Velocity ($V_\infty$) | 0.1 m/s |
| Domain Type | External Flow |
| Simulation Type | Transient CFD |
| Turbulence Model | k-ε (Standard / Realizable) |
| Solver | ANSYS Fluent |

## Geometry & Mesh Details

### CAD Geometry
![Toroidal Propeller CAD Model](propeller_cad.png)

* **Design Software:** SolidWorks
* **Configuration:** 3-bladed closed-loop toroidal propeller
* **Features:** Continuous loop geometry designed to mitigate tip vortices by eliminating open blade tips.

---

### Mesh Specifications

The computational grid was generated in **ANSYS Fluent Meshing** using the parameters extracted below:

| Parameter | Setting / Value |
|---|---|
| **Physics Preference** | CFD |
| **Solver Preference** | Fluent |
| **Element Order** | Linear |
| **Global Element Size** | $6.0 \times 10^{-3} \text{ m}$ ($6 \text{ mm}$) |
| **Smoothing** | Medium |
| **Target Skewness** | Default (0.9) |
| **Total Nodes** | 247,344 |
| **Total Elements** | 1,351,455 |

## Computational Methodology

A 3D transient CFD model was developed in ANSYS Fluent to analyze the unsteady aerodynamics around the rotating toroidal propeller. 

The computational domain consists of two sub-domains:
1. **Stationary Outer Domain:** Captures far-field atmospheric conditions and downstream wake propagation.
2. **Rotating Inner Domain:** Encloses the propeller geometry and utilizes a **Rotating/Sliding Mesh (SMM)** approach to model true transient rotation.

The simulations run at a rotational speed of **10,000 RPM** with a low axial freestream inlet velocity of **0.1 m/s**.

---

## CFD Setup

### Solver Configurations

* **Solver Type:** Pressure-Based Solver
* **Time Formulation:** Transient (Unsteady)
* **Space:** 3D Flow
* **Turbulence Model:** k-ε Model
* **Motion Frame:** Sliding Mesh Approach (Transient Rotor Domain)

### Operating & Boundary Conditions

* **Working Fluid:** Air (density $\rho = 1.225 \text{ kg/m}^3$, viscosity $\mu = 1.7894 \times 10^{-5} \text{ kg/m}\cdot\text{s}$)
* **Rotational Speed:** 10,000 RPM (Y-axis)
* **Inlet:** Velocity Inlet ($V = 0.1 \text{ m/s}$)
* **Outlet:** Pressure Outlet ($P_{\text{gauge}} = 0 \text{ Pa}$)
* **Propeller Surface:** Moving Wall (No-slip, relative to adjacent cell zone)
* **Outer Boundaries:** Stationary Wall / Far-Field

---

## Mesh Generation

A locally refined, unstructured mesh is applied to resolve the complex curvature of the toroidal blades and high velocity/pressure gradients, while maintaining a coarser mesh in the far field to save computational cost.

Refinement regions include:
* Propeller blade surface and leading/trailing edges
* Near-wall boundary layers
* Interface between rotating and stationary domains
* Downstream wake development region

> *Note: A Mesh Independence Study (MIS) was performed to ensure force convergence (Thrust and Torque) across varying grid densities.*

---

## Transient Parameters

The time-advancement parameters are selected based on the rotational velocity to ensure numerical stability and proper transient flow capture per revolution:

| Parameter | Value |
|---|---|
| Time Step Size ($\Delta t$) | $1.0 \times 10^{-4} \text{ s}$ |
| Total Time Steps | 1000 |
| Max Iterations per Time Step | 20 |
| Reporting Interval | 1 |

At **10,000 RPM**, one full rotation ($360^\circ$) takes approximately **0.006 seconds**. A time step of $0.0001\text{ s}$ captures approximately $60^\circ$ of rotation per step, tracking transient vortex shedding and flow development over multiple complete revolutions.

---

## Governing Formulas & Performance Metrics

### 1. Angular Velocity ($\omega$)
$$\omega = \frac{2 \pi N}{60}$$

For $N = 10,000 \text{ RPM}$:
$$\omega = \frac{2 \pi (10000)}{60} \approx 1047.20 \text{ rad/s}$$

### 2. Axial Thrust ($T$)
The total aerodynamic thrust force acts along the propeller’s rotational Y-axis:
$$T = F_y$$

### 3. Aerodynamic Torque ($Q$)
The resistance moment acting around the Y-axis:
$$Q = M_y$$

### 4. Shaft Power ($P$)
The required mechanical input power calculated from torque and angular velocity:
$$P = Q \cdot \omega$$

### 5. Non-Dimensional Performance Coefficients

To standardize performance evaluation, non-dimensional parameters are calculated as follows (where $n = \frac{N}{60}$ in rev/s, $\rho$ is air density, and $D$ is diameter):

* **Thrust Coefficient ($C_T$):**
  $$C_T = \frac{T}{\rho n^2 D^4}$$

* **Torque Coefficient ($C_Q$):**
  $$C_Q = \frac{Q}{\rho n^2 D^5}$$

* **Power Coefficient ($C_P$):**
  $$C_P = \frac{P}{\rho n^3 D^5} = 2\pi C_Q$$

---

## Flow-Field & Post-Processing Visualizations

### 1. Blade Surface Velocity Distribution
![Blade Surface Velocity Contour](vel_contour.png)

**Physical Interpretation:**
* **Tip Speed Maxima:** The linear velocity reaches its peak at the outermost radii of the toroidal loops, topping out at approximately **$52.3 \text{ m/s}$** ($5.027 \times 10^1 \text{ m/s}$). This agrees with the rotational velocity equation $v = \omega r$.
* **Gradual Velocity Gradient:** Velocity smoothly increases from near **$0 \text{ m/s}$** at the central hub region outward along the closed loops. The continuous toroidal loop structure eliminates sharp blade tips, which smooths out high-velocity localized spikes typical of traditional open propellers.

---

### 2. Surface Static Pressure Contour
![Surface Static Pressure Contour](pressure_contour.png)

**Physical Interpretation:**
* **Pressure Differential ($ \Delta P $):** The contour demonstrates a distinct pressure variation across the blade surfaces. High static pressure (up to **$+275.1 \text{ Pa}$**, marked in red/orange) develops on the driving face of the blades, pushing air downstream.
* **Suction Zone Generation:** Low relative static pressure drops down to **$-874.5 \text{ Pa}$** (marked in dark blue) on the suction side, particularly near inner curvature joins. 
* **Thrust Mechanism:** The net thrust force ($T = 0.07889 \text{ N}$) is direct integration of this pressure difference ($\Delta P = P_{\text{pressure}} - P_{\text{suction}}$) over the effective surface area of the toroidal blades.

---

### 3. Velocity Streamlines & Wake Structure
![3D Velocity Streamlines](vel_streamline.png)

**Physical Interpretation:**
* **Downstream Acceleration:** Streamlines visualize fluid draw from the far field (blue lines $\approx 0 - 13.2 \text{ m/s}$) accelerating as air approaches and passes through the rotor plane, reaching peak local core velocities around **$52.96 \text{ m/s}$**.
* **Vortex Mitigation:** Standard propellers shed powerful tip vortices from open blade tips into a turbulent helical wake. The toroidal loop geometry redirects vortex structures back into the wake stream, keeping recirculation localized around the blade roots and suppressing broad tip-vortex shedding.

## Final Results

The transient CFD simulation was performed for the 3-bladed toroidal propeller at 10,000 RPM and an inlet velocity of 0.1 m/s. The final aerodynamic performance parameters obtained from ANSYS Fluent are summarized below.

| Parameter | Final Value |
|---|---:|
| Propeller Diameter | 100 mm |
| Number of Blades | 3 |
| Rotational Speed | 10,000 RPM |
| Inlet Velocity | 0.1 m/s |
| Thrust ($T$) | 0.07889 N |
| Torque ($Q$) | 0.02571 N·m |
| Shaft Power ($P$) | 26.92 W |

---

### Shaft Power Calculation

The shaft power is calculated from the aerodynamic torque and angular velocity:

$$P = Q \cdot \omega$$

where:
* $P$ = Shaft Power (W)
* $Q$ = Propeller Torque (N·m)
* $\omega$ = Angular Velocity (rad/s)

The angular velocity is calculated as:

$$\omega = \frac{2\pi N}{60}$$

For $N = 10,000 \text{ RPM}$:

$$\omega = \frac{2\pi (10000)}{60} \approx 1047.20 \text{ rad/s}$$

Hence:

$$P = 0.02571 \text{ N}\cdot\text{m} \times 1047.20 \text{ rad/s}$$

$$\mathbf{P \approx 26.92 \text{ W}}$$

The results indicate the aerodynamic loading and power requirement of the toroidal propeller under the specified transient operating condition.

## Key Takeaways

Through this transient analysis, the following phenomena were analyzed:
* Quantitative force metrics (Thrust $T$, Torque $Q$, and Shaft Power $P$).
* Reduction of tip-vortex structure compared to standard open-ended blades.
* Pressure gradients across the enclosed toroidal loop during continuous rotation.
* Unsteady convergence characteristics over multiple mechanical rotations.

---

## Software Used

* **CAD Modeling:** SolidWorks
* **Meshing:** ANSYS Meshing
* **CFD Solver & Post-Processing:** ANSYS Fluent

---

## Author

**Riya Dewangan**  
*Undergraduate Researcher | CFD & Aerodynamics*
