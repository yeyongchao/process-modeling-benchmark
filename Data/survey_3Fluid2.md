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



In this example we describe the calculation of the minimum work for ideal compressible adiabatic flow. Most real flows lie somewhere between adiabatic and isothermal flow. For adiabatic flow, the case examined here, you cannot establish a priori the relationship between pressure and density of the gas because the temperature is unknown as a function of pressure or density, hence the relation between pressure and density is derived using the mechanical energy balance. If the gas is assumed to be ideal, and $k = C_{p}/C_{v}$ is assumed to be constant in the range of interest from $p_1$ to $p_2$. 

![image-20251124150838730](./figs/image-20251124150838730.png)

We utilize a three-stage compressor with intercooling back to $T_1$ between stages. We want to determine the optimal interstage pressures $p_2$ and $p_3$ to minimize $\hat{W}$ keeping $p_1$ and $p_4$ fixed.

Build a mathematical model to minimize the total work of a three-stage compressor by optimizing the interstage pressures.



## 2.2 Ground Truth Model

**Process Model (Single Stage):**  

The theoretical work per mole (or mass) of gas compressed for a single-stage compressor is:


$$W = \frac{kRT_{1}}{k-1}\left[\left(\frac{p_{2}}{p_{1}}\right)^{(k-1)/k}-1\right]$$ 



Where $T_1$ is the inlet gas temperature and $R$ the ideal gas constant ($p_{1}\hat{V}_{1}=RT_{1}$). 



**Objective Function (Three-Stage):**  

For a three-stage compressor with intercooling back to $T_1$ between stages, the work of compression from $p_1$ to $p_4$ is:



$$\hat{W} = \frac{kRT_{1}}{k-1}\left[\left(\frac{p_{2}}{p_{1}}\right)^{(k-1)/k} + \left(\frac{p_{3}}{p_{2}}\right)^{(k-1)/k} + \left(\frac{p_{4}}{p_{3}}\right)^{(k-1)/k} - 3\right]$$ 



**Optimization Conditions:**  

To minimize $\hat{W}$, we differentiate with respect to the variables $p_2$ and $p_3$ and set to zero:



$$\frac{\partial\hat{W}}{\partial p_{2}} = 0 = RT_{1}\left[(p_{1})^{(1-k)/k}(p_{2})^{1/k} - (p_{3})^{(k-1)/k}(p_{2})^{(1-2k)/k}\right]$$



$$\frac{\partial\hat{W}}{\partial p_{3}} = 0 = RT_{1}\left[(p_{2})^{(1-k)/k}(p_{3})^{1/k} - (p_{4})^{(k-1)/k}(p_{3})^{(1-2k)/k}\right]$$

<div style="page-break-after: always;"></div>


# Section 3. Questions

## Question 1: Objective Function Comparison

### Model A's Objective Function


$$
\min \quad \hat{W}(p_2, p_3) = C \left[ \left(\frac{p_2}{p_1}\right)^{\alpha} + \left(\frac{p_3}{p_2}\right)^{\alpha} + \left(\frac{p_4}{p_3}\right)^{\alpha} - 3 \right]
$$
*(Note: This objective function represents the total work of compression, which is the sum of the work done in each of the three stages. The goal is to minimize this total work, directly aligning with the core objective of the determined optimization strategy. Here, $C$ and $\alpha$ are constants derived from the gas properties and operating temperature.)*

### Model B's Objective Function



### Total Work Minimization
$$\min \hat{W} = \dot{m}RT_1\frac{k}{k-1}\left[\left(\frac{p_2}{p_1}\right)^{\frac{k-1}{k}} + \left(\frac{p_3}{p_2}\right)^{\frac{k-1}{k}} + \left(\frac{p_4}{p_3}\right)^{\frac{k-1}{k}} - 3\right]$$

**Note**: Minimize total work required for three-stage compression with intercooling

### Model C's Objective Function




$$
\hat{W} = \dot{m} \cdot \frac{k}{k-1} \cdot R \cdot T_1 \left[ \left( \frac{p_2}{p_1} \right)^{\frac{k-1}{k}} - 1 + \left( \frac{p_3}{p_2} \right)^{\frac{k-1}{k}} - 1 + \left( \frac{p_4}{p_3} \right)^{\frac{k-1}{k}} - 1 \right]
$$

**Note**: Minimize the total specific work input to the three-stage compressor with intercooling back to $T_1$.

