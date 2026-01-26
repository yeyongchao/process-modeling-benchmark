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




Liquid-liquid extraction is carried out either (1) in a series of well-mixed vessels or stages (well-mixed tanks or in plate column), or (2) in a continuous process, such as a spray column, packed column, or rotating disk column. If the process model is to be represented with integer variables, as in a staged process, MILNP (Glanz and Stichlmair, 1997) or one of the methods described in Chapters 9 and 10 can be employed. This example focuses on optimization in which the model is composed of two first-order, steady-state differential equations (a plug flow model). A similar treat ment can be applied to an axial dispersion model.


Figure E12.2a illustrates a typical steady-state continuous column. The model and the objective function are formulated as follow's.

The process model. Under certain conditions, the plug flow model for an extraction process has an analytical solution. Under other conditions, numerical solutions of the equations must be used. As a practical matter, specifying the model so that an analytical solution exists means assuming that the concentrations are expressed on
a solute-free mole basis, that the equilibrium relation between $Y$ and $X$ is a straight line $Y = mX + B$ (i.e., not necessarily through the origin), and that the operating line is straight, that is, the phases are insoluble. Then the model is

$$
\frac{dX}{dZ} - N_{ox} (X - Y) = 0
$$

$$
\frac{dY}{dZ} - FN_{ox} (Y - X) = 0
$$

where $F = $ extraction factor $(mv_x/v_y)$  
$m = $ distribution coefficient  
$N_{ox} = $ number of transfer units  
$v_x, v_y = $ superficial velocity in raffinate, extract phase, respectively  
$X = $ dimensionless raffinate phase concentration  
$Y = $ dimensionless extract phase concentration  
$Z = $ dimensionless contactor length  


![image-20251124144415872](./figs/image-20251124144415872.png)

Figure E12.2a shows the boundary conditions $X_0$ and $Y_1$. Given values for $m, N_{ox}$, and the length of the column, a solution for $Y_0$ in terms of $v_x$ and $v_y$ can be obtained; $X_1$ is related to $Y_0$ and $F$ via a material balance: $X_1 = 1 - (Y_0/F)$. Hartland and Meck lenburgh (1975) list the solutions for the plug flow model (and also the axial disper sion model) for a linear equilibrium relationship, in terms of $F$:

Build a steady-state plug-flow model and optimization formulation for a liquid-liquid extraction column, and determine operating velocities that maximize extraction rate under flooding and bounds constraints.


---

## 2.2 Ground Truth Model

### Variables and parameters

- Decision variables:
  - $v_x$: superficial velocity in raffinate phase
  - $v_y$: superficial velocity in extract phase

- State/output variables:
  - $X(Z)$: dimensionless raffinate concentration along column
  - $Y(Z)$: dimensionless extract concentration along column
  - $Y_0$: extract concentration at raffinate outlet (top/bottom as defined by $Z$)
  - $X_1$: raffinate concentration at its outlet

- Parameters:
  - $m$: distribution coefficient, $m = 1.5$
  - $N_{ox}$: number of transfer units
  - $F$: extraction factor,
    $$
    F = \frac{m v_x}{v_y}
    $$
  - $Z$: dimensionless column length coordinate, $Z \in [0,1]$ (by definition)
  - Boundary conditions: $X_0, Y_1$ (given)

### Plug-flow model

Assumptions ensuring analytic solution:

- Concentrations are expressed on a solute-free mole basis
- Linear equilibrium relation:
  $$
  Y = mX + B
  $$
- Straight operating line (phases insoluble)
- First-order, steady-state plug-flow model

The plug-flow equations (two first-order ODEs in $Z$) are assumed with solution summarized by Hartland and Mecklenburgh.

Analytical solution for $Y_0$ (in terms of $F$ and $N_{ox}$):

Let
$$
F = \frac{m v_x}{v_y}
$$

Then:
$$
Y_0(F, N_{ox}) =
\frac{
F \left\{ 1 - \exp\left[ N_{ox}(1 - F) \right] \right\}
}{
1 - F \exp\left[ N_{ox}(1 - F) \right]
}
$$

Raffinate outlet concentration $X_1$ (material balance relation):
$$
X_1 = 1 - \frac{Y_0}{F}
$$

### Implicit constraints

