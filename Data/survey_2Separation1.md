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



Distillation is probably the most widely used separation process in industry. Various classes of optimization problems for steady-state distillation are, in increasing order of complexity,
1. Determine the optimal operating conditions for an existing column to achieve specific performance at minimum cost (or minimum energy usage) given the feed(s). Usually, the manipulated (independent) variables are indirect heat inputs, cooling stream inputs, and product flow rates. The number of degrees of freedom is most likely equal to the number of product streams. Specific performance is measured by specified component concentrations or fractional recoveries from the feed (specifications leading to equality constraints) or minimum (or maximum) concentrations and recoveries (specifications leading to inequality constraints). In principle, any of the specified quantities as well as costs can be calculated from the values of the manipulated variables given the mathematical model (or computer code) for the column. When posed as described earlier, the optimization problem is a nonlinear programming problem often with implicit nested loops for calculation of physical properties. If the number of degrees of freedom is reduced to zero by specifications placed on the controlled variables, the optimization problem reduces to the classic problem of distillation design that requires just the solution of a set of nonlinear equations.
2. A more complex problem is to determine not only the values of the operating conditions as outlined in item 1 but also the (minimum) number of stages required for the separation. Because the stages are discrete (although in certain examples in this book we have treated them as continuous variables), the problem outlined in item 1 becomes a nonlinear mixed-integer programming problem (see Chapter 9). In this form of the design problem, the costs include both capital costs and operating costs. Capital costs increase with the number of stages and internal column flow rates, whereas operating costs decrease up to a certain point.
3. An even more difficult problem is to determine the number of stages and the optimal locations for the feed(s) and side strearn(s) withdrawal. Fortunately, the range of candidates for stage locations for feed and withdrawals is usually small, and from a practical viewpoint the objective function is usually not particularly sensitive to a specific location within the appropriate range.

Optimization of distillation columns using mathematical programming, as opposed to other methods, has been carried out using many techniques, including search methods such as Hooke and Jeeves (Srygley and Holland, 1965), mixed-integer nonlinear programming (MINLP) (Frey et al., 1997; and Bauer and Stichlmair, 1998), genetic algorithms (Fraga and Matias, 1996), and successive quadratic programming (SQP) (Schmid and Biegler, 1994), which is the technique we use in this example. The review by Skogestad (1997) treats many of the various issues involved in the optimization of distillation columns beyond those we illustrate here.


![image-20251124143730434](./figs/image-20251124143730434.png)

This example focuses on the design and optimization of a steady-state staged column. Figure E12.1 shows a typical column and some of the notation we will use, and Table E12.1A lists the other variables and parameters. Feed is denoted by superscript F. Withdrawals take the subscripts of the withdrawal stage. Superscripts V for vapor and L for liquid are used as needed to distinguish between phases. If we number the stages from the bottom of the column (the reboiler) upward with $k = 1$, then $V_0 = L_1 = 0$, and at the top of the column, or the condenser, $V_n = L_{n+1} = 0$. We first formulate the equality constraints, then the inequality constraints, and lastly the objective function.

Build a steady-state mathematical model and optimization formulation for the design and operation of a conventional staged-distillation column, based on the following description.

---

## 2.2 Ground Truth Model

The mathematical model for the conventional staged-distillation column and its optimization consists of:

### Sets and indices

- Stages: $k = 1, 2, \dots, n$ (with $k = 1$ reboiler, $k = n$ condenser)  
- Components: $i = 1, 2, \dots, m$

### Variables (per stage $k$ and component $i$ as appropriate)