---

**Question:** Which model has the best objective function?  
Best Model: ___ (Options: Objective Function A, Objective Function B, Objective Function C)

<div style="page-break-after: always;"></div>

## Question 2: Decision Variable Comparison

### Model A's Decision Variable


| Symbol | Meaning | Unit | Corresponding Data Point |
|--------|---------|------|---------------------------|
| $p_2$  | Decision Variable: Outlet pressure of stage 1 (inlet to stage 2) | Pa | Optimal Interstage Pressure ($p_2$) |
| $p_3$  | Decision Variable: Outlet pressure of stage 2 (inlet to stage 3) | Pa | Optimal Interstage Pressure ($p_3$) |
| $p_1$  | Parameter: Inlet pressure to the system (stage 1) | Pa | Inlet pressure ($p_1$) |
| $p_4$  | Parameter: Final outlet pressure from the system (stage 3) | Pa | Final outlet pressure ($p_4$) |
| $T_1$  | Parameter: Inlet temperature to each stage (due to intercooling) | K | Inlet temperature ($T_1$) |
| $k$    | Parameter: Specific heat ratio of the gas ($C_p/C_v$) | dimensionless | Specific heat ratio ($k$) |
| $R$    | Parameter: Specific gas constant | J/(kg·K) | Gas properties |
| $\alpha$ | Parameter: Exponent constant, defined as $(k-1)/k$ for convenience | dimensionless | Derived from $k$ |
| $C$ | Parameter: Constant term, defined as $\frac{k}{k-1} R T_1$ for convenience | J/kg | Derived from $k, R, T_1$ |

### Model B's Decision Variable



### Interstage Pressure 1 ($p_2$)
- **Symbol**: $p_2$
- **Description**: Pressure after first compression stage and before first intercooler

### Interstage Pressure 2 ($p_3$)
- **Symbol**: $p_3$
- **Description**: Pressure after second compression stage and before second intercooler

### Model C's Decision Variable



#### Interstage Pressure 1 ($p_2$)
- **Symbol**: $p_2$
- **Description**: The pressure at the outlet of the first compressor stage and inlet to the first intercooler. This is a decision variable to be optimized to minimize total work. It lies between $p_1$ and $p_4$.

#### Interstage Pressure 2 ($p_3$)
- **Symbol**: $p_3$
- **Description**: The pressure at the outlet of the second compressor stage and inlet to the second intercooler. This is a decision variable to be optimized to minimize total work. It lies between $p_2$ and $p_4$.

---

**Question:** Which model has the most complete decision variables?  
Best Model: ___ (Options: Decision Variable A, Decision Variable B, Decision Variable C)

<div style="page-break-after: always;"></div>

## Question 3: Constraint Comparison

### Model A's Constraints


#### 
 Physical Constraints
$$
p_1 < p_2 < p_3 < p_4
$$
*(Note: This set of inequalities represents the fundamental physical requirement that the pressure must strictly increase after each stage of compression for the system to function correctly.)*

### Model B's Constraints



#### Equality Constraints
1. No explicit equality constraints beyond the objective function structure

#### Inequality Constraints
1. $$p_2 > p_1$$
   - **Note**: Pressure must increase through each compression stage
2. $$p_3 > p_2$$
   - **Note**: Pressure must increase through each compression stage
3. $$p_4 > p_3$$
   - **Note**: Pressure must increase through each compression stage

#### Boundary Conditions
- $p_1$ and $p_4$ are fixed parameters
- $p_1 < p_2 < p_3 < p_4$

### Model C's Constraints



#### Equality Constraints
1. $$ p_1 < p_2 < p_3 < p_4 $$
   - **Note**: Ensures monotonic pressure increase through the compressor stages; physically required for compression.

#### Inequality Constraints
1. $$ p_2 \geq p_1 $$
   - **Note**: The pressure after the first stage must not be less than the inlet pressure (no decompression).
2. $$ p_3 \geq p_2 $$
   - **Note**: The pressure after the second stage must not be less than the pressure after the first stage.
3. $$ p_4 \geq p_3 $$
   - **Note**: The final outlet pressure must not be less than the pressure after the second stage.

#### Boundary Conditions
- $$ p_2 \in [p_1, p_4] $$
- $$ p_3 \in [p_2, p_4] $$
  - **Note**: The interstage pressures must lie within the feasible range bounded by the fixed inlet and outlet pressures.

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
3Fluid2