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



Reactor systems that can be described by a "yield matrix" are potential candidates for the application of linear programming. In these situations, each reactant is known to produce a certain distribution of products. When multiple reactants are employed, it is desirable to optimize the amounts of each reactant so that the products satisfy flow and demand constraints. In this example, we illustrate the use of linear programming to optimize the operation of a thermal cracker sketched in Figure E14.1.


![image-20251124152045227](./figs/image-20251124152045227.png)

Table E14.1A shows various feeds and the corresponding product distribution for a thermal cracker that produces olefins. The possible feeds include ethane, propane, debutanized natural gasoline (DNG), and gas oil, some of which may be fed simultaneously. Based on plant data, eight products are produced in varying proportions according to the following matrix. The capacity to run gas feeds through the cracker is 200,000 lb/stream hour (total flow based on an average mixture). Ethane uses the equivalent of 1.1 lb of capacity per pound of ethane; propane 0.9 lb; gas oil 0.9 lb/lb; and DNG 1.0.

**Table E14.1A Yield structure: (wt. fraction)** 

| Product   | Ethane | Propane | Gas oil | DNG  |
| :-------- | :----- | :------ | :------ | :--- |
| Methane   | 0.07   | 0.25    | 0.10    | 0.15 |
| Ethane    | 0.40   | 0.06    | 0.04    | 0.05 |
| Ethylene  | 0.50   | 0.35    | 0.20    | 0.25 |
| Propane   |        | 0.10    | 0.01    | 0.01 |
| Propylene | 0.01   | 0.15    | 0.15    | 0.18 |
| Butadiene | 0.01   | 0.02    | 0.04    | 0.05 |
| Gasoline  | 0.01   | 0.07    | 0.25    | 0.30 |
| Fuel oil  |        |         | 0.21    | 0.01 |

Downstream processing limits exist of 50,000 lb/stream hour on the ethylene and 20,000 lb/stream hour on the propylene. The fuel requirements to run the cracking system for each feedstock type are as follows:

* Ethane: 8364 Btu/lb
* Propane: 5016 Btu/lb
* Gas oil: 3900 Btu/lb
* DNG: 4553 Btu/lb

Methane and fuel oil produced by the cracker are recycled as fuel. All the ethane and propane produced is recycled as feed. Heating values are as follows:

* Natural gas: 21,520 Btu/lb
* Methane: 21,520 Btu/lb
* Fuel oil: 18,000 Btu/lb

Because of heat losses and the energy requirements for pyrolysis, the fixed fuel requirement is $20.0\times10^{6}$ Btu/stream hour. The price structure on the feeds and products and fuel costs is:

* Feeds ($/lb$): Ethane 6.55, Propane 9.73, Gas oil 12.50, DNG 10.14.
* Products ($/lb$): Methane 5.38 (fuel value), Ethylene 17.75, Propylene 13.79, Butadiene 26.64, Gasoline 9.93, Fuel oil 4.50 (fuel value).

Assume an energy (fuel) cost of $\$2.50/10^{6}$ Btu. Assumptions used in formulating the objective function and constraints are:

1.  $20\times10^{6}Btu/h$ fixed fuel requirement (methane) to compensate for the heat loss.
2.  All propane and ethane are recycled with the feed, and all methane and fuel oil are recycled as fuel.

Build a linear programming model to maximize the profit of a thermal cracker operation.

## 2.2 Ground Truth Model

We define the following variables for the flow rates to and from the furnace (in lb/h):

* $x_{1} =$ fresh ethane feed 
* $x_{2} =$ fresh propane feed 
* $x_{3} =$ gas oil feed 
* $x_{4} =$ DNG feed 
* $x_{5} =$ ethane recycle 
* $x_{6} =$ propane recycle 
* $x_{7} =$ fuel added 

**Objective function (profit):**
In words, the profit $f$ is:
$$f = \text{Product value} - \text{Feed cost} - \text{Energy cost}$$