- $F_k^V, F_k^L$: vapor and liquid feed flows to stage $k$ (moles/time)
- $V_k$: vapor flow leaving stage $k$ (moles/time)
- $L_k$: liquid flow leaving stage $k$ (moles/time)
- $W_k^V, W_k^L$: vapor and liquid withdrawal flows from stage $k$ (moles/time)
- $x_{ik}$: liquid-phase mole fraction of component $i$ on stage $k$
- $y_{ik}$: vapor-phase mole fraction of component $i$ on stage $k$
- $T_k$: temperature on stage $k$
- $p_k$: pressure on stage $k$
- $Q_k$: heat transfer rate to stage $k$ (positive into stage)
- $h_k^F$: feed liquid enthalpy on stage $k$ (function of $p_k, T_k, x_{ik}$)
- $h_k$: liquid enthalpy on stage $k$ (function of $p_k, T_k, x_{ik}$)
- $H_k$: vapor enthalpy on stage $k$ (function of $p_k, T_k, y_{ik}$)
- $K_{ik}$: equilibrium constant for component $i$ on stage $k$ (function of $p_k, T_k, x_{ik}, y_{ik}$)

Boundary conditions for internal flows:

- $V_0 = 0$
- $L_{n+1} = 0$

#### Equality Constraints (process model)

1. **Total material balance on each stage $k$**

   For all $k = 1, \dots, n$:
   $$
   F_k^V + F_k^L + V_{k-1} + L_{k+1} = V_k + L_k + W_k^V + W_k^L
   $$
   (Note: $F_k$ and $W_k$ are ordinarily not involved in most of the stages.)

2. **Component material balances**

   For each component $i = 1,\dots,m$ and stage $k = 1,\dots,n$, a component balance of the form

   $$
   \text{(in of component $i$ to stage $k$)} = \text{(out of component $i$ from stage $k$)}
   $$
   
   that is consistent with the total balance, e.g., schematically:
   
   $$
   F_k^V y_{i,k}^F + F_k^L x_{i,k}^F + V_{k-1} y_{i,k-1} + L_{k+1} x_{i,k+1} =
   V_k y_{ik} + L_k x_{ik} + W_k^V y_{i,k}^W + W_k^L x_{i,k}^W
   $$
   
   (The exact form follows the same flow structure as the total material balance.)

3. **Energy balance on each stage $k$**

   For all $k = 1,\dots,n$:

   $$
   Q_k + h_k^F F_k + H_{k-1} V_{k-1} + h_{k+1} L_{k+1} =
   H_k V_k + h_k L_k + H_k W_k^V + h_k W_k^L
   \tag{c}
   $$

4. **Phase equilibrium relations**

   For each component $i$ and stage $k$:
   
   $$
   y_{ik} = K_{ik} x_{ik}
   $$

5. **Equilibrium-constant relations**

   For each $i, k$:

   $$
   K_{ik} = K_i(p_k, T_k, x_{1k}, \dots, x_{mk}, y_{1k}, \dots, y_{mk})
   $$
   
   (given by a thermodynamic correlation or model).

6. **Enthalpy relations**

   For each stage $k$:
   
   $$
   h_k = h(p_k, T_k, x_{1k}, \dots, x_{mk})
   $$
   
   $$
   H_k = H(p_k, T_k, y_{1k}, \dots, y_{mk})
   $$

7. **Implicit equality constraints**

   These arise from the specification of operating conditions, such as:

   - Overall and component material balances over the entire column (feed(s), product(s), and side streams)
   - Summation constraints on mole fractions:
     $$
     \sum_{i=1}^{m} x_{ik} = 1, \quad
     \sum_{i=1}^{m} y_{ik} = 1, \quad \forall k
     $$
   - Fixed stage pressures $p_k$ and specified feed and side-stream locations, feed rates, and enthalpies, as given data.

8. **Specification of fixed parameters**

   Frequently given:

   - Number of stages $n$
   - Flow rate, composition, and enthalpy of the feed(s)
   - Location of the feed(s) and side stream withdrawal(s)
   - Flow rate of the side stream(s)
   - Heat input rate to each stage except one
   - Stage pressures $p_k$

   These appear as fixed values in the corresponding equations above.

#### Inequality Constraints