- Dimensionless variables require
  $$
  0 \le X(Z) \le 1,\quad 0 \le Y(Z) \le 1,\quad \forall Z \in [0,1]
  $$
  which, in terms of $Y_0, X_1$, implies
  $$
  0 \le Y_0 \le 1,\quad 0 \le X_1 \le 1
  $$
- Relationship between $F, v_x, v_y$:
  $$
  F = \frac{m v_x}{v_y}
  $$

#### Inequality Constraints

1. **Velocity bounds**

   $$
   v_x^{\min} \le v_x \le v_x^{\max}
   $$
   $$
   v_y^{\min} \le v_y \le v_y^{\max}
   $$

   for specified lower and upper bounds (e.g., based on equipment capability).

2. **Flooding constraint**

   A flooding relation limiting simultaneous values of $v_x$ and $v_y$:
   $$
   g(v_x, v_y) \le 0
   $$
   where $g$ encodes the empirical flooding correlation (as per Jackson and Agnew or column design).

3. **Feasibility of extraction factor**

   To avoid singular behavior in the analytic solution:
   $$
   1 - F \exp\left[ N_{ox}(1 - F) \right] \neq 0
   $$
   and sign/size restrictions ensuring physically meaningful $Y_0$ and $X_1$.

### Objective function

Total extraction rate (per unit cross-section, for constant disk rotation speed):

- Objective:
  $$
  \max_{v_x, v_y} \; f(v_x, v_y) = v_y \, Y_0(F(v_x,v_y), N_{ox})
  $$
  subject to:
  - $F = \dfrac{m v_x}{v_y}$
  - Analytical expression for $Y_0(F, N_{ox})$ above
  - Velocity bounds and flooding constraint
  - Implicit feasibility constraints on $Y_0, X_1$.

<div style="page-break-after: always;"></div>


# Section 3. Questions

## Question 1: Objective Function Comparison

### Model A's Objective Function



$$
f = v_y Y_0
$$

**Note**: Maximize the total extraction rate, defined as the product of the extract phase velocity and the solute concentration in the extract outlet stream. This represents the total molar flow rate of the extracted solute, which is the primary performance metric for the column.

### Model B's Objective Function


$$
\max_{v_x, v_y} \quad f(v_x, v_y) = v_y Y_0
$$
*(Note: This objective function aims to maximize the total extraction rate of the solute. The term $Y_0$ represents the outlet concentration of the solute in the extract phase, which is a complex, non-linear function of the decision variables $v_x$ and $v_y$ as defined in the constraints.)*

### Model C's Objective Function



### Extraction Rate Maximization
$$f = v_y Y_0$$

**Note**: Maximize total extraction rate for constant disk rotation speed, where $Y_0$ is the extract outlet concentration

---

**Question:** Which model has the best objective function?  
Best Model: ___ (Options: Objective Function A, Objective Function B, Objective Function C)

<div style="page-break-after: always;"></div>

## Question 2: Decision Variable Comparison

### Model A's Decision Variable


### Superficial Velocity of Raffinate Phase ($v_x$)
- **Symbol**: $v_x$
- **Description**: The superficial velocity of the raffinate (feed) phase flowing downward in the extraction column. It represents the volumetric flow rate per unit cross-sectional area of the column and directly influences the residence time and mass transfer efficiency. Higher $v_x$ increases throughput but may reduce contact efficiency or lead to flooding.

### Superficial Velocity of Extract Phase ($v_y$)
- **Symbol**: $v_y$
- **Description**: The superficial velocity of the extract (solvent) phase flowing upward in the extraction column. It governs the solvent flow rate per unit area and affects the driving force for mass transfer. Increasing $v_y$ enhances extraction rate but is constrained by flooding and operational limits.

### Model B's Decision Variable