Combining product sales, feed costs, and energy costs, the objective function ($\phi/h$) is:
$$f = 2.84x_{1} - 0.22x_{2} - 3.33x_{3} + 1.09x_{4} + 9.39x_{5} + 9.51x_{6}$$
(Note: The fixed heat loss represents a constant cost that is independent of the variables $x_{i}$, hence in optimization we can ignore this factor.)

**Constraints:**

1.  Cracker capacity of 200,000 lb/h:
    $$1.1(x_{1}+x_{5}) + 0.9(x_{2}+x_{6}) + 0.9x_{3} + 1.0x_{4} \le 200,000$$
    or:
    $$1.1x_{1} + 0.9x_{2} + 0.9x_{3} + 1.0x_{4} + 1.1x_{5} + 0.9x_{6} \le 200,000$$

2.  Ethylene processing limitation of 100,000 lb/h:
    $$0.5x_{1} + 0.35x_{2} + 0.20x_{3} + 0.25x_{4} + 0.5x_{5} + 0.35x_{6} \le 100,000$$
    *(Note: Coefficient for $x_3$ corrected from text based on Table E14.1A)*

3.  Propylene processing limitation of 20,000 lb/h:
    $$0.01x_{1} + 0.15x_{2} + 0.15x_{3} + 0.18x_{4} + 0.01x_{5} + 0.15x_{6} \le 20,000$$

4.  Ethane recycle:
    $$x_{5} = 0.4x_{1} + 0.4x_{5} + 0.06x_{2} + 0.06x_{6} + 0.04x_{3} + 0.05x_{4}$$
    Rearranging:
    $$0.4x_{1} + 0.06x_{2} + 0.04x_{3} + 0.05x_{4} - 0.6x_{5} + 0.06x_{6} = 0$$

5.  Propane recycle:
    $$x_{6} = 0.1x_{2} + 0.1x_{6} + 0.01x_{3} + 0.01x_{4}$$
    Rearranging:
    $$0.1x_{2} + 0.01x_{3} + 0.01x_{4} - 0.9x_{6} = 0$$

6.  Heat constraint:
    The sum of the fuel required plus 20,000,000 Btu/h is equal to the Total Fuel Heating Value (THV):
    $$-6857.6x_{1} + 364x_{2} + 2032x_{3} - 1145x_{4} - 6857.6x_{5} + 364x_{6} + 21,520x_{7} = 20,000,000$$

<div style="page-break-after: always;"></div>


# Section 3. Questions

## Question 1: Objective Function Comparison

### Model A's Objective Function



### Profit Maximization
$$\max Z = \sum_{j=1}^6 p_j^+ y_j - \sum_{i=1}^4 p_i^- x_i - p_f \cdot \max\left(0, \frac{\sum_{i=1}^4 f_i x_i - (h_1 y_1 + h_2 y_6)}{10^6}\right)$$

**Note**: Maximize total profit from product sales minus feedstock costs minus external fuel cost

### Model B's Objective Function


$$
\max \quad \text{Profit} = \left( \sum_{j \in \{Ey, Py, B, Gs\}} P_j S_j \right) - \left( \sum_{i \in I} f_i C_i \right) - \left( F_{ext} H_{ext} \frac{C_{ext}}{10^6} \right)
$$
*(Note: This objective function aims to maximize the total hourly profit. It is calculated as the total revenue from selling products (Ethylene, Propylene, Butadiene, Gasoline), minus the total cost of purchasing fresh feedstocks, minus the cost of supplementary external fuel.)*

### Model C's Objective Function




$$
\max \quad \sum_{i \in \text{Products}} p_i^{\text{out}} \cdot Q_i - \sum_{j \in \text{Feeds}} p_j^{\text{in}} \cdot x_j - c_{\text{fuel}} \cdot \left( F_{\text{req}} - F_{\text{rec}} \right)
$$

**Note**: Maximize net profit, which is total revenue from saleable products minus cost of fresh feedstocks minus net fuel cost (fixed requirement minus fuel generated from methane and fuel oil).

---

**Question:** Which model has the best objective function?  
Best Model: ___ (Options: Objective Function A, Objective Function B, Objective Function C)

<div style="page-break-after: always;"></div>