1. **Nonnegativity of physical flows and heats**

   For all $k$:
   $$
   V_k \ge 0,\quad L_k \ge 0,\quad F_k^V \ge 0,\quad F_k^L \ge 0,\quad
   W_k^V \ge 0,\quad W_k^L \ge 0,\quad Q_k \text{ satisfying specified bounds}
   $$

2. **Product specifications and bounds**

   - Upper and lower bounds on certain product concentrations (e.g., top and bottom product compositions):
     $$
     x_{i,\text{top}}^{\min} \le x_{i,\text{top}} \le x_{i,\text{top}}^{\max}, \quad
     x_{i,\text{bottom}}^{\min} \le x_{i,\text{bottom}} \le x_{i,\text{bottom}}^{\max}
     $$

3. **Recovery factors**

   A recovery factor for stage $k$ is defined as the ratio
   $$
   \text{Recovery factor}_k = 
   \frac{\text{amount of component recovered in the product associated with stage $k$}}
        {\text{amount of that component in the feed}}
   $$
   and is constrained to satisfy minimum (or maximum) specified values:
   $$
   \text{Recovery factor}_k \ge \text{Recovery}_k^{\min}
   $$
   (or analogous inequalities).

4. **Bounds on manipulated variables**

   For manipulated variables such as $Q_1$ and reflux ratio (if used as an independent variable), bounds:
   $$
   Q_1^{\min} \le Q_1 \le Q_1^{\max}
   $$
   and similar for any other manipulated variable (e.g., reflux ratio, product flow rates).

### Objective function

The annual operating cost is assumed to be dominated by the reboiler heat input. We therefore take:

- Decision variable (for simplest case): $Q_1$ (reboiler heat input)

Objective:
$$
\min_{Q_1, \text{other free variables}} \; Q_1
\tag{j}
$$

subject to all equality and inequality constraints above.

If desired, condenser duty $Q_n$ is determined from the energy balance, and all other $Q_k$ are assumed zero.

### Specific 4-stage problem with 5 degrees of freedom

For the particular case:

- A 4-stage column ($n=4$), with stage 1 reboiler, stage 4 condenser
- Total feed: 100 lb mol/h liquid
- Feed enthalpy: $h_F = 4000$ Btu/lb mol
- Top product: 10 lb mol/h liquid
- Equality constraints: general Equations (a)–(i) plus overall feed/product and enthalpy specifications as stated
- Inequality constraints: for $k = 1,\dots,4$, nonnegativity and composition/recovery bounds as above
- Decision variables (degrees of freedom): $Q_1, F_1, F_2, F_3, F_4$

Optimization problem:

$$
\min_{Q_1, F_1, F_2, F_3, F_4} \; Q_1
$$

subject to:

- Stage-wise balances and thermodynamic relations (Equations (a)–(i))
- Overall feed constraint:
  $$
  \sum_{k=1}^{4} F_k = 100 \;\text{lb mol/h}
  $$
- Top product flow constraint:
  $$
  L_4 - W_4^L = 10 \;\text{lb mol/h}
  $$
- All inequality constraints on flows, compositions, and recovery factors.

<div style="page-break-after: always;"></div>


# Section 3. Questions

## Question 1: Objective Function Comparison

### Model A's Objective Function




$$
\min \quad Q_1
$$

**Note**: Minimize the reboiler heat duty $Q_1$, which dominates the operating cost. This is justified by the problem description, which assumes other heat duties $Q_k = 0$ for $k=2,\dots,n-1$, and $Q_n$ is determined implicitly by energy balance. Operating cost is assumed proportional to $Q_1$, and capital costs are fixed.

### Model B's Objective Function


$$
\min \quad Q_1
$$
*(Note: This objective function aims to minimize the reboiler heat duty, which is the primary driver of operating costs for the distillation process. This directly aligns with the chosen strategy of "Operational Energy Minimization".)*

### Model C's Objective Function




$$Q_1$$

**Note**: Minimize - Annual operating cost is assumed to be directly proportional to the reboiler heat duty $Q_1$

