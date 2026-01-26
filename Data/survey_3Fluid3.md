# Expert evaluation - Intelligent Agent for Industrial Process Optimization Modeling with Large Language Models

## Guidelines for Evaluation

Thank you for contributing to this research. 

This survey evaluates three AI-generated mathematical models (Model A, B, and C) by comparing them against different problem descriptions and ground truth models. 
Please assess each model's objective functions, decision variables, and constraints for correctness, completeness.

+ **Read the Toy Example in Section 1:** Review the toy example, which is a simple problem to illustrate the task for you to evaluate the models.

+ **Read the Problem in Section 2:** Review the problem description, which describes the problem background, objective, etc.

+ **Select the Best Model in Section 3:** This section contains 4 questions, addressing the objective function, decision variable, constraint, and model parts. For each question, select the best formulation (A, B, or C) based on the specified criterion.


## Evaluation Sheet and Answer

**Early questionnaire:**

Demographic properties:
-	What is your level of education? Answer: ___ 
(Options: Undergraduate, Master, Doctoral)
-	What is your job position? Answer: ___ 
(Options: Academic, Industry)

Acquaintance with the field:
- What is your most relevant major? Answer: _______
 (Options: Control Engineering, Computer Science, Chemical Engineering, Data Science, Mathematics, Other: ____)
- How much are you familiar with optimization modeling? Answer: _______
(Options: Not familiar, Somewhat familiar, Familiar, Very familiar)
- How much are you familiar with process control problems? Answer: _______
(Options: Not familiar, Somewhat familiar, Familiar, Very familiar)


**Final questionnaire:**

1. Which objective function is the best?
Best Model: ___ (Options: A,B,C)

2. Which decision variable is the most complete?
Best Model: ___ (Options: A,B,C)

3. Which constraint is the most complete?
Best Model: ___ (Options: A,B,C)

4. Overall, which model is the most convincing?
Best Model: ___ (Options: A,B,C)



<div style="page-break-after: always;"></div>


# Section 1. Toy Example for illustration


Suppose we have a simple temperature control system:  A heater is used to maintain the temperature of a tank at a setpoint of 75°C. The control input is the power supplied to the heater, and the output is the tank temperature. The goal is to keep the temperature close to the setpoint despite disturbances.

**Decision Variable:**  
- $K_p$: Proportional gain of the PID controller

**Objective:**  
Minimize the steady-state error and overshoot in response to a step change in setpoint.

### Candidate Control Models

- **Model A:** Proportional-only controller ($u = K_p \cdot e$)
- **Model B:** Proportional-Integral (PI) controller ($u = K_p \cdot e + K_i \int e\,dt$)
- **Model C:** Proportional-Integral-Derivative (PID) controller ($u = K_p \cdot e + K_i \int e\,dt + K_d \frac{de}{dt}$)

**Question:**  
Which model offers the best balance of fast response and minimal steady-state error in practice?

**Answer:**
Best Model: ___ (Options: A,B,C)

**Sample Answer:**  
Best Model: C (the PID controller typically offers the best balance between fast response and minimal steady-state error, because it combines proportional, integral, and derivative actions.) 



<div style="page-break-after: always;"></div>


# Section 2. Problem to be evaluated



## 2.1. Problem Description




Various rules of thumb exist for standard water filtration rates and cycle time before backwashing. Higher filtration rates may appear to be economically justified, however, when the filter loading is within conventional limits. In this example, we examine the issues involved for constant-rate filtration for a dual-media bed. Dual and mixed-media beds result in increased production of water in a filter for two reasons. First, the larger grains (sya charcoal approximately 1-mm size) as a top layer help reduce cake formation and deposition within the small (150-mm) as a top layer of the bed. Second, the head loss in the region of sigificant filtration is reduced.

With respect to the objective function for a filter, the total annual cost of filtration $f$ is assumed to be the sum of the annualized capital costs $f_c$ and the annual operating costs $f_o$. 