| Symbol | Meaning | Unit | Corresponding Data Point |
|--------|---------|------|---------------------------|
| $v_x$  | Superficial velocity in the raffinate phase | m/s | Decision Variable |
| $v_y$  | Superficial velocity in the extract phase | m/s | Decision Variable |
| $m$    | Distribution coefficient | dimensionless | 1.5 |
| $a$    | Coefficient for $N_{ox}$ correlation | varies | Parameter (Value Unknown) |
| $b$    | Exponent for $v_x$ in $N_{ox}$ correlation | dimensionless | Parameter (Value Unknown) |
| $c$    | Exponent for $v_y$ in $N_{ox}$ correlation | dimensionless | Parameter (Value Unknown) |
| $v_{x,min}$ | Lower bound for raffinate velocity | m/s | 0.01 |
| $v_{x,max}$ | Upper bound for raffinate velocity | m/s | 0.20 |
| $v_{y,min}$ | Lower bound for extract velocity | m/s | 0.05 |
| $v_{y,max}$ | Upper bound for extract velocity | m/s | 0.20 |
| $C_{flood}$ | Flooding constraint limit | (m/s)$^{0.5}$ | 0.25 |

### Model C's Decision Variable



#### Raffinate Superficial Velocity ($v_x$)
- **Symbol**: $v_x$
- **Description**: Superficial velocity of the raffinate phase (feed-rich phase flowing downward) in the extraction column

#### Extract Superficial Velocity ($v_y$)
- **Symbol**: $v_y$
- **Description**: Superficial velocity of the extract phase (solvent-rich phase flowing upward) in the extraction column

---

**Question:** Which model has the most complete decision variables?  
Best Model: ___ (Options: Decision Variable A, Decision Variable B, Decision Variable C)

<div style="page-break-after: always;"></div>

## Question 3: Constraint Comparison

### Model A's Constraints

#### Flooding Constraint Coefficient ($C_f$)
- **Symbol**: $C_f$
- **Description**: Empirical constant used in the flooding constraint correlation $v_x^{0.5} + v_y^{0.5} \leq C_f$, derived from experimental data to define the maximum combined velocity before flooding occurs.

### Model B's Constraints


#### Physical Constraints
$$
v_x^{0.5} + v_y^{0.5} \leq C_{flood}
$$
*(Note: This constraint ensures that the combination of superficial velocities does not cause the column to flood, which is a critical condition for stable and efficient operation.)*

#### Operational Constraints
$$
v_{x,min} \leq v_x \leq v_{x,max}
$$
$$
v_{y,min} \leq v_y \leq v_{y,max}
$$
*(Note: These constraints ensure that the superficial velocities of both phases remain within the specified operational or equipment design limits.)*

#### Definitional Constraints (Equality)
$$
F = \frac{m v_x}{v_y}
$$
*(Note: This equation defines the extraction factor $F$, a key dimensionless group that influences separation efficiency.)*
$$
N_{ox} = a v_x^b v_y^c
$$
*(Note: This equation defines the number of transfer units $N_{ox}$, which quantifies the mass transfer efficiency of the column as a function of the phase velocities.)*
$$
Y_0 = \frac{F \left\{ 1 - \exp[N_{ox}(1 - F)] \right\}}{1 - F \exp[N_{ox}(1 - F)]}
$$
*(Note: This is the analytical solution to the plug flow model, defining the outlet extract concentration $Y_0$ based on the intermediate variables $F$ and $N_{ox}$. This equation links the decision variables to the objective function.)*

### Model C's Constraints



#### Equality Constraints

1. $$Y_0 = \frac{F\left\{1 - \exp[N_{ox}(1 - F)]\right\}}{1 - F \exp[N_{ox}(1 - F)]}$$
   - **Note**: Analytical solution for extract outlet concentration from plug flow model with linear equilibrium

2. $$F = \frac{m v_x}{v_y}$$
   - **Note**: Definition of extraction factor relating distribution coefficient and phase velocities

3. $$X_1 = 1 - \frac{Y_0}{F}$$
   - **Note**: Material balance relating raffinate outlet concentration to extract outlet concentration and extraction factor

#### Inequality Constraints

1. $$v_x^{min} \leq v_x \leq v_x^{max}$$
   - **Note**: Lower and upper bounds on raffinate superficial velocity

2. $$v_y^{min} \leq v_y \leq v_y^{max}$$
   - **Note**: Lower and upper bounds on extract superficial velocity

3. $$v_x + v_y \leq v_{flood}$$
   - **Note**: Flooding constraint limiting the sum of phase velocities to prevent column flooding

#### Boundary Conditions
- $X = X_0$ at column feed entry point
- $Y = Y_1$ at column solvent entry point

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
2Separation2