---

**Question:** Which model has the best objective function?  
Best Model: ___ (Options: Objective Function A, Objective Function B, Objective Function C)

<div style="page-break-after: always;"></div>

## Question 2: Decision Variable Comparison

### Model A's Decision Variable



#### Vapor Flow Rate at Stage $k$ ($V_k$)
- **Symbol**: $V_k$
- **Description**: Molar flow rate of vapor leaving stage $k$ (in mol/h). This variable represents the upward vapor flow in the column and is determined by material and energy balances. It is a key state variable that links adjacent stages and influences separation efficiency and energy consumption.

#### Liquid Flow Rate at Stage $k$ ($L_k$)
- **Symbol**: $L_k$
- **Description**: Molar flow rate of liquid leaving stage $k$ (in mol/h). This variable represents the downward liquid flow in the column and is coupled with vapor flows through stage-wise material balances. It affects contact efficiency and product purity.

#### Temperature at Stage $k$ ($T_k$)
- **Symbol**: $T_k$
- **Description**: Temperature of the liquid-vapor mixture on stage $k$ (in K or °R). Temperature governs phase equilibrium, enthalpy values, and mass transfer rates. It is an internal state variable determined by energy balances and equilibrium relations.

#### Liquid Mole Fraction of Component $i$ at Stage $k$ ($x_{ik}$)
- **Symbol**: $x_{ik}$
- **Description**: Mole fraction of component $i$ in the liquid phase on stage $k$. This variable describes the composition of the liquid stream and is critical for determining purity of products and equilibrium conditions.

#### Vapor Mole Fraction of Component $i$ at Stage $k$ ($y_{ik}$)
- **Symbol**: $y_{ik}$
- **Description**: Mole fraction of component $i$ in the vapor phase on stage $k$. This variable describes the composition of the vapor stream and is linked to $x_{ik}$ via equilibrium relations. It determines the composition of distillate and bottoms products.

#### Heat Input to Stage $k$ ($Q_k$)
- **Symbol**: $Q_k$
- **Description**: Rate of heat transfer to stage $k$ (in Btu/h or kW). Positive values indicate heat addition (e.g., reboiler), negative values indicate heat removal (e.g., condenser). In the optimization problem, $Q_1$ (reboiler) is the primary decision variable; other $Q_k$ are typically fixed at zero except for $Q_n$ (condenser), which is determined implicitly.

#### Feed Flow Rate to Stage $k$ ($F_k$)
- **Symbol**: $F_k$
- **Description**: Molar flow rate of liquid feed introduced at stage $k$ (in mol/h). This variable determines how the total feed is distributed across stages. In the optimization problem, $F_k$ for $k=1,\dots,n$ are decision variables subject to total feed constraint.

#### Withdrawal Flow Rate from Stage $k$ ($W_k$)
- **Symbol**: $W_k$
- **Description**: Molar flow rate of product stream withdrawn from stage $k$ (in mol/h). This includes side streams or product draws (e.g., distillate or bottoms). In the problem, $W_k$ may be fixed or optimized depending on context; here, we assume bottoms $W_1$ and distillate $W_{n+1}$ are outputs, but side streams $W_k$ for $2 \leq k \leq n$ may be decision variables.

#### Reflux Ratio ($R$)
- **Symbol**: $R$
- **Description**: Ratio of reflux flow rate to distillate flow rate, $R = L_n / D$, where $D$ is the distillate flow rate. This variable controls the separation efficiency in the rectifying section. In some formulations, $R$ is a decision variable; in others, it is derived from $L_n$ and $D$. Here, we treat $L_n$ and $D$ explicitly, so $R$ is not a separate variable.

#### Distillate Flow Rate ($D$)
- **Symbol**: $D$
- **Description**: Molar flow rate of the top product (distillate) withdrawn from the reflux drum (in mol/h). This is a key product specification variable and may be fixed or optimized. In the example, it is fixed, but in general, it can be a decision variable.