## Question 2: Decision Variable Comparison

### Model A's Decision Variable



### Feedstock Flow Rates ($x_i$)
- **Symbol**: $x_1, x_2, x_3, x_4$
- **Description**: Flow rates (lb/stream hour) of feedstocks: $x_1$ = Ethane, $x_2$ = Propane, $x_3$ = Gas oil, $x_4$ = DNG

### Product Flow Rates ($y_j$)
- **Symbol**: $y_1, y_2, y_3, y_4, y_5, y_6$
- **Description**: Flow rates (lb/stream hour) of products: $y_1$ = Methane, $y_2$ = Ethylene, $y_3$ = Propylene, $y_4$ = Butadiene, $y_5$ = Gasoline, $y_6$ = Fuel oil

### Model B's Decision Variable


Let the set of feeds be $I = \{E, P, D, G\}$ representing Ethane, Propane, DNG, and Gas oil, respectively.
Let the set of products be $J = \{M, E_p, Ey, P_p, Py, B, Gs, Fo\}$ representing Methane, Ethane, Ethylene, Propane, Propylene, Butadiene, Gasoline, and Fuel oil, respectively.

| Symbol | Meaning | Unit | Corresponding Data Point |
|---|---|---|---|
| **Decision Variables** | | | |
| $f_i$ | Fresh feed rate of feedstock $i \in I$ | lb/hr | N/A |
| $F_{ext}$ | Amount of external natural gas purchased for fuel | lb/hr | N/A |
| **Auxiliary Variables** | | | |
| $F_i$ | Total feed rate of feedstock $i \in I$ to the cracker (fresh + recycle) | lb/hr | N/A |
| $P_j$ | Production rate of product $j \in J$ | lb/hr | N/A |
| **Parameters** | | | |
| $C_i$ | Purchase cost of fresh feedstock $i \in I$ | $/lb | Feed Costs |
| $S_j$ | Sale price of saleable product $j \in \{Ey, Py, B, Gs\}$ | $/lb | Product Sale Prices |
| $C_{ext}$ | Cost of external fuel | $/10^6$ Btu | External Fuel Cost |
| $Y_{ij}$ | Yield of product $j$ from feed $i$ | wt. fraction | Yield Matrix |
| $W_i$ | Capacity weighting factor for feed $i \in I$ | dimensionless | Capacity Weighting Factors |
| $E_i$ | Specific fuel energy requirement for feed $i \in I$ | Btu/lb | Feed-Specific Fuel Requirements |
| $H_j$ | Heating value of fuel source $j \in \{M, Fo\}$ | Btu/lb | Fuel Heating Values |
| $H_{ext}$ | Heating value of external natural gas | Btu/lb | Fuel Heating Values |
| $Q_{fixed}$ | Fixed fuel energy requirement | Btu/hr | Fixed Fuel Requirement |
| $CAP_{cracker}$ | Maximum weighted cracker capacity | lb/hr | Cracker Capacity |
| $CAP_{Ey}$ | Maximum production capacity for Ethylene | lb/hr | Production Limits |
| $CAP_{Py}$ | Maximum production capacity for Propylene | lb/hr | Production Limits |

### Model C's Decision Variable



### Feed Rate of Ethane ($x_E$)
- **Symbol**: $x_E$
- **Description**: The mass flow rate (in lb/stream hour) of ethane fed into the thermal cracker as fresh feed. This is a decision variable because the amount of ethane to be introduced (beyond what is recycled) must be optimized to maximize profit.

### Feed Rate of Propane ($x_P$)
- **Symbol**: $x_P$
- **Description**: The mass flow rate (in lb/stream hour) of propane fed into the thermal cracker as fresh feed. This is a decision variable to be optimized, as propane can be cracked to produce valuable products but also consumes energy and has a cost.

### Feed Rate of Gas Oil ($x_G$)
- **Symbol**: $x_G$
- **Description**: The mass flow rate (in lb/stream hour) of gas oil fed into the thermal cracker as fresh feed. Gas oil yields significant gasoline and butadiene but requires capacity and has a high cost.