The cross-sectional area can be calculated by dividing the design flow rate by a quantity that is equal to the number of filter runs per day times the net water production per run per cross-sectional area. For a constant filtration rate, the length of the filter run is given by $t_{f} = V_{f}/Q$. The water production per filter run $V_f$ is based on a relation proposed by Letterman (1980) that assumes minimal surface cake formation by the time filtration is stopped because of head loss. The backwash flow rate is calculated from the volume of filtered water used for backwash. We assume the backwash water is not recycled. We next summarize the annual operating costs of the filter because they are equal to the energy costs for pumping.

Build a mathematical model to minimize the total annual cost of filtration (capital and operating) subject to constraints on filter runs and head loss.



## 2.2 Ground Truth Model

**Capital Cost Model:**



$$f_{c} = rbA^{z}$$ 



Where:

\* $r$ = capital recovery factor

\* $b$ = empirical constant

\* $z$ = empirical exponent

\* $A$ = cross-sectional area of the filter 



**Process Constraints:**  

The cross-sectional area is defined as:



$$A = \frac{q}{1440/[(V_{f}/Q)+t_{b}]\cdot(V_{f}-V_{b})}$$ 



Where:

\* $q$ = design flow rate (gal/day)

\* $V_f$ = volume of water filtered per unit area per run

\* $V_b$ = volume of filtered water used for backwash per unit area

\* $Q$ = filtration rate

\* $t_b$ = filter down time for backwash 



The water production per filter run ($V_f$) constraint (Head Loss model):



$$V_{f} = \frac{K_{\rho}\dot{D}}{\beta C_{0}n}\sum_{i=1}^{n}\log\frac{n\Delta H}{k_{i}\dot{D}Q}$$ 



Where $\Delta H$ is the terminal pressure loss, $k_i$ is a function of grain diameter, and other symbols represent bed properties. 



The backwash flow rate ($q_b$):



$$q_{b} = \left(\frac{V_{f}}{V_{f}-V_{b}}-1\right)q$$ 



**Operating Cost Model:**



$$f_{0} = q_{b}\left[1.146\times10^{-3}C_{E}\left(\frac{h}{\eta}\right)\right]$$ 



Where $C_E$ is cost of electricity, $h$ is pumping head, and $\eta$ is efficiency. 



**Total Cost Objective Function:**



$$f = f_c + f_o$$



Substituting values leads to the general form:



$$f(\$/year) = 116\left[\frac{10^{6}q}{1440/[(V_{f}/Q)+t_{b}](V_{f}-V_{b})}\right]^{0.86} + 4.73\times10^{3}\left[\frac{V_{f}}{V_{f}-V_{b}}-1\right]q$$

<div style="page-break-after: always;"></div>


# Section 3. Questions

## Question 1: Objective Function Comparison

### Model A's Objective Function



### Total Annual Cost
$$\min f = f_c + f_o = c_c \cdot A + c_o \cdot \frac{Q_d \cdot t_f}{V_f}$$

**Note**: Minimize the sum of annualized capital costs and annual operating costs

### Model B's Objective Function




$$
f = f_c + f_o = c_c \cdot A + c_o \cdot Q_d \cdot \sum_{i=1}^{r} \Delta H_i
$$

**Note**: Minimize total annual cost of filtration, composed of annualized capital cost and annual operating (pumping) cost.

> **Note on head loss summation**: Since filtration rate $Q$ is constant and head loss increases linearly with filtered volume during a run, the average head loss per run is $\Delta H_{\text{avg}} = \frac{1}{2} \Delta H_{\text{max}}$. Thus, total annual head loss energy cost is proportional to $r \cdot \Delta H_{\text{max}}$. We simplify the objective using this approximation.

Revised objective (simplified):
$$
f = c_c \cdot A + c_o \cdot Q_d \cdot r \cdot \frac{\Delta H_{\text{max}}}{2}
$$

### Model C's Objective Function