#### Bottoms Flow Rate ($B$)
- **Symbol**: $B$
- **Description**: Molar flow rate of the bottom product withdrawn from the reboiler (in mol/h). This is determined by total material balance: $B = F_{\text{total}} - D$. In the example, it is derived, but in general, it may be optimized.

### Model B's Decision Variable


| Symbol | Meaning | Unit | Corresponding Data Point |
|---|---|---|---|
| **Decision Variables** | | | |
| $F_k$ | Feed flow rate to stage $k$, for $k \in \{1, 2, 3, 4\}$ | lb mol/h | Adjustable Variable |
| $Q_1$ | Heat duty of the reboiler (stage 1) | Btu/h | Adjustable Variable |
| **Dependent/State Variables** | | | |
| $V_k$ | Vapor flow rate leaving stage $k$ | lb mol/h | Calculated by model |
| $L_k$ | Liquid flow rate leaving stage $k$ | lb mol/h | Calculated by model |
| $T_k$ | Temperature of stage $k$ | °F or K | Calculated by model |
| $x_{ik}$ | Mole fraction of component $i$ in the liquid on stage $k$ | dimensionless | Calculated by model |
| $y_{ik}$ | Mole fraction of component $i$ in the vapor on stage $k$ | dimensionless | Calculated by model |
| $Q_4$ | Heat duty of the condenser (stage 4) | Btu/h | Calculated by model |
| **Parameters** | | | |
| $n$ | Total number of stages | dimensionless | 4 |
| $m$ | Total number of components | dimensionless | Not Specified |
| $F_{total}$ | Total feed flow rate | lb mol/h | 100 |
| $x_{i}^F$ | Mole fraction of component $i$ in the feed | dimensionless | Not Specified |
| $h_F$ | Specific enthalpy of the feed | Btu/lb mol | 4000 |
| $L_{4}^{spec}$ | Specified liquid flow rate from stage 4 | lb mol/h | 10 |
| $x_{1,4}^{min}$ | Minimum mole fraction of component 1 in top product | dimensionless | 0.95 |
| $p_k$ | Pressure on stage $k$ | psi or Pa | Not Specified |
| $K_{ik}$ | Vapor-liquid equilibrium constant for component $i$ on stage $k$ | dimensionless | Physical Property Model |
| $h_k$ | Specific enthalpy of liquid on stage $k$ | Btu/lb mol | Physical Property Model |
| $H_k$ | Specific enthalpy of vapor on stage $k$ | Btu/lb mol | Physical Property Model |

### Model C's Decision Variable



#### Heat Input to Reboiler ($Q_1$)
- **Symbol**: $Q_1$
- **Description**: Heat transfer flow to stage 1 (reboiler), which is the primary manipulated variable for energy optimization

#### Feed Flow Rates ($F_k$)
- **Symbol**: $F_k$ for $k = 1,\dots,n$
- **Description**: Flow rate of feed entering stage $k$, representing feed distribution optimization

#### Withdrawal Flow Rates ($W_k$)
- **Symbol**: $W_k^V$, $W_k^L$ for $k = 1,\dots,n$
- **Description**: Vapor and liquid withdrawal streams from stage $k$, representing product stream optimization

#### Internal Flows ($V_k$, $L_k$)
- **Symbol**: $V_k$, $L_k$ for $k = 1,\dots,n$
- **Description**: Vapor and liquid flow rates between stages, determined by mass and energy balances

#### Compositions ($x_{i,k}$, $y_{i,k}$)
- **Symbol**: $x_{i,k}$, $y_{i,k}$ for $i = 1,\dots,m$, $k = 1,\dots,n$
- **Description**: Mole fractions of component $i$ in liquid and vapor phases on stage $k$

#### Temperatures ($T_k$)
- **Symbol**: $T_k$ for $k = 1,\dots,n$
- **Description**: Temperature on stage $k$, determined by equilibrium relationships

---

**Question:** Which model has the most complete decision variables?  
Best Model: ___ (Options: Decision Variable A, Decision Variable B, Decision Variable C)

<div style="page-break-after: always;"></div>

## Question 3: Constraint Comparison

### Model A's Constraints



#### Equality Constraints

1. $$F_k^L + V_{k-1} + L_{k+1} = V_k + L_k + W_k^L + W_k^V \quad \forall k = 1, \dots, n$$
   - **Note**: Total material balance on stage $k$. $F_k^L$ is the liquid feed at stage $k$ (assumed to be the only feed form); $W_k^L$ and $W_k^V$ are liquid and vapor withdrawals (often one is zero). For the reboiler ($k=1$), $V_0 = 0$; for the condenser ($k=n$), $L_{n+1} = 0$. Withdrawals are typically only liquid or vapor, not both.

2. $$F_k z_i^F + V_{k-1} y_{i,k-1} + L_{k+1} x_{i,k+1} = V_k y_{ik} + L_k x_{ik} + W_k^L x_{ik}^W + W_k^V y_{ik}^W \quad \forall i = 1,\dots,m; \; \forall k = 1,\dots,n$$
   - **Note**: Component material balance for component $i$ on stage $k$. $x_{ik}^W$ and $y_{ik}^W$ are the mole fractions of component $i$ in the withdrawal stream from stage $k$. For non-withdrawal stages, $W_k^L = W_k^V = 0$. Feed composition $z_i^F$ is known.

3. $$Q_k + h_k^F F_k + H_{k-1} V_{k-1} + h_{k+1} L_{k+1} = H_k V_k + h_k L_k + H_k W_k^V + h_k W_k^L \quad \forall k = 1, \dots, n$$
   - **Note**: Energy balance on stage $k$. $h_k^F$ is the enthalpy of the feed stream at stage $k$. $Q_k$ is the heat input to stage $k$. Enthalpies $h_k$, $H_k$ are nonlinear functions of composition and temperature.

4. $$y_{ik} = K_{ik} x_{ik} \quad \forall i = 1,\dots,m; \; \forall k = 1,\dots,n$$
   - **Note**: Vapor-liquid equilibrium relation. Assumes ideal or near-ideal behavior; $K_{ik}$ is a function of $p_k$, $T_k$, and $x_{ik}$ (e.g., $K_{ik} = \gamma_i p_i^{\text{sat}}(T_k)/p_k$ via modified Raoult’s law).

5. $$\sum_{i=1}^m x_{ik} = 1 \quad \forall k = 1,\dots,n$$
   - **Note**: Liquid phase mole fraction summation constraint.

6. $$\sum_{i=1}^m y_{ik} = 1 \quad \forall k = 1,\dots,n$$
   - **Note**: Vapor phase mole fraction summation constraint.

7. $$\sum_{k=1}^n F_k = F_{\text{total}}$$
   - **Note**: Total feed constraint. Sum of all feed streams equals total feed flow rate.

8. $$L_n = R \cdot D \quad \text{(if reflux ratio } R \text{ is fixed)}$$
   - **Note**: Reflux relation. If $R$ is fixed, this links reflux flow to distillate. If $R$ is not fixed, then $L_n$ and $D$ are both decision variables with $D$ fixed as parameter.

9. $$B = F_{\text{total}} - D$$
   - **Note**: Bottoms flow rate derived from overall material balance.

10. $$V_0 = 0, \quad L_{n+1} = 0$$
    - **Note**: Boundary conditions for vapor and liquid flows at column ends.

#### Inequality Constraints

1. $$V_k > 0 \quad \forall k = 1, \dots, n$$
   - **Note**: Vapor flow rates must be positive to ensure physical feasibility and avoid column flooding or weeping.

2. $$L_k > 0 \quad \forall k = 1, \dots, n$$
   - **Note**: Liquid flow rates must be positive to ensure proper liquid-vapor contact.

3. $$F_k \geq 0 \quad \forall k = 1, \dots, n$$
   - **Note**: Feed flow rates cannot be negative.

4. $$W_k \geq 0 \quad \forall k = 1, \dots, n$$
   - **Note**: Withdrawal flow rates must be non-negative.

5. $$Q_k \geq 0 \quad \forall k = 1, \dots, n-1; \quad Q_n \leq 0$$
   - **Note**: Reboiler ($Q_1$) and intermediate stages ($Q_2,\dots,Q_{n-1}$) must add heat (positive); condenser ($Q_n$) must remove heat (negative). In the optimization, $Q_1 > 0$ is enforced implicitly by minimization.

6. $$0 \leq x_{ik} \leq 1, \quad 0 \leq y_{ik} \leq 1 \quad \forall i,k$$
   - **Note**: Mole fractions must lie within physical bounds.

7. $$\frac{\sum_{k=1}^n W_k^L x_{ik}^W}{\sum_{k=1}^n F_k z_i^F} \geq \eta_i^{\min} \quad \forall i \in \text{distillate components}$$
   - **Note**: Minimum recovery constraint for component $i$ in the distillate. For example, if component $i$ is to be recovered in the top product, its recovery fraction must exceed a specified minimum.

8. $$\frac{\sum_{k=1}^n W_k^L (1 - x_{ik}^W)}{\sum_{k=1}^n F_k (1 - z_i^F)} \geq \eta_i^{\min} \quad \forall i \in \text{bottoms components}$$
   - **Note**: Minimum recovery constraint for component $i$ in the bottoms product.

#### Boundary Conditions

- $$V_0 = 0$$
- $$L_{n+1} = 0$$
- $$x_{i1} = x_{i,B} \quad \text{(bottoms composition)}$$
- $$y_{in} = y_{i,D} \quad \text{(distillate composition)}$$

### Model B's Constraints


#### 
 Physical Constraints (MESH Equations)
*(Note: These equations represent the fundamental laws of conservation of mass and energy, and phase equilibrium for each stage $k \in \{1, ..., 4\}$ and each component $i \in \{1, ..., m\}$)*

- **Total Material Balance:**
$$
F_k + V_{k-1} + L_{k+1} - V_k - L_k = 0
$$
*(Note: This is a simplified form assuming no side-stream withdrawals, consistent with the problem description. Column boundaries are defined by $V_0=0$ and $L_5=0$.)*

- **Component Material Balance:**
$$
F_k x_i^F + V_{k-1} y_{i,k-1} + L_{k+1} x_{i,k+1} - V_k y_{ik} - L_k x_{ik} = 0
$$

- **Energy Balance:**
$$
Q_k + F_k h_F + V_{k-1} H_{k-1} + L_{k+1} h_{k+1} - V_k H_k - L_k h_k = 0
$$
*(Note: Heat is only exchanged at the ends: $Q_k=0$ for $k=2,3$.)*

- **Phase Equilibrium:**
$$
y_{ik} = K_{ik}(T_k, p_k, \mathbf{x}_k, \mathbf{y}_k) x_{ik}
$$

- **Summation Constraints:**
$$
\sum_{i=1}^{m} x_{ik} = 1 \quad \text{and} \quad \sum_{i=1}^{m} y_{ik} = 1
$$

#### 
 Operational Constraints
- **Total Feed Constraint:**
$$
\sum_{k=1}^{4} F_k = F_{total}
$$
*(Note: This ensures that the sum of all distributed feeds equals the total available feed rate of 100 lb mol/h.)*

- **Top Product Flow Specification:**
$$
L_4 = L_{4}^{spec}
$$
*(Note: This constraint fixes the internal liquid flow from the condenser (stage 4) to stage 3 at 10 lb mol/h, as specified in the problem statement.)*

- **Top Product Purity Specification:**
$$
x_{1,4} \ge x_{1,4}^{min}
$$
*(Note: This inequality ensures that the mole fraction of the desired component (component 1) in the liquid product from the top stage meets the minimum quality requirement of 0.95.)*

#### 
 Logical Constraints
- **Non-negativity Constraints:**
$$
Q_1 \ge 0
$$
$$
F_k \ge 0, \quad \forall k \in \{1, 2, 3, 4\}
$$
$$
V_k \ge 0, \quad L_k \ge 0, \quad \forall k \in \{1, 2, 3, 4\}
$$
$$
0 \le x_{ik} \le 1, \quad 0 \le y_{ik} \le 1, \quad \forall i \in \{1, ..., m\}, \forall k \in \{1, ..., 4\}
$$
*(Note: These constraints ensure that all physical quantities like flows, heat duties, and compositions are physically meaningful.)*

### Model C's Constraints



#### Equality Constraints

1. **Total Material Balance**
   $$F_k^V + F_k^L + V_{k-1} + L_{k+1} = V_k + L_k + W_k^V + W_k^L \quad \text{for } k = 1,\dots,n$$
   - **Note**: Conservation of total mass on each stage

2. **Component Material Balance**
   $$F_k^V y_{i,k}^F + F_k^L x_{i,k}^F + V_{k-1} y_{i,k-1} + L_{k+1} x_{i,k+1} = V_k y_{i,k} + L_k x_{i,k} + W_k^V y_{i,k} + W_k^L x_{i,k} \quad \text{for } i = 1,\dots,m, \ k = 1,\dots,n$$
   - **Note**: Conservation of each component mass on each stage

3. **Energy Balance**
   $$Q_k + h_k^F F_k + H_{k-1} V_{k-1} + h_{k+1} L_{k+1} = H_k V_k + h_k L_k + H_k W_k^V + h_k W_k^L \quad \text{for } k = 1,\dots,n$$
   - **Note**: Conservation of energy on each stage

4. **Phase Equilibrium**
   $$y_{i,k} = K_{i,k} x_{i,k} \quad \text{for } i = 1,\dots,m, \ k = 1,\dots,n$$
   - **Note**: Vapor-liquid equilibrium relationship

5. **Summation Constraints**
   $$\sum_{i=1}^m x_{i,k} = 1, \quad \sum_{i=1}^m y_{i,k} = 1 \quad \text{for } k = 1,\dots,n$$
   - **Note**: Mole fractions must sum to unity in each phase

6. **Physical Property Relations**
   $$K_{i,k} = f_K(p_k, T_k, x_{i,k}, y_{i,k}), \quad h_k = f_h(p_k, T_k, x_{i,k}), \quad H_k = f_H(p_k, T_k, y_{i,k})$$
   - **Note**: Functional relationships for equilibrium constants and enthalpies

#### Inequality Constraints

1. **Non-negativity Constraints**
   $$V_k \geq 0, \quad L_k \geq 0, \quad F_k \geq 0, \quad W_k \geq 0, \quad x_{i,k} \geq 0, \quad y_{i,k} \geq 0$$
   - **Note**: All flow rates and compositions must be non-negative

2. **Product Specifications**
   $$x_{i,\text{product}} \geq x_{i,\text{min}} \quad \text{or} \quad x_{i,\text{product}} \leq x_{i,\text{max}}$$
   - **Note**: Minimum or maximum concentration requirements for product streams

3. **Recovery Factors**
   $$\frac{\text{Component in product}}{\text{Component in feed}} \geq R_{i,\text{min}}$$
   - **Note**: Minimum recovery requirements for valuable components

#### Boundary Conditions

- **Bottom Stage**: $V_0 = 0$, $L_1 = 0$ (reboiler conditions)
- **Top Stage**: $V_n = 0$, $L_{n+1} = 0$ (condenser conditions)
- **Heat Duties**: $Q_k = 0$ for $k = 2,\dots,n-1$ (heat transfer only at ends)

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
2Separation1