### Feed Rate of DNG ($x_D$)
- **Symbol**: $x_D$
- **Description**: The mass flow rate (in lb/stream hour) of debutanized natural gasoline (DNG) fed into the thermal cracker as fresh feed. DNG is a feedstock with moderate yield of valuable products and a specific capacity consumption.

---

**Question:** Which model has the most complete decision variables?  
Best Model: ___ (Options: Decision Variable A, Decision Variable B, Decision Variable C)

<div style="page-break-after: always;"></div>

## Question 3: Constraint Comparison

### Model A's Constraints



#### Equality Constraints
1. $$y_1 = \sum_{i=1}^4 a_{i1} x_i$$
   - **Note**: Methane production from all feedstocks

2. $$y_2 = \sum_{i=1}^4 a_{i3} x_i$$
   - **Note**: Ethylene production from all feedstocks

3. $$y_3 = \sum_{i=1}^4 a_{i5} x_i$$
   - **Note**: Propylene production from all feedstocks

4. $$y_4 = \sum_{i=1}^4 a_{i6} x_i$$
   - **Note**: Butadiene production from all feedstocks

5. $$y_5 = \sum_{i=1}^4 a_{i7} x_i$$
   - **Note**: Gasoline production from all feedstocks

6. $$y_6 = \sum_{i=1}^4 a_{i8} x_i$$
   - **Note**: Fuel oil production from all feedstocks

#### Inequality Constraints
1. $$\sum_{i=1}^4 c_i x_i \leq C_{max}$$
   - **Note**: Total capacity constraint

2. $$y_2 \leq E_{max}$$
   - **Note**: Ethylene processing limit

3. $$y_3 \leq P_{max}$$
   - **Note**: Propylene processing limit

4. $$h_1 y_1 + h_2 y_6 + F_{purchased} \geq \sum_{i=1}^4 f_i x_i + F_{fixed}$$
   - **Note**: Fuel energy balance (internal + purchased ≥ required)

#### Boundary Conditions
- $$x_i \geq 0 \quad \forall i = 1,2,3,4$$
- $$y_j \geq 0 \quad \forall j = 1,2,3,4,5,6$$
- $$F_{purchased} \geq 0$$

### Model B's Constraints


#### Physical Constraints
$$
\sum_{i \in I} F_i W_i \leq CAP_{cracker}
$$
*(Note: This constraint ensures that the total weighted feed rate into the cracker does not exceed its maximum design capacity of 200,000 lb/stream hour.)*

#### Operational Constraints
$$
P_{Ey} \leq CAP_{Ey}
$$
*(Note: This constraint ensures that the total production of Ethylene does not exceed the downstream processing limit of 50,000 lb/hr.)*

$$
P_{Py} \leq CAP_{Py}
$$
*(Note: This constraint ensures that the total production of Propylene does not exceed the downstream processing limit of 20,000 lb/hr.)*

#### Logical and Balance Constraints
**Material Balance: Product Production**
$$
P_j = \sum_{i \in I} F_i Y_{ij}, \quad \forall j \in J
$$
*(Note: This set of equations defines the production rate of each product $j$ as the sum of yields from all total feeds $F_i$ entering the cracker.)*

**Material Balance: Recycle Loops**
$$
F_E = f_E + P_{E_p}
$$
*(Note: This constraint defines the total Ethane feed to the cracker ($F_E$) as the sum of fresh Ethane feed ($f_E$) and the recycled Ethane produced ($P_{E_p}$), enforcing the 100% recycle rule.)*
$$
F_P = f_P + P_{P_p}
$$
*(Note: This constraint defines the total Propane feed to the cracker ($F_P$) as the sum of fresh Propane feed ($f_P$) and the recycled Propane produced ($P_{P_p}$), enforcing the 100% recycle rule.)*
$$
F_D = f_D \quad \text{and} \quad F_G = f_G
$$
*(Note: These constraints define the total feed for DNG and Gas oil as equal to their fresh feed rates, as they are not recycled.)*

**Energy Balance**
$$
\underbrace{Q_{fixed} + \sum_{i \in I} F_i E_i}_{\text{Total Heat Required}} = \underbrace{P_M H_M + P_{Fo} H_{Fo} + F_{ext} H_{ext}}_{\text{Total Heat Supplied}}
$$
*(Note: This constraint ensures that the total energy demand (fixed requirement plus variable requirement based on feed rates) is exactly met by the energy supplied from internal by-products (Methane and Fuel oil) and purchased external fuel.)*

**Non-negativity Constraints**
$$
f_i \geq 0, \quad \forall i \in I
$$
$$
F_{ext} \geq 0
$$
*(Note: These constraints ensure that the flow rates of all fresh feeds and purchased external fuel cannot be negative.)*

### Model C's Constraints



#### Equality Constraints

1. $$Q_{\text{meth}} = 0.07x_E + 0.25x_P + 0.10x_G + 0.15x_D$$
   - **Note**: Total methane production equals sum of methane yields from all feeds.

2. $$Q_{\text{eth}} = 0.40x_E + 0.06x_P + 0.04x_G + 0.05x_D$$
   - **Note**: Total ethane production equals sum of ethane yields from all feeds. All ethane is recycled, so it does not contribute to net output.

3. $$Q_{\text{ethy}} = 0.50x_E + 0.35x_P + 0.20x_G + 0.25x_D$$
   - **Note**: Total ethylene production equals sum of ethylene yields from all feeds.

4. $$Q_{\text{prop}} = 0.06x_P + 0.01x_G + 0.01x_D$$
   - **Note**: Total propane production equals sum of propane yields from all feeds. All propane is recycled.

5. $$Q_{\text{propy}} = 0.01x_E + 0.15x_P + 0.15x_G + 0.18x_D$$
   - **Note**: Total propylene production equals sum of propylene yields from all feeds.

6. $$Q_{\text{but}} = 0.01x_E + 0.02x_P + 0.04x_G + 0.05x_D$$
   - **Note**: Total butadiene production equals sum of butadiene yields from all feeds.

7. $$Q_{\text{gas}} = 0.01x_E + 0.07x_P + 0.25x_G + 0.30x_D$$
   - **Note**: Total gasoline production equals sum of gasoline yields from all feeds.

8. $$Q_{\text{fo}} = 0.21x_G + 0.01x_D$$
   - **Note**: Total fuel oil production equals sum of fuel oil yields from gas oil and DNG.

9. $$F_{\text{rec}} = HV_{\text{meth}} \cdot Q_{\text{meth}} + HV_{\text{fo}} \cdot Q_{\text{fo}}$$
   - **Note**: Total recycled fuel energy is the sum of energy from methane and fuel oil produced.

10. $$F_{\text{req}} = f_E x_E + f_P x_P + f_G x_G + f_D x_D + F_{\text{fix}}$$
    - **Note**: Total fuel energy required is the sum of energy needed to crack each feed plus the fixed heat loss requirement.

#### Inequality Constraints

1. $$c_E x_E + c_P x_P + c_G x_G + c_D x_D \leq C_{\text{max}}$$
   - **Note**: Total capacity consumption (weighted by feed-specific factors) must not exceed the cracker’s maximum capacity of 200,000 lb/stream hour.

2. $$Q_{\text{ethy}} \leq L_{\text{eth}}$$
   - **Note**: Ethylene production must not exceed downstream processing limit of 50,000 lb/stream hour.

3. $$Q_{\text{propy}} \leq L_{\text{prop}}$$
   - **Note**: Propylene production must not exceed downstream processing limit of 20,000 lb/stream hour.

4. $$F_{\text{req}} \geq F_{\text{rec}}$$
   - **Note**: Fuel energy required must be at least equal to fuel energy recovered (non-negativity of net fuel cost). This ensures the model does not generate excess fuel to artificially reduce cost.

#### Boundary Conditions

- $$x_E \geq 0$$
- $$x_P \geq 0$$
- $$x_G \geq 0$$
- $$x_D \geq 0$$
  - **Note**: All fresh feed rates must be non-negative.

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
4Reactor1