$$
\min \quad f(A_f, v_f) = f_c + f_o = (C_c \cdot A_f) + \left( C_{bw} \cdot \frac{N_{days} \cdot T_{day}}{t_f + t_{bw}} \right)
$$
*(Note: This objective function aims to minimize the total annual cost of filtration ($f$). It is the sum of the annualized capital cost ($f_c$), which is directly proportional to the filter area $A_f$, and the total annual operating cost ($f_o$), which is calculated based on the cost per backwash cycle and the total number of cycles per year. The number of cycles depends on the productive filter run length ($t_f$) and the non-productive backwash duration ($t_{bw}$), creating the core trade-off.)*

---

**Question:** Which model has the best objective function?  
Best Model: ___ (Options: Objective Function A, Objective Function B, Objective Function C)

<div style="page-break-after: always;"></div>

## Question 2: Decision Variable Comparison

### Model A's Decision Variable



#### Filtration Rate ($v$)
- **Symbol**: $v$
- **Description**: Constant filtration rate (velocity) through the filter bed

#### Filter Run Time ($t_f$)
- **Symbol**: $t_f$
- **Description**: Duration of filter operation between backwashing cycles

#### Cross-sectional Area ($A$)
- **Symbol**: $A$
- **Description**: Cross-sectional area of the filter bed perpendicular to flow direction

### Model B's Decision Variable



#### Filter Cross-Sectional Area ($A$)
- **Symbol**: $A$
- **Description**: The cross-sectional area of the filter bed (in m²). This is the primary decision variable that determines the size of the filter. A larger area reduces filtration rate and extends filter run time, lowering operating costs but increasing capital cost. The goal is to find the optimal area that minimizes total annual cost.

#### Filtration Rate ($Q$)
- **Symbol**: $Q$
- **Description**: The constant filtration rate (in m³/h·m²). This represents the volumetric flow rate per unit area of the filter bed. It directly affects the length of the filter run and the frequency of backwashing. Higher $Q$ reduces capital cost (smaller area needed) but increases operating cost due to shorter runs and higher head loss.

### Model C's Decision Variable


| Symbol | Meaning | Unit | Corresponding Data Point |
|--------|---------|------|---------------------------|
| **Decision Variables** | | | |
| $A_f$ | Filter cross-sectional area | m² | Design Variable |
| $v_f$ | Filtration rate (superficial velocity) | m/h | Operational Variable |
| **Parameters** | | | |
| $Q_d$ | Average daily design flow rate required from the plant | m³/day | System Requirement |
| $C_c$ | Annualized capital cost per unit of filter area | €/m²·year | Economic Data |
| $C_{bw}$ | Total cost per backwash cycle (includes energy and lost water) | €/cycle | Economic/Operational Data |
| $V_{bw}$ | Volume of filtered water used per backwash cycle | m³ | Operational Data |
| $t_{bw}$ | Duration of one backwash cycle (non-productive time) | h | Operational Data |
| $L_{min}$ | Minimum conventional filter loading rate | m/h | Operational Constraint |
| $L_{max}$ | Maximum conventional filter loading rate | m/h | Operational Constraint |
| $P_{LM}$ | Set of parameters for the Letterman model | various | Empirical Data |
| $T_{day}$ | Duration of a day | h | Constant (24) |
| $N_{days}$ | Number of operating days per year | days/year | Constant (365) |

---

**Question:** Which model has the most complete decision variables?  
Best Model: ___ (Options: Decision Variable A, Decision Variable B, Decision Variable C)

<div style="page-break-after: always;"></div>

## Question 3: Constraint Comparison

### Model A's Constraints



#### Equality Constraints
1. $$A = \frac{Q_d}{n_r \cdot \frac{V_f}{A}}$$
   - **Note**: Cross-sectional area calculation based on design flow rate and water production per run

2. $$t_f = \frac{V_f}{Q}$$
   - **Note**: Filter run time determined by filtered water volume and flow rate relationship

#### Inequality Constraints
1. $$t_f \geq t_{f,min}$$
   - **Note**: Filter run time must meet minimum operational requirements

2. $$v \leq v_{max}$$
   - **Note**: Filtration rate must not exceed maximum allowable velocity

3. $$H(v, t_f) \leq H_{max}$$
   - **Note**: Head loss during filtration must remain below maximum permissible level

#### Boundary Conditions
- $v > 0$
- $t_f > 0$
- $A > 0$

### Model B's Constraints



#### Equality Constraints
1. $$A = \frac{Q_d}{Q \cdot r}$$
   - **Note**: The cross-sectional area must be sufficient to handle the design flow rate $Q_d$ at filtration rate $Q$, given $r$ runs per day. Derived from mass balance: total flow = filtration rate × area × runs per day.

2. $$r = \frac{24}{t_f + t_b}$$
   - **Note**: The number of daily filter cycles equals 24 hours divided by the total cycle time (filtration + backwash).

3. $$t_f = \frac{V_f}{Q \cdot A}$$
   - **Note**: Filtration run time equals water produced per run divided by the volumetric flow rate through the filter (i.e., $Q \cdot A$).

4. $$V_f = k \cdot \Delta H_{\text{max}}$$
   - **Note**: Letterman’s model: water production per run is proportional to maximum allowable head loss, with proportionality constant $k$.

#### Inequality Constraints
1. $$Q_{\min} \leq Q \leq Q_{\max}$$
   - **Note**: Filtration rate must remain within conventional engineering limits to avoid media washout ($Q_{\max}$) or inadequate particle removal ($Q_{\min}$).

2. $$A_{\min} \leq A \leq A_{\max}$$
   - **Note**: Physical constraints on filter size due to plant layout, construction feasibility, or equipment availability.

3. $$r \leq r_{\max}$$
   - **Note**: Maximum number of backwash cycles per day is limited by operational constraints (e.g., labor, backwash water supply, downtime).

#### Boundary Conditions
- $$A > 0, \quad Q > 0$$
  - **Note**: Physical non-negativity and positivity constraints on area and filtration rate.

### Model C's Constraints


#### 
 Physical Constraints
$$
V_f = g(v_f, P_{LM})
$$
*(Note: This equation defines the total volume of water produced during a single filter run, $V_f$. The function $g(v_f, P_{LM})$ represents the empirical model by Letterman (1980), which calculates the production volume until the maximum allowable head loss is reached. This volume is a function of the filtration rate $v_f$ and a set of empirical parameters $P_{LM}$ related to water quality and filter media.)*

$$
t_f = \frac{V_f}{v_f \cdot A_f}
$$
*(Note: This constraint defines the productive filter run length, $t_f$. It is the total volume produced per run ($V_f$) divided by the gross volumetric flow rate ($Q = v_f \cdot A_f$). This equation directly links the design variable ($A_f$) and the operational variable ($v_f$) to the cycle timing.)*

#### 
 Operational Constraints
$$
\frac{T_{day}}{t_f + t_{bw}} \cdot (V_f - V_{bw}) \geq Q_d
$$
*(Note: This is the primary performance constraint, ensuring that the net daily water production meets the design requirement $Q_d$. The net production is calculated by multiplying the number of cycles per day by the net volume produced per cycle ($V_f - V_{bw}$), accounting for water consumed during backwashing.)*

$$
L_{min} \leq v_f \leq L_{max}
$$
*(Note: This constraint ensures that the chosen filtration rate $v_f$ remains within the "conventional limits" for filter loading. This is an operational heuristic to prevent issues like media fouling, bed compaction, or poor effluent quality that are not explicitly captured in the cost model.)*

#### 
 Logical Constraints
$$
A_f > 0
$$
*(Note: This constraint ensures the physical filter area is a positive value.)*

$$
v_f > 0
$$
*(Note: This constraint ensures the filtration rate is a positive value.)*

---

**Question:** Which model has the best constraints?  
Best Model: ___ (Options: Constraint A, Constraint B, Constraint C)

<div style="page-break-after: always;"></div>

## Question 4: Overall Evaluation

**Question:** Overall, which model is the most convincing?  
Best Model: ___ (Options: Model A, Model B, Model C)

### General Comments

Provide any additional observations about the models above.


---
3Fluid3