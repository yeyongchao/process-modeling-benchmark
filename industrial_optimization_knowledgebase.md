# Industrial and Brewing Process Optimization Knowledge Base (v3: Symptom–Objective–Algorithm Pathways)

This knowledge base supports industrial process optimization, RAG retrieval, and knowledge-based agents. It contains 55 independently retrievable entries: 45 general L1 problem pathways and 10 brewing-specific L2 examples. Each second-level heading and its body form a self-contained chunk.

A *symptom* is an observable or reportable phenomenon, not a confirmed root cause. The *optimization objective* states the metric to improve and the conditions that must be preserved. *Candidate algorithms* are conditional method choices rather than a claim that one method is always correct. Site safety, food hygiene, equipment protection, product-release requirements, and regulatory boundaries must come from valid documents for the specific facility. Temperature, time, pressure, concentration, flow, and quality thresholds in this knowledge base must not be used directly as plant setpoints.

---

## [L1-001] A Single-Variable Control Loop Oscillates Continuously or Overshoots Frequently

**Level and keywords**: L1 general pathway; oscillation; overshoot; PID; valve movement; loop tuning

**Scenario and symptoms**: Temperature, pressure, flow, or level oscillates around its setpoint, recovers slowly after disturbances, drives the actuator back and forth, or performs worse in automatic mode than under manual operation.

**Optimization objective**: Reduce deviation, overshoot, settling time, and unnecessary actuator movement without violating equipment or quality limits.

**Candidate algorithms**: First check measurement quality, actuator direction, deadband, and integral windup, then retune the PID controller. Add feedforward when the dominant disturbance is measurable. Use cascade control when the process contains clearly separated fast and slow dynamics.

**Required information**: Setpoint, measurement, controller output, automatic/manual state, sampling period, alarms, valve-position feedback, disturbance records, and step responses at different operating conditions.

**Mathematical and constraint patterns**: Dynamic state transition, pure time delay, input bounds, rate-of-change limits, measurement noise, and actuator saturation.

**Applicability limits**: Strong coupling, long delays, or several simultaneously active hard constraints usually cannot be solved by increasing PID gain alone. Safety interlocks must not be treated as ordinary tuning objectives.

---

## [L1-002] Measurable Disturbances Repeatedly Drive the Process Away from Target

**Level and keywords**: L1 general pathway; disturbance rejection; feedforward; cascade; load change

**Scenario and symptoms**: Changes in feed properties, upstream flow, steam pressure, coolant temperature, or ambient conditions repeatedly move the process output in the same direction, while feedback control responds only after the deviation occurs.

**Optimization objective**: Compensate before the disturbance materially affects key quality or state variables, reducing peak deviation and recovery time.

**Candidate algorithms**: Combine feedforward with feedback when the disturbance is measured promptly and reliably. Use cascade control when a fast intermediate variable is available. Consider MPC when several disturbances and manipulated variables interact under constraints.

**Required information**: Disturbance measurements, manipulated variables, outputs, gain directions, delays, historical events, sensor latency, and actuator capacity.

**Mathematical and constraint patterns**: Disturbance-to-output dynamics, pure delays, feedforward gains, input saturation, and rate limits.

**Applicability limits**: Noisy, time-misaligned, or post-event measurements are unsuitable as direct feedforward signals. A feedback loop is still required to correct model error.

---

## [L1-003] Long Process Delays Cause Slow and Unstable Responses

**Level and keywords**: L1 general pathway; long delay; slow process; predictive control; system identification

**Scenario and symptoms**: Quality or state variables respond long after a valve, pump, heating, or dosing adjustment. Operators then stack additional actions, eventually causing overshoot and repeated correction.

**Optimization objective**: Obtain stable, predictable responses while accounting for transport, reaction, heat-transfer, and measurement delays, and reduce excessive intervention.

**Candidate algorithms**: Use system identification to separate pure delay from process inertia. A single loop may use a delay-compensating structure; multivariable constrained problems favor MPC; batch processes may combine state estimation with dynamic optimization.

**Required information**: Timestamped input changes and output responses, sampling period, sensor and communication delays, material residence time, process stages, and data across loads.

**Mathematical and constraint patterns**: Delayed states, difference or state-space models, input-rate limits, prediction horizon, and initial state.

**Applicability limits**: Do not collapse all delays into an empirical average. A delay model may fail when training and deployment use different sampling periods.

---

## [L1-004] Multivariable Coupling Makes Local Controllers Conflict

**Level and keywords**: L1 general pathway; multivariable coupling; MPC; constrained control; decoupling

**Scenario and symptoms**: One manipulated variable affects temperature, pressure, composition, throughput, or energy simultaneously. Independent loops cancel or amplify one another, causing frequent setpoint changes and repeated constraint activation.

**Optimization objective**: Coordinate manipulated variables so primary outputs track their targets with limited control movement while equipment, quality, and safety constraints remain satisfied.

**Candidate algorithms**: Use loop pairing and static decoupling for weak coupling. Apply MPC when coupling, delays, and constraints are all material. Combine robust design and online state estimation when model uncertainty is significant.

**Required information**: Candidate manipulated variables and outputs, interaction directions, dynamic responses, bounds, rate limits, control priorities, and mutually exclusive actions.

**Mathematical and constraint patterns**: MIMO state-space models, tracking error, move penalties, hard and soft constraints, and prioritized objectives.

**Applicability limits**: Coupled models cannot be identified reliably without sufficient input excitation. MPC does not replace interlocks, equipment protection, or regulatory control loops.

---

## [L1-005] The Process Is Stable but Long-Run Operating Cost Is High

**Level and keywords**: L1 general pathway; economic optimization; RTO; economic MPC; operating target

**Scenario and symptoms**: Quality and control indicators are stable, but feedstock, energy, throughput, or by-product costs remain above expectations. Loop setpoints remain fixed despite price, equipment, and demand changes.

**Optimization objective**: Optimize the economic operating point within quality, safety, and equipment constraints and pass executable targets to the control layer.

**Candidate algorithms**: Use data reconciliation, parameter updates, and RTO for near-steady processes with slow update cycles. Use dynamic RTO or economic MPC for frequent transitions. Apply LP/QP to approximately linear structures and NLP otherwise.

**Required information**: Material and energy prices, throughput and quality data, steady-state criteria, model parameters, equipment capacity, implemented values, active constraints, and a profit baseline.

**Mathematical and constraint patterns**: Economic objective, material and energy balances, equipment capacity, steady-state or dynamic models, hierarchical control, and feasibility checks.

**Applicability limits**: When data do not close, models remain uncalibrated, or operating states are not comparable, a calculated economic optimum may not produce realized savings.

---

## [L1-006] Critical Internal States Cannot Be Measured Online

**Level and keywords**: L1 general pathway; state estimation; soft sensor; Kalman; MHE; unmeasured state

**Scenario and symptoms**: Concentration, reaction progress, biomass, contamination load, equipment health, or another critical variable is available only offline or cannot be measured directly, so control and endpoint decisions depend on experience.

**Optimization objective**: Estimate critical states and their uncertainty continuously from measured inputs and outputs to support monitoring, control, and optimization.

**Candidate algorithms**: Use Kalman-family filters for linear or near-linear dynamics; UKF or moving-horizon estimation for nonlinear constrained systems; soft sensors when mechanisms are weak but labeled data are sufficient; and hybrid models where appropriate.

**Required information**: Online sensors, laboratory labels, input actions, sampling and label delays, process model, noise levels, initial conditions, and observability analysis.

**Mathematical and constraint patterns**: State and measurement equations, process and measurement noise, state bounds, parameter estimation, and confidence intervals.

**Applicability limits**: Estimates cannot automatically replace statutory tests or product-release measurements. Predictions outside the training or identification domain require rejection or human review.

---

## [L1-007] Batch-Process Stage Changes Depend on Fixed Time

**Level and keywords**: L1 general pathway; batch process; phase identification; state machine; endpoint detection

**Scenario and symptoms**: Charging, reaction, holding, cooling, discharge, or cleaning stages change after fixed durations. Variations in feed or equipment cause premature completion, idle waiting, or transitions into an incorrect state.

**Optimization objective**: Trigger stage transitions from process state and quality conditions, shortening the cycle while reducing early- or late-transition risk.

**Candidate algorithms**: Represent discrete stages and allowed transitions with a hybrid state machine. Detect stage completion using rates of change, soft sensors, or state estimation. Combine dynamic optimization with stage-duration and trajectory decisions for complex batches.

**Required information**: Stage definitions, entry and exit conditions, online variables, laboratory endpoints, event logs, timeouts, abnormal transitions, and manual takeover records.

**Mathematical and constraint patterns**: Hybrid continuous-discrete state machine, guard conditions, terminal constraints, minimum and maximum stage times, and state resets.

**Applicability limits**: Critical sanitation and safety stages must not end from a single sensor event. Timeout, exception, and manual fallback paths must remain available.

---

## [L1-008] Optimization or Control Problems Are Frequently Infeasible

**Level and keywords**: L1 general pathway; infeasibility; conflicting constraints; soft constraint; hierarchical optimization

**Scenario and symptoms**: The optimizer frequently reports infeasibility, a controller repeatedly hits constraints, or a solution appears only after relaxing quality, throughput, delivery, or equipment limits.

**Optimization objective**: Identify genuinely conflicting conditions, protect non-negotiable limits first, and provide a minimal, explainable feasibility-recovery plan.

**Candidate algorithms**: Analyze constraint conflicts and irreducible infeasible subsets. Convert preferences or planning targets into soft constraints with slack. Use lexicographic or hierarchical optimization and adjust targets or horizons on a rolling basis when necessary.

**Required information**: Constraint sources, units, priorities, activation conditions, historical activity, initial state, prediction error, and the range of adjustments authorized by operators.

**Mathematical and constraint patterns**: Hard constraints, slack variables, positive-part penalties, priority objectives, terminal feasible sets, and safety margins.

**Applicability limits**: Safety, regulatory, and equipment-protection limits must not be softened to obtain feasibility. Any temporary relaxation requires documentation and responsible approval.

---

## [L1-009] Material or Energy Data Do Not Reconcile Over Time

**Level and keywords**: L1 general pathway; data reconciliation; balance residual; gross error; sensor bias

**Scenario and symptoms**: Inflows, outflows, and inventory changes across one process boundary do not close. Yield or energy use changes with the accounting convention, and optimization results are overly sensitive to individual meters.

**Optimization objective**: Produce mutually consistent data with quantified uncertainty and distinguish random measurement error, systematic bias, omitted streams, and real losses.

**Candidate algorithms**: Apply weighted data reconciliation against material and energy balances. Combine gross-error detection to locate bias, leakage, and entry mistakes. Use dynamic reconciliation or an appropriate time window for transient processes.

**Required information**: System boundary, flows and inventories, composition, temperature and pressure, measurement errors, units, timestamps, calibration records, drains, samples, and unmetered streams.

**Mathematical and constraint patterns**: Total and component balances, energy balance, measurement equations, weighted residuals, observability, and redundancy.

**Applicability limits**: An incorrect system boundary assigns real loss to instruments. A nonredundant single measurement cannot be diagnosed for bias by reconciliation alone.

---

## [L1-010] Sudden Abnormal Events Are Not Detected Promptly

**Level and keywords**: L1 general pathway; anomaly detection; control chart; residual monitoring; abrupt change

**Scenario and symptoms**: Leaks, blockages, stuck valves, abnormal raw materials, or operating errors are noticed only through final quality tests or obvious alarms because early signals are not used effectively.

**Optimization objective**: Detect significant deviation before its impact grows and link the alert to the relevant equipment, batch, stage, and plausible causes.

**Candidate algorithms**: Use Shewhart charts or thresholds with persistence logic for clear univariate changes. Monitor prediction residuals when a process model exists. Use change-point or isolated-anomaly detection for complex signals and combine it with rule-based alarms.

**Required information**: Stable-operation baseline, alarms and event labels, process stages, product types, sensor-quality flags, maintenance records, and operating records.

**Mathematical and constraint patterns**: Stratified baselines, residuals, control limits, persistence logic, measurement noise, and event windows.

**Applicability limits**: A statistical anomaly is not a root cause. Grade changes and recipe changes must be separated from actual abnormalities, and alarm handling must include human confirmation and suppression rules.

---

## [L1-011] Small Persistent Drift Remains Undetected

**Level and keywords**: L1 general pathway; slow drift; EWMA; CUSUM; fouling; sensor drift

**Scenario and symptoms**: Variables remain inside conventional alarm limits while energy use, heat-transfer capability, quality mean, or sensor readings shift over weeks and attract attention only after capacity or quality has fallen materially.

**Optimization objective**: Detect small sustained changes in means or residuals early and estimate their onset and progression rate.

**Candidate algorithms**: Use EWMA or CUSUM for stable univariate signals. Monitor standardized residuals when a predictive model exists. Combine degradation models and maintenance rules for changes related to operating age.

**Required information**: Long stable histories, calibration and maintenance records, cleaning cycles, load, ambient conditions, product changes, and reference samples before and after drift.

**Mathematical and constraint patterns**: Recursive statistics, trend terms, change points, stratified control limits, and drift rate.

**Applicability limits**: Seasonality, product mix, and new operating policies alter the baseline. Before recalculating limits, determine whether the change reflects improvement, normal switching, or failure.

---

## [L1-012] Correlated Variables Become Abnormal Together While Each Remains Within Limits

**Level and keywords**: L1 general pathway; multivariate anomaly; PCA; latent variable; covariance

**Scenario and symptoms**: Temperature, flow, pressure, and composition each remain permissible, but their normal relationship breaks down and later appears as a quality or energy problem.

**Optimization objective**: Detect abnormalities in variable relationships and locate the measurements and process areas contributing most strongly.

**Candidate algorithms**: Use PCA, PLS latent-variable statistics, or multivariate control charts for stable continuous operation. Monitor residuals from state-space or sequence models for dynamic processes, then combine them with process topology for localization.

**Required information**: Synchronized multivariable history, product and stage labels, stable operating envelope, missing values, sensor locations, and known abnormal events.

**Mathematical and constraint patterns**: Covariance structure, latent scores, residual statistics, time alignment, and contribution analysis.

**Applicability limits**: Scaling, collinearity, and mixed operating modes materially affect results. Contribution ranking is a diagnostic lead, not automatic proof of root cause.

---

## [L1-013] Delayed Laboratory Quality Measurements Delay Decisions

**Level and keywords**: L1 general pathway; soft sensor; quality prediction; delayed label; virtual analyzer

**Scenario and symptoms**: Critical quality metrics are available only every few hours or after a batch, so process adjustments, endpoint decisions, and abnormal-event responses lag the actual change.

**Optimization objective**: Estimate current quality and confidence from online process data to support early warning, control constraints, and sampling decisions.

**Candidate algorithms**: Use PLS or tree models when data volume is moderate and variables are correlated. Use sequence models or state estimation for strongly dynamic or nonlinear behavior. Prefer a hybrid soft sensor when reliable mechanisms are known.

**Required information**: Online measurements, laboratory results, sampling and reporting times, batch and stage, calibration, raw-material information, and coverage of the training domain.

**Mathematical and constraint patterns**: Regression with delayed labels, measurement equations, residual correction, confidence intervals, and prediction-rejection conditions.

**Applicability limits**: Validation sets must be split by time and batch to prevent leakage. Soft sensors cannot replace regulatory, sanitation, or final product-release tests.

---

## [L1-014] Quality or Equipment Problems Recur Without a Clear Root Cause

**Level and keywords**: L1 general pathway; root-cause diagnosis; causal graph; fault propagation; Bayesian network

**Scenario and symptoms**: Quality deviation, downtime, or abnormal energy use appears across many variables. Correlation analysis suggests many candidates but cannot distinguish root causes, propagation variables, and outcomes.

**Optimization objective**: Produce reviewable candidate causal chains and validation actions, narrowing the investigation and estimating the impact of corrective measures.

**Candidate algorithms**: Combine process topology, temporal order, and expert rules into a candidate causal graph, then compare explanations with statistical tests, Bayesian networks, or controlled interventions. Dynamic residuals can support fault-propagation analysis.

**Required information**: Equipment connectivity, variable timestamps, operating and maintenance events, batch genealogy, known failures, intervention records, and possible hidden common causes.

**Mathematical and constraint patterns**: Directed causal relations, conditional dependence, time lags, before-and-after interventions, and uncertainty ranking.

**Applicability limits**: Observational data rarely determine a unique causal direction. Report alternative explanations and tests instead of asserting one unconfirmed cause.

---

## [L1-015] Fault Examples Are Rare and Classes Are Highly Imbalanced

**Level and keywords**: L1 general pathway; few-shot; anomaly detection; transfer learning; class imbalance

**Scenario and symptoms**: Normal-operation data are abundant but important faults have occurred only a few times. A supervised classifier appears accurate while missing the abnormalities that matter most.

**Optimization objective**: Improve event recall and warning lead time with limited fault labels while controlling the inspection and downtime costs caused by false alarms.

**Candidate algorithms**: Establish a one-class or semi-supervised anomaly baseline. Use transfer learning when similar equipment has labels. When simulation can generate fault trajectories, apply conservative augmentation and select thresholds using cost-sensitive evaluation.

**Required information**: Coverage of normal modes, sparse fault events, equipment and sensor differences, failure consequences, alarm-response cost, maintenance confirmation, and unlabeled history.

**Mathematical and constraint patterns**: Anomaly score, class weights, event-level recall, lead time, confidence, and domain difference.

**Applicability limits**: Synthetic data cannot replace validation on real failures. Equipment and operating differences may cause negative transfer, and safety alarms require independent protective logic.

---

## [L1-016] Multisource Data Are Missing, Asynchronous, or Timestamped Inconsistently

**Level and keywords**: L1 general pathway; missing data; time synchronization; imputation; data quality

**Scenario and symptoms**: Control, laboratory, energy, maintenance, and production records come from different systems with different clocks and sampling rates. Missing and duplicated observations create false input–outcome relationships.

**Optimization objective**: Build a traceable modeling dataset with explicit time semantics and quantify uncertainty introduced by data processing.

**Candidate algorithms**: Align records by events, batches, and material residence time. Use state estimation or imputation with missingness indicators for short gaps, while retaining long gaps as unknown. Use event matching or cross-correlation to help correct clock offsets.

**Required information**: Raw timestamps, time zones and clock sources, sampling periods, batch and stage events, label sampling and result times, reasons for missingness, and quality flags.

**Mathematical and constraint patterns**: Time windows, lag alignment, measurement equations, missingness masks, imputation uncertainty, and batch indices.

**Applicability limits**: Do not fill the past with future data and then evaluate an online model. Structural missingness and instrument failure must not be hidden by smooth interpolation.

---

## [L1-017] A Deployed Model Gradually Loses Accuracy

**Level and keywords**: L1 general pathway; concept drift; model maintenance; continual learning; version rollback

**Scenario and symptoms**: A soft sensor, predictor, or optimizer works initially but develops persistent bias as equipment wears or fouls and raw materials, products, or operating policies change.

**Optimization objective**: Detect changes in the model's applicability domain, update it without destroying performance on previous modes, and preserve rollback capability.

**Candidate algorithms**: Monitor input distributions, prediction residuals, and calibration error. After confirming drift, use scheduled retraining, recursive estimation, or continual learning. Use hierarchical, mixture-of-experts, or mode-switching models for multiple regimes.

**Required information**: Model version, training coverage, online inputs, ground-truth labels, residuals, equipment and recipe changes, maintenance events, and regression tests covering old and new versions.

**Mathematical and constraint patterns**: Distribution distance, residual trends, recursive parameters, applicability-domain classification, champion–challenger comparison, and version control.

**Applicability limits**: The newest data may not represent all future modes. Automatic updates require approval, old-mode tests, and rapid rollback; an unvalidated model must not overwrite a validated one.

---

## [L1-018] Continuous Resource Allocation Relies on Fixed Manual Ratios

**Level and keywords**: L1 general pathway; resource allocation; linear programming; quota; cost optimization

**Scenario and symptoms**: Raw materials, energy sources, equipment capacity, or logistics sources are allocated by fixed ratios. When prices, inventory, or demand change, manual practice causes excess cost or idle capacity.

**Optimization objective**: Minimize total cost, loss, or resource use while meeting demand, quality, and capacity conditions.

**Candidate algorithms**: Use LP when relationships are continuous and linear. Use QP for quadratic penalties or smoothing. Apply piecewise linearization or NLP when quality or efficiency relations are materially nonlinear.

**Required information**: Source costs, inventory, capacity, composition or efficiency, demand, contracts, quality limits, and the permitted decision time step.

**Mathematical and constraint patterns**: Material and energy balances, capacity bounds, demand satisfaction, continuous allocation, piecewise cost, and shadow prices.

**Applicability limits**: Linear relationships must hold over the decision range. Omitting fixed charges, minimum purchase quantities, or logistics constraints produces an unexecutable plan.

---

## [L1-019] Startup, Selection, and Allocation Decisions Depend on One Another

**Level and keywords**: L1 general pathway; MILP; binary variable; startup and shutdown; equipment allocation

**Scenario and symptoms**: Production requires selecting equipment, starting units, choosing batches, or routing material. Continuous flow optimization cannot express whether an event occurs or its fixed time and cost.

**Optimization objective**: Jointly choose discrete actions and continuous operating amounts while satisfying logic, capacity, and demand and reducing cost and changeover loss.

**Candidate algorithms**: Use MILP for startup, task allocation, batch selection, and logic. Use piecewise approximation or MINLP for nonlinear relations. For large instances, apply rolling horizons, decomposition, or structure-aware heuristics.

**Required information**: Candidate equipment and routes, fixed and variable cost, minimum run time, startup and shutdown time, capacity, compatibility, demand, and initial state.

**Mathematical and constraint patterns**: Binary events, indicator constraints, tight Big-M values, minimum up/down time, capacity activation, and logical truth tables.

**Applicability limits**: Excessive Big-M values weaken the formulation and conceal errors. Every binary variable must remain consistent with physical material flow and equipment state.

---

## [L1-020] Key Performance Metrics Depend Nonlinearly on Operating Variables

**Level and keywords**: L1 general pathway; nonlinear optimization; NLP; dynamics; local optimum

**Scenario and symptoms**: Yield, selectivity, pressure drop, heat transfer, power, or quality exhibits saturation, exponential, multiplicative, or peaked behavior, and the linear-model optimum performs poorly in operation.

**Optimization objective**: Find improved conditions inside a trusted nonlinear operating domain while assessing local optima and parameter uncertainty.

**Candidate algorithms**: Use NLP for continuously differentiable relations and dynamic optimization when states evolve over time. Sequential quadratic programming can solve local models; strong nonconvexity may require multistart, global search, or validated piecewise approximations.

**Required information**: Variable ranges, nonlinear mechanism or fitted data, parameters and initial values, scaling, constraints, historical operating points, and data for validating candidate solutions.

**Mathematical and constraint patterns**: Nonlinear equations, variable scaling, physical bounds, multistart, sensitivity, and constraint qualifications.

**Applicability limits**: Solver success does not establish a global optimum or plant feasibility. Unidentifiable parameters and extrapolation can create false precision.

---

## [L1-021] Batch Temperature, Dosing, or Flow Trajectories Follow a Fixed Recipe

**Level and keywords**: L1 general pathway; dynamic optimization; optimal control; trajectory optimization; batch process

**Scenario and symptoms**: The same temperature, feed, aeration, or flow trajectory is used despite changes in raw materials, load, or equipment condition, causing variation in cycle time, energy, or endpoint quality.

**Optimization objective**: Optimize an executable trajectory under path and terminal constraints, balancing batch time, resource use, and quality risk.

**Candidate algorithms**: Use direct collocation, multiple shooting, or another dynamic-optimization method when a credible dynamic model exists. Combine state estimation and MPC for online correction. When the model is inadequate, begin with system identification or hybrid modeling.

**Required information**: Time series of states and manipulated variables, stage events, dynamic parameters, equipment capacity, input-rate limits, endpoint quality, and the current batch initial state.

**Mathematical and constraint patterns**: Differential or difference states, path and terminal constraints, cumulative consumption, stage transitions, and smooth control trajectories.

**Applicability limits**: Validate optimized trajectories through simulation, replay, and controlled trials. Local sensor temperature and instantaneous actions that equipment cannot deliver must not be treated as real operations.

---

## [L1-022] Many Factors Matter but Their Main Effects and Interactions Are Unclear

**Level and keywords**: L1 general pathway; DOE; factor screening; response surface; interaction

**Scenario and symptoms**: Quality, throughput, or cycle time may depend on raw material, temperature, time, pH, flow, and equipment state. One-factor-at-a-time testing is slow and cannot reveal interactions.

**Optimization objective**: Use as few safe experiments as possible to screen important factors, estimate interactions and local curvature, and identify a promising region for confirmation.

**Candidate algorithms**: Use a screening design first, followed by response-surface or mixture design for local optimization. Block on difficult-to-control batch and equipment differences, and finish with an independent confirmation experiment.

**Required information**: Responses, controllable and noise factors, allowed ranges, experimental units, batch and equipment blocks, replicates, and center points.

**Mathematical and constraint patterns**: Main effects, interactions, quadratic response surfaces, randomization, blocking, lack-of-fit tests, and confidence intervals.

**Applicability limits**: A response surface is generally local to the experimental region. Production trials must respect safety and quality limits and control confounding by time, raw material, and equipment.

---

## [L1-023] Each Experiment Is Expensive and the Best Parameters Are Unknown

**Level and keywords**: L1 general pathway; Bayesian optimization; active experimentation; expensive black box; safe exploration

**Scenario and symptoms**: Each production trial or high-fidelity simulation is costly or slow, the objective lacks a reliable gradient, and grid search requires too many evaluations.

**Optimization objective**: Find better parameters within a limited trial budget while quantifying uncertainty and safety risk in unexplored regions.

**Candidate algorithms**: Use a Gaussian process or another probabilistic surrogate with an acquisition function to select the next trial. Use batch Bayesian optimization for parallel equipment. Model quality and safety conditions as separate constraints.

**Required information**: Decision variables and approved safe domain, historical experiments, objective and constraint observations, measurement noise, batch size, waiting time, and stopping rule.

**Mathematical and constraint patterns**: Probabilistic surrogate, predictive mean and variance, acquisition function, probability of feasibility, trust region, and confirmation points.

**Applicability limits**: Plant exploration must remain inside approved bounds. High dimension, strong drift, and many categorical variables weaken standard methods, and any candidate optimum requires confirmation.

---

## [L1-024] The Optimization Problem Is Discrete, Nonconvex, or Nondifferentiable

**Level and keywords**: L1 general pathway; genetic algorithm; particle swarm; simulated annealing; global search

**Scenario and symptoms**: The objective is produced by complex simulation, rules, lookup tables, or combinatorial selection. Gradients are unavailable, and local methods produce different answers from different starting points.

**Optimization objective**: Explore multiple high-quality feasible solutions within a computation budget and assess result stability instead of reporting only one best run.

**Candidate algorithms**: Consider genetic algorithms, particle swarm, simulated annealing, or multiobjective evolutionary methods. Establish random-search and local-optimization baselines. Handle infeasible candidates with feasible encodings, repair, or strict constraint checks.

**Required information**: Variable encoding, allowed ranges, constraint checker, objective interface, random seeds, computation budget, stopping rule, and a known feasible baseline.

**Mathematical and constraint patterns**: Population search, penalty functions, feasibility repair, independent repeated runs, optimality-gap proxies, and Pareto sets.

**Applicability limits**: Metaheuristics generally cannot prove global optimality. Do not present one random run as a certain optimum, and enforce safety constraints through the feasibility mechanism.

---

## [L1-025] Quality, Throughput, Cost, and Cycle-Time Objectives Conflict

**Level and keywords**: L1 general pathway; multiobjective optimization; Pareto; tradeoff; priority

**Scenario and symptoms**: Higher throughput raises energy use or quality risk, shorter cycles increase equipment load, and one weighted score hides the real exchanges among alternatives.

**Optimization objective**: Identify objectives that cannot improve simultaneously and present interpretable Pareto alternatives with marginal benefits and costs.

**Candidate algorithms**: After defining non-negotiable constraints, use normalized weighted sums, lexicographic optimization, epsilon constraints, or multiobjective evolutionary algorithms. Return representative alternatives rather than only one aggregate score.

**Required information**: KPI baselines, units, target ranges, business priorities, non-negotiable limits, and acceptable tradeoff ranges.

**Mathematical and constraint patterns**: Multiple objectives, normalization, Pareto dominance, epsilon constraints, hierarchical priorities, and marginal value.

**Applicability limits**: Weights require a business interpretation and sensitivity analysis. Safety and regulatory requirements belong in constraints, not economic tradeoffs.

---

## [L1-026] Raw Materials, Demand, or Model Parameters Are Materially Uncertain

**Level and keywords**: L1 general pathway; robust optimization; stochastic programming; scenario; chance constraint

**Scenario and symptoms**: Plans based on average feedstock, average demand, or one parameter set frequently violate capacity, delivery, or quality constraints in operation.

**Optimization objective**: Make a reviewable tradeoff among expected benefit, worst-case performance, service level, and the cost of conservatism.

**Candidate algorithms**: Use robust optimization when credible ranges exist but distributions do not. Use stochastic programming with probabilistic or scenario forecasts. Use chance constraints for nonsafety limits that can tolerate bounded risk, and combine these with a predictive rolling framework.

**Required information**: Historical forecast errors, raw-material distributions, demand scenarios, parameter ranges, extreme events, risk tolerance, and consequences of different failures.

**Mathematical and constraint patterns**: Uncertainty sets, scenario replication, chance constraints, tail risk, probabilistic feasibility, and robust cost.

**Applicability limits**: Overly broad uncertainty sets create excessive conservatism; narrow sets create false confidence. Safety conditions must not rely only on empirical probability.

---

## [L1-027] High-Fidelity Simulation Is Too Slow for Direct Optimization

**Level and keywords**: L1 general pathway; surrogate model; simulation optimization; Kriging; trust region

**Scenario and symptoms**: One mechanistic simulation or digital-twin run is slow enough that parameter search, real-time decision making, or uncertainty analysis becomes computationally impractical.

**Optimization objective**: Approximate the simulator accurately enough inside the decision domain at lower cost and recheck candidate optima with the original model.

**Candidate algorithms**: Generate a space-filling design and fit a response surface, Gaussian process, radial-basis model, or neural surrogate. Use trust regions and active sampling during optimization and validate candidates with the original simulator.

**Required information**: Decision domain, simulator inputs and outputs, failed runs, sampling budget, error tolerance, boundaries, and validation points at extreme conditions.

**Mathematical and constraint patterns**: Surrogate regression, cross-validation, predictive uncertainty, trust region, residual correction, and active sampling.

**Applicability limits**: Interpolation accuracy at training points does not establish boundary or extrapolation reliability. Optimizers can exploit surrogate error, so final candidates require original-model validation.

---

## [L1-028] Sequential Decisions Have Delayed Rewards and Resist Explicit Rules

**Level and keywords**: L1 general pathway; reinforcement learning; sequential decision; offline policy; safe policy

**Scenario and symptoms**: Current actions affect energy, quality, or equipment state much later. Rule combinations are complex, and conventional period-by-period optimization does not yield a practical real-time policy.

**Optimization objective**: Learn an executable policy for long-run cumulative benefit while limiting dangerous actions, out-of-distribution states, and policy uncertainty.

**Candidate algorithms**: Consider reinforcement learning only when states, actions, transitions, and rewards are defined clearly. Prefer training in a validated simulator or offline RL, with action shielding, constrained policies, and a reliable fallback controller.

**Required information**: States and observables, allowed actions, time step, reward definition, historical behavior policy, simulator credibility, safety limits, and abnormal scenarios.

**Mathematical and constraint patterns**: MDP or partially observable model, cumulative return, constrained policy, action bounds, offline coverage, and policy confidence.

**Applicability limits**: Do not use MDP or RL terminology without a defined action space and transition process. Unprotected online trial and error is generally unacceptable in industrial operation.

---

## [L1-029] Purely Mechanistic and Purely Data-Driven Models Both Have Major Gaps

**Level and keywords**: L1 general pathway; hybrid modeling; gray box; residual learning; physics-informed

**Scenario and symptoms**: A mechanistic model is interpretable but biased by unknown parameters or side reactions, while a data model fits history but violates conservation or fails under new conditions.

**Optimization objective**: Build a hybrid model with explicit roles for interpretability, data requirements, accuracy, and extrapolation.

**Candidate algorithms**: Retain conservation laws and known kinetics as the mechanistic backbone and use machine learning for unknown rates, parameters, or residuals. State estimation may update slowly changing parameters online.

**Required information**: Credible mechanisms, unknown relations, process and experimental data, parameter ranges, training coverage, physical-consistency tests, and submodel interfaces.

**Mathematical and constraint patterns**: Conservation equations, state transitions, residual models, parameter estimation, monotonicity or positivity constraints, and joint validation.

**Applicability limits**: Incorrect mechanistic priors constrain the learned component, while the learned component can violate physics. Increased complexity requires explicit model versions and fault-diagnosis responsibility.

---

## [L1-030] Simulation, Real-Time Data, and Optimization Models Are Disconnected

**Level and keywords**: L1 general pathway; digital twin; what-if analysis; online synchronization; model governance

**Scenario and symptoms**: Simulators, equipment models, and live data exist, but models do not reflect the current equipment state and recommendations lack model version, input snapshot, and implementation feedback.

**Optimization objective**: Establish a model–data closed loop for a defined decision purpose, enabling traceable state estimation, scenario analysis, and optimization validation.

**Candidate algorithms**: Build a purpose-specific digital twin combining equipment topology, mechanistic or data models, state synchronization, and simulation optimization. Maintain state through reconciliation and parameter estimation, and record recommendation, execution, and feedback.

**Required information**: Twin object and boundary, data interfaces, synchronization frequency, model version, calibration and validation metrics, decision horizon, execution feedback, and fallback mechanism.

**Mathematical and constraint patterns**: State mapping, model calibration, data assimilation, scenario simulation, versioned parameters, and decision traceability.

**Applicability limits**: A digital twin is not a static visualization and does not become current or trustworthy through naming. Model fidelity must match the risk of the specific decision.

---

## [L1-031] Yield Declines but the Location and Accounting of Loss Are Unclear

**Level and keywords**: L1 general pathway; yield; material loss; balance; batch tracking

**Scenario and symptoms**: The gap between raw-material input and conforming product output grows, but loss is recorded generically and cannot be separated into inventory change, sampling, drainage, rework, evaporation, and measurement error.

**Optimization objective**: Establish a closed, stage-resolved yield baseline and locate economically meaningful losses that can be acted upon.

**Candidate algorithms**: Build equipment- and operation-level total and component balances with data reconciliation. Compare losses through batch genealogy and statistical stratification. After controllable factors are confirmed, apply DOE, regression, or constrained optimization.

**Required information**: Raw-material and product quantities, inventories, composition and concentration, batch transfers, sampling and drains, rework and scrap, meter error, system boundary, and a common time window.

**Mathematical and constraint patterns**: Total and component balances, inventory change, yield decomposition, measurement residuals, and nonnegative losses.

**Applicability limits**: A nonclosing residual cannot be labeled as physical loss directly. Local yield must not improve by increasing downstream time, energy, or quality risk.

---

## [L1-032] Energy Use Is High but No Comparable Baseline Exists

**Level and keywords**: L1 general pathway; energy baseline; specific energy; energy balance; energy saving

**Scenario and symptoms**: Total steam, electricity, or refrigeration use is high, but throughput, product mix, weather, startup, shutdown, and standby effects are mixed together, obscuring whether load or efficiency is responsible.

**Optimization objective**: Build an energy baseline normalized for production and major operating conditions, identify avoidable consumption, and quantify improvement.

**Candidate algorithms**: Begin with submetering and energy balances. Use stratified regression, benchmark models, or control charts to identify abnormal use. After confirming drivers, apply RTO, equipment-load optimization, or operating-rule improvements.

**Required information**: Disaggregated energy, production, product and batch, equipment state, ambient conditions, startup and standby, temperature and pressure, maintenance, and modification dates.

**Mathematical and constraint patterns**: Energy balance, normalized KPIs, regression baseline, residual monitoring, and fixed-versus-variable consumption.

**Applicability limits**: Specific energy cannot be compared directly across products with different quality requirements. Savings must be calculated against a defined baseline, not two isolated periods.

---

## [L1-033] Waste Heat Exists but Sources and Sinks Cannot Be Matched Effectively

**Level and keywords**: L1 general pathway; heat recovery; heat integration; thermal storage; heat source and sink

**Scenario and symptoms**: Some operations discharge hot streams while others still consume steam or hot water, but temperature grade, timing, equipment, or hygiene separation prevents effective use; nominal recovery is high but actual utilization is low.

**Optimization objective**: Maximize heat that truly displaces purchased energy while respecting temperature, time, equipment, and hygiene conditions.

**Candidate algorithms**: Begin with time-resolved energy balances and source–sink matching. Use pinch analysis for steady design and time-sliced MILP with thermal inventory for batch processes. Optimize capital and operation jointly when selecting equipment.

**Required information**: Stream temperature, flow, heat capacity, timing, heat demand, minimum approach temperature, exchanger area, storage, cleaning, isolation, and energy price.

**Mathematical and constraint patterns**: Energy balances, temperature feasibility, temporal matching, thermal inventory, exchanger capacity, and investment binaries.

**Applicability limits**: Waste-heat sources are not interchangeable. Only heat actually used by a later demand and compliant with product and hygiene requirements counts as recovered heat.

---

## [L1-034] Concurrent Operations Create Utility Peaks

**Level and keywords**: L1 general pathway; steam; refrigeration; compressed air; peak load; rolling optimization

**Scenario and symptoms**: A production schedule appears locally feasible, but simultaneous heating, cooling, cleaning, or compressed-air users cause pressure or temperature sag, demand charges, or inadequate standby margin.

**Optimization objective**: Coordinate production tasks and utility equipment to reduce peaks, improve part-load efficiency, and preserve reserve while meeting product demand.

**Candidate algorithms**: Use rolling-horizon MILP or constrained scheduling for short-term coordination. Use LP/NLP for continuous unit load allocation. Jointly optimize production shifting and storage when thermal storage or price signals exist.

**Required information**: Production schedule, user load profiles, unit capacity and efficiency, startup constraints, storage state, energy prices, maintenance, and minimum reserve.

**Mathematical and constraint patterns**: Shared-resource capacity, unit commitment, load balance, inventory state, peak penalty, ramp limits, and reserve constraints.

**Applicability limits**: Do not obtain peak reduction by relaxing product temperature, pressure, or safety limits. Reoptimize as uncertain future loads unfold and retain a fallback plan.

---

## [L1-035] Excess Inventory, Shortages, and Material Aging Occur Together

**Level and keywords**: L1 general pathway; inventory optimization; material age; demand uncertainty; batch

**Scenario and symptoms**: Some materials occupy tanks or storage for long periods while others run short. Aggregate inventory hides batch, quality status, maturity, and shelf life.

**Optimization objective**: Reduce holding, shortage, disposal, and blockage risk while meeting production and service levels.

**Candidate algorithms**: Use LP or inventory-control policies for stable demand and scenario or stochastic optimization for uncertain demand and supply. In batch production, include material age, quarantine, release, and rework status in rolling plans.

**Required information**: Inventory and batch identity, receipt time, quality status, shelf life, demand forecast, replenishment lead time, tank or warehouse compatibility, and shortage and disposal costs.

**Mathematical and constraint patterns**: Inventory balance, material age, FIFO, service level, chance constraints, and rolling horizon.

**Applicability limits**: Products, states, and quality grades cannot be pooled unconditionally. Immature or unreleased material must not be consumed early by the model.

---

## [L1-036] Product Changeovers and Cleaning Consume Excessive Time

**Level and keywords**: L1 general pathway; changeover; cleaning; sequencing; sequence-dependent setup

**Scenario and symptoms**: Changeover, rinsing, cleaning, and preparation consume a large share of multiproduct production. Order-priority sequencing ignores the different costs of predecessor–successor product pairs.

**Optimization objective**: Reduce total changeover time, cleaning resources, and product loss while meeting delivery, inventory, and compatibility conditions.

**Candidate algorithms**: Use sequence-dependent MILP, constraint programming, or scheduling heuristics. Build a compatibility matrix before grouping compatible products or campaigns. Freeze near-term tasks during rolling rescheduling.

**Required information**: Product–equipment compatibility, predecessor–successor setup matrix, cleaning steps, time, water and chemical use, orders, inventory, and initial equipment state.

**Mathematical and constraint patterns**: Precedence, resource exclusivity, setup tasks, binary variables, route selection, and time windows.

**Applicability limits**: Allergen, contamination, food-safety, and validated cleaning requirements cannot be traded for shorter changeovers. Setup time must be specific to the equipment and product pair.

---

## [L1-037] Shared Equipment Causes Waiting, Blocking, and Starvation

**Level and keywords**: L1 general pathway; production scheduling; bottleneck; shared equipment; batch process

**Scenario and symptoms**: Products or operations compete for equipment, tanks, operators, or laboratory capacity. Upstream material cannot transfer, downstream equipment starves, and the bottleneck shifts with the order mix.

**Optimization objective**: Synchronize operations, transfers, and resource occupancy to reduce makespan, delay, blocking, and idle time.

**Candidate algorithms**: Use MILP or constraint programming at moderate scale and decomposition, rolling horizons, or structured heuristics at large scale. Add buffers, scenarios, or event-triggered rescheduling when completion times are uncertain.

**Required information**: Task routes, processing times, equipment and labor capacity, tanks, transfers, initial state, due dates, maintenance, and quality-release events.

**Mathematical and constraint patterns**: Precedence, equipment exclusivity, buffer capacity, blocking, batch state, start and finish times, and tardiness penalties.

**Applicability limits**: High local equipment utilization does not necessarily improve total delivery. The model must represent both material availability and actual transfer conditions.

---

## [L1-038] Shared Pipelines and Material Routes Conflict

**Level and keywords**: L1 general pathway; network flow; pipeline scheduling; routing; valve matrix

**Scenario and symptoms**: Equipment capacity is available, but shared lines, valve matrices, pumps, or loading points prevent simultaneous transfer. Ignoring route occupancy creates execution conflicts or contamination risk.

**Optimization objective**: Select available, compatible routes, coordinate transfer timing, and reduce waiting, residue, and cleaning.

**Candidate algorithms**: Use time-expanded network flow or MILP with route selection. Enumerate validated legal routes for complex valve matrices. Combine rolling scheduling with state confirmation when availability changes in real time.

**Required information**: Equipment and piping topology, valve state, direction, capacity, transfer time, shared pumps, product compatibility, residue, and cleaning requirements.

**Mathematical and constraint patterns**: Node balance, arc capacity, route binaries, resource exclusivity, direction limits, and transfer time.

**Applicability limits**: Model topology must match field valves and interlocks. Incompatible products must not share an uncleaned route.

---

## [L1-039] Raw-Material Composition Variation Destabilizes Recipe Cost or Quality

**Level and keywords**: L1 general pathway; blending; recipe optimization; raw-material lot; robust recipe

**Scenario and symptoms**: Composition, activity, and moisture vary by source or lot. Fixed ratios cause product variation, while conservative manual practice overuses expensive materials.

**Optimization objective**: Optimize ingredient proportions, cost, and robustness to lot differences under composition, quality, inventory, and compatibility constraints.

**Candidate algorithms**: Use LP or QP when composition relations are approximately linear. For mixture interactions, use mixture experiments and response surfaces before NLP. Apply robust or scenario recipe optimization when lot uncertainty is significant.

**Required information**: Lot composition and uncertainty, costs, inventory, allowed ratios, target quality, mixing shrinkage or reaction, and historical recipe results.

**Mathematical and constraint patterns**: Component balances, proportions summing to one, quality bounds, inventory, mixture response, and uncertainty sets.

**Applicability limits**: Concentrations require mass- or volume-weighted averaging, not a simple mean. Validate new recipes in laboratory or controlled production rather than accepting model cost alone.

---

## [L1-040] Maintenance Plans Repeatedly Conflict with Production Due Dates

**Level and keywords**: L1 general pathway; maintenance scheduling; production planning; health state; outage window

**Scenario and symptoms**: Production repeatedly postpones maintenance until an unplanned failure occurs, or fixed-interval shutdowns ignore actual health, orders, and backup capacity.

**Optimization objective**: Jointly consider failure risk, maintenance resources, and lost production to schedule executable maintenance windows while preserving redundancy for critical assets.

**Candidate algorithms**: Convert health index, failure probability, or remaining useful life into risk inputs and jointly schedule production and maintenance using MILP, stochastic programming, or rolling optimization. Apply conservative rules to high-consequence equipment.

**Required information**: Equipment hierarchy, failure modes, condition monitoring, maintenance duration, labor and spares, orders, backup assets, failure consequences, and statutory intervals.

**Mathematical and constraint patterns**: Equipment availability, maintenance tasks, resource occupancy, failure scenarios, production loss, and minimum reserve capacity.

**Applicability limits**: Predicted life is not a deterministic countdown. Statutory inspections, protection tests, and maintenance of high-consequence equipment must not be postponed by economic optimization alone.

---

## [L1-041] Product Remains Within Specification While Process Variability Expands

**Level and keywords**: L1 general pathway; process capability; SPC; quality variability; robust control

**Scenario and symptoms**: Most finished product remains conforming, but process variance, near-limit batches, or rework risk increases. The team tracks only specification violations and ignores the distinction between statistical stability and conformance.

**Optimization objective**: Restore a stable process and reduce variance and near-boundary risk rather than improving only the mean.

**Candidate algorithms**: Use control charts and capability analysis to distinguish drift, special causes, and common variation. After identifying drivers, apply DOE, feedforward, control tuning, or robust optimization to reduce variability.

**Required information**: Stable history, specification and control limits, raw-material lots, equipment and shifts, measurement system, rework, and the quality distribution near limits.

**Mathematical and constraint patterns**: Mean and variance, control limits, process capability, stratified baselines, robust objectives, and quality safety margins.

**Applicability limits**: Specification limits are not statistical control limits. Until measurement-system capability is confirmed, apparent process variation may mainly reflect test error.

---

## [L1-042] Endpoint Quality Failure Is Found Only After the Batch Ends

**Level and keywords**: L1 general pathway; terminal quality; batch endpoint; soft sensor; dynamic optimization

**Scenario and symptoms**: Temperature and flow appear normal during processing, but final composition, conversion, or quality fails requirements and no early forecast or corrective action is available.

**Optimization objective**: Predict terminal-quality risk early and adjust trajectories, stage duration, or endpoint conditions to reduce scrap and rework.

**Candidate algorithms**: Build a terminal-quality soft sensor or batch-evolution model and update its forecast through state estimation. Use dynamic optimization or MPC when trajectories can be manipulated; otherwise optimize sampling and intervention timing.

**Required information**: Within-batch time series, initial feed, stage events, final laboratory results, manipulated variables, abnormal batch history, and allowable corrective actions.

**Mathematical and constraint patterns**: Dynamic states, terminal and path constraints, completion-time decisions, risk prediction, and soft constraints.

**Applicability limits**: Endpoint forecasts require uncertainty and rejection conditions. Final safety and quality release cannot rely only on model output.

---

## [L1-043] Water, Wastewater, or Emission Loads Form Concentrated Peaks

**Level and keywords**: L1 general pathway; water network; wastewater; emission peak; environmental constraint

**Scenario and symptoms**: Production, cleaning, or discharge tasks overlap, causing water shortage or shocks in treatment capacity, pH, or organic load even though daily totals appear acceptable.

**Optimization objective**: Reduce fresh-water use and instantaneous discharge peaks and increase qualified reuse while satisfying production and environmental limits.

**Candidate algorithms**: Build water balances and quality grades by use. Optimize reuse paths with network flow and stagger discharges with MILP or rolling schedules. Use scenarios when loads are uncertain.

**Required information**: Water use by consumer, discharge flow and quality, task times, storage and treatment capacity, reuse routes, discharge limits, and product and hygiene compatibility.

**Mathematical and constraint patterns**: Water and component balances, network flow, instantaneous capacity, inventory, task scheduling, quality matching, and cumulative discharge.

**Applicability limits**: Compliance on total volume does not excuse instantaneous overload. Reuse must meet use-specific hygiene, chemical, and product-contact requirements.

---

## [L1-044] Equipment Performance Degrades Gradually but Maintenance Timing Is Uncertain

**Level and keywords**: L1 general pathway; predictive maintenance; RUL; degradation; condition monitoring

**Scenario and symptoms**: Vibration, current, temperature, pressure drop, or efficiency changes gradually. Fixed thresholds cannot distinguish normal load effects from degradation, leading to premature maintenance or post-failure repair.

**Optimization objective**: Estimate health state, failure risk, or a remaining-life interval and provide inspection or maintenance recommendations with adequate lead time.

**Candidate algorithms**: Use reliability, survival, or supervised models when failure history exists. Use state-space and degradation models for continuous signals. With sparse labels, combine anomaly detection, mechanistic features, and transfer from similar equipment.

**Required information**: Equipment hierarchy, failure modes, operating load, condition signals, maintenance and failure history, post-repair reset, inspection cost, and failure consequence.

**Mathematical and constraint patterns**: Health index, degradation state, failure probability, RUL distribution, lead time, false-positive and false-negative costs, and maintenance window.

**Applicability limits**: RUL is a conditional prediction with uncertainty, not a deterministic countdown. It cannot replace statutory inspection or protective systems.

---

## [L1-045] A Model Fails After Transfer to New Equipment, Products, or Lines

**Level and keywords**: L1 general pathway; transfer learning; domain adaptation; new product; cross-equipment model

**Scenario and symptoms**: A soft sensor, anomaly detector, or optimizer that works on one asset or product develops systematic bias after transfer because sensors, scale, recipes, and operating ranges differ.

**Optimization objective**: Reduce target-domain data requirements while identifying which knowledge can be reused and which parameters require re-identification and validation.

**Candidate algorithms**: Compare variable definitions, mechanisms, and data distributions first. When structures match, transfer features or parameters and calibrate with limited target data. Use hierarchical models, domain adaptation, or full rebuilding for larger differences.

**Required information**: Source- and target-domain variables, equipment size, sensors and sampling, product recipes, operating envelopes, a small labeled target set, and an independent validation set.

**Mathematical and constraint patterns**: Domain distance, parameter scaling, hierarchical random effects, transfer regularization, target calibration, and applicability-domain classification.

**Applicability limits**: Transfer reduces data requirements, not validation responsibility. Different mechanisms or measurement semantics can cause negative transfer.

---

## [L2-BER-001] Raw-Material Lot Variation Destabilizes Mashing Yield and Lautering Performance

**Level and keywords**: L2 brewing scenario; malt lot; recipe; milling; extract; lautering

**Scenario and symptoms**: Changes in moisture, extract, protein, enzyme activity, and husk condition across malt and adjunct lots cause a fixed recipe and roller gap to produce simultaneous variation in mash yield, lautering time, and turbidity.

**Optimization objective**: Improve raw-material utilization and control recipe cost while preserving target wort composition, lautering capacity, and brand quality.

**Candidate algorithms**: Use mixture experiments, DOE, or constrained regression to model lot–recipe–milling–outcome relationships. Then optimize proportions with LP/QP or robust formulation and, where needed, jointly optimize roller gap and feed rate.

**Required information**: Raw-material lot analyses, recipes, roller gap, particle-size distribution, mash yield, lautering time, pressure drop, turbidity, inventory, cost, and finished-product quality.

**Mathematical and constraint patterns**: Component balances, proportions summing to one, quality bounds, lot uncertainty, multiobjective tradeoffs, and equipment ranges.

**Applicability limits**: Do not sacrifice filter-bed structure, foam, flavor, or brand consistency for short-term extract. Confirm new combinations in laboratory or controlled production.

---

## [L2-BER-002] A Fixed Mashing Trajectory Cannot Adapt to Raw Materials and Fill Volume

**Level and keywords**: L2 brewing scenario; mashing; temperature trajectory; pH; fermentable sugar; energy

**Scenario and symptoms**: Every batch uses the same temperatures, rest durations, and heating rates. Changes in feed, pH, or fill volume then alter sugar profile, extract, viscosity, cycle time, and energy use.

**Optimization objective**: Optimize the temperature–time–pH trajectory under heating-capacity and wort-quality constraints, balancing extract, fermentability, cycle time, and energy.

**Candidate algorithms**: Screen primary factors with DOE. Use dynamic optimization when reliable kinetic or hybrid models exist, state estimation or MPC for online correction, and a local response surface when data are limited.

**Required information**: Raw-material analysis, liquor-to-grist ratio, multipoint temperatures, pH, agitation, steam flow, online or offline extract, sugar profile, and lautering and fermentation outcomes.

**Mathematical and constraint patterns**: Enzyme kinetics, energy balance, dynamic states, heating capacity, path constraints, stage time, and terminal quality.

**Applicability limits**: Laboratory trajectories cannot be transferred directly to a production vessel. Nonuniform heat transfer and local temperature can create a false optimum, and brand and equipment limits require revalidation.

---

## [L2-BER-003] Lauter-Tun Pressure Drop Rises and Weak-Wort Endpoint Uses Fixed Time

**Level and keywords**: L2 brewing scenario; lauter tun; filter bed; pressure drop; sparging; weak wort

**Scenario and symptoms**: Higher pump speed compacts the bed and raises pressure drop and turbidity. Fixed-time sparging may leave recoverable extract behind or add water and downstream evaporation load without sufficient value.

**Optimization objective**: Shorten lautering within clarity and equipment pressure-drop limits and continue sparging only while marginal recovery exceeds the added water, steam, time, and quality risk.

**Candidate algorithms**: Estimate bed state from a pressure-drop–flow dynamic model or soft sensor and coordinate pump and rake with feedback or MPC. Use an economic endpoint rule or dynamic optimization to stop weak-wort collection.

**Required information**: Flow, pressure drop, level, turbidity, temperature, online concentration, cumulative volume, rake and pump speed, particle-size distribution, kettle capacity, and energy cost.

**Mathematical and constraint patterns**: Filtration resistance, hydraulic pressure drop, cumulative recovery, equipment bounds, quality soft constraints, and marginal value.

**Applicability limits**: Do not maximize instantaneous flow alone. Concentration instruments require temperature compensation, and bed stability, turbidity, and brand-approved sparging limits cannot be relaxed.

---

## [L2-BER-004] Boiling Steam, Wort Cooling, and Hot-Water Recovery Are Disconnected

**Level and keywords**: L2 brewing scenario; boiling; cooling; steam; heat recovery; hot-water inventory

**Scenario and symptoms**: Boiling follows a fixed steam trajectory, while hot water recovered during cooling does not align with later mashing or CIP demand, causing steam waste, tank overflow, or delay to the next batch.

**Optimization objective**: Reduce steam and cooling load, increase heat recovery that is actually usable, and preserve production rhythm under boiling and pitching-quality requirements.

**Candidate algorithms**: Build boiling and heat-exchanger energy balances. Use dynamic optimization for the steam trajectory and time-sliced MILP or rolling optimization to coordinate cooling, hot-water inventory, and future demand.

**Required information**: Kettle volume, concentration, steam, temperature and evaporation, quality tests, exchanger inlet and outlet conditions, hot-water tank temperature and inventory, batch schedule, and CIP demand.

**Mathematical and constraint patterns**: Sensible and latent heat, exchanger capacity, terminal quality, hot-water inventory, temporal task matching, and multiobjective cost.

**Applicability limits**: A fixed total evaporation rate cannot replace quality testing. Heat recovery must not delay wort cooling, violate hygiene isolation, or exceed exchanger capability.

---

## [L2-BER-005] Pitching and Aeration Create Inconsistent Initial Fermentation States

**Level and keywords**: L2 brewing scenario; yeast; pitching; dissolved oxygen; propagation; fermentation consistency

**Scenario and symptoms**: Operation by slurry volume or fixed gas flow ignores yeast generation, viability, storage time, and wort gravity, producing large differences in viable cell count and dissolved oxygen.

**Optimization objective**: Stabilize the initial cell and oxygen state, reduce lag, cycle-time, and flavor variation, and preserve yeast health.

**Candidate algorithms**: Use hierarchical regression or kinetic models to predict early fermentation. Apply feedforward pitching and aeration from wort and yeast state with online DO feedback. Use constrained optimization for lot allocation.

**Required information**: Viable cells, viability and vitality, generation, storage, wort flow and gravity, gas flow and pressure, online DO, pitching temperature, and early fermentation trajectories.

**Mathematical and constraint patterns**: Cell and oxygen component balances, kinetics, measurement delay, batch allocation, quality constraints, and uncertainty.

**Applicability limits**: Supplied oxygen is not the same as dissolved oxygen. Abnormal or contaminated yeast requires rejection rules, and parameters cannot be transferred directly across strains and brands.

---

## [L2-BER-006] Fermentation Temperature and Stage Endpoints Depend on Fixed Days

**Level and keywords**: L2 brewing scenario; fermentation; temperature trajectory; soft sensor; MPC; stage endpoint

**Scenario and symptoms**: Primary fermentation, diacetyl reduction, cooling, and transfer occur after fixed durations. Batch differences cause premature transitions, needless waiting, temperature overshoot, or quality risk.

**Optimization objective**: Shorten the cycle and improve batch consistency within flavor, endpoint-quality, and cooling-capacity constraints.

**Candidate algorithms**: Predict sugar, ethanol, and by-products with kinetic or mechanistic–data hybrid models. Fuse online and delayed laboratory data with state estimation. Generate temperature trajectories with dynamic optimization, correct them online with MPC, and trigger endpoints jointly from quality, rate of change, and confidence.

**Required information**: Multipoint temperature, coolant valve position and supply/return temperature, density, pressure, CO2, pH, VDK/diacetyl, ethanol, yeast lot, vessel type, and fill volume.

**Mathematical and constraint patterns**: Fermentation kinetics, state estimation, temperature path and rate limits, terminal quality, stage state machine, and soft constraints.

**Applicability limits**: Published temperature and quality thresholds are not plant setpoints. Abnormal batches require human review, and laboratory release and cooling-capacity limits cannot be bypassed.

---

## [L2-BER-007] Beer-Filtration Pressure Drop Rises Rapidly at Higher Flux

**Level and keywords**: L2 brewing scenario; beer filtration; flux; pressure drop; turbidity; filtration endpoint

**Scenario and symptoms**: Sustained high flux increases filtration resistance and pressure drop, reduces the total volume handled per run, and raises turbidity, consumables, beer loss, or cleaning frequency.

**Optimization objective**: Maximize whole-run throughput and reduce consumables, beer loss, and CIP frequency under finished-beer clarity, dissolved-oxygen, and equipment pressure-drop requirements.

**Candidate algorithms**: Estimate resistance growth and remaining capacity with a state model or soft sensor. Adjust flux using dynamic optimization or MPC and determine switching, backwash, or cleaning with an economic endpoint rule.

**Required information**: Inlet and outlet turbidity, flow, pressure drop, temperature, media and filter aid, beer loss, dissolved oxygen, run duration, cleaning, and upstream batch quality.

**Mathematical and constraint patterns**: Filtration-resistance dynamics, flow–pressure relationship, cumulative throughput, terminal decision, equipment bounds, and multiobjective tradeoffs.

**Applicability limits**: Stratify models by product and filter medium. Do not determine the endpoint from pressure drop or fixed time alone; microbiological and finished-product requirements remain hard limits.

---

## [L2-BER-008] A Fixed CIP Program Causes Overcleaning and Wastewater Peaks

**Level and keywords**: L2 brewing scenario; CIP; cleaning endpoint; water; chemicals; wastewater scheduling

**Scenario and symptoms**: Equipment with different soil loads receives the same cleaning time, temperature, and rinse volume, while concurrent CIP circuits shock water, heating, and wastewater capacity.

**Optimization objective**: Reduce water, steam, chemicals, and time and cut site wastewater peaks within validated cleanliness and food-safety windows.

**Candidate algorithms**: Define non-negotiable cleaning windows from sanitation validation. Use conductivity, temperature, flow, and return signals for stage endpoint rules or state estimation. Coordinate concurrent CIP and treatment capacity with MILP or rolling schedules.

**Required information**: Equipment and previous product, CIP stages, supply and return flow and temperature, conductivity, chemical concentration, cleaning validation, metered water, wastewater flow, pH, and organic load.

**Mathematical and constraint patterns**: Stage state machine, endpoint conditions, minimum time–temperature–concentration, resource exclusivity, water and component balances, and peak capacity.

**Applicability limits**: Clear return liquid alone does not prove cleanliness. Food safety, chemical exposure, and personnel safety are hard constraints, and a shortened program requires revalidation.

---

## [L2-BER-009] Refrigeration, Steam, and CO2 Supply and Demand Create Peaks and Venting

**Level and keywords**: L2 brewing scenario; refrigeration; steam; CO2; utilities; load forecasting

**Scenario and symptoms**: Mashing, boiling, cooling, fermentation, CIP, and packaging loads overlap. Utility units run inefficiently or cycle frequently, cooling and steam become insufficient, and CO2 storage both vents and requires purchased supply.

**Optimization objective**: Coordinate units, storage, and movable production tasks to reduce peaks and energy cost, improve supply–demand matching, and preserve reliable reserve.

**Candidate algorithms**: Begin with user-level metering and load forecasts. Use rolling MILP for unit commitment, load allocation, thermal storage, and production shifting. Refine continuous setpoints with RTO or MPC.

**Required information**: Production schedule, user loads, unit state and part-load efficiency, supply and return temperature and pressure, pump speed, storage, energy prices, CO2 generation and use, and maintenance.

**Mathematical and constraint patterns**: Utility-network balance, unit commitment, inventory, peak, ramp, reserve capacity, and rolling forecasts.

**Applicability limits**: Product temperature, steam safety, refrigeration reserve, food-contact gas purity, and ventilation requirements cannot be softened. Reduce leaks and wasteful purging before evaluating recovery investment.

---

## [L2-BER-010] Fermentation, Maturation, Filtration, and Packaging Schedules Block One Another

**Level and keywords**: L2 brewing scenario; brewery scheduling; fermenter; filtration; packaging; quality release

**Scenario and symptoms**: Multiday fermentation and maturation, batch filtration, and high-speed packaging operate on different timescales. Brand and package changes create tank blockage, packaging starvation, delay, and utility peaks.

**Optimization objective**: Synchronize liquid readiness, quality release, filtration, bright-beer tanks, and packaging to reduce blocking, changeovers, and inventory waiting while meeting delivery and quality requirements.

**Candidate algorithms**: Use MILP, constraint programming, or rolling scheduling to represent tank occupancy, maturation, filtration, and packaging jointly. Model cleaning, transfer, and quality testing as real tasks. Use scenarios or event-triggered replanning for uncertain fermentation completion.

**Required information**: Order due dates, recipe batch sizes, tank capacity and compatibility, fermentation completion probability, quality release, filtration and packaging capacity, changeover matrix, CIP, maintenance, and package-quality metrics.

**Mathematical and constraint patterns**: Batch state and material age, equipment exclusivity, precedence, changeover, quality-release events, due dates, and a rolling freeze window.

**Applicability limits**: “Ready to package” must be defined by a quality-release event, not planned completion time alone. Safety pressure, product stability, and rated packaging capacity cannot be softened.

---

### Maintenance Notes

- A new second-level entry must describe one primary symptom and one primary optimization objective. Split monitoring, control, and economic optimization into separate pathways when the same phenomenon leads to different decisions.
- Each entry should list no more than three candidate algorithm families and state their activation conditions. Do not generate unvalidated symptom × objective × algorithm combinations.
- Every entry must be self-contained and avoid cross-chunk references such as “see the previous entry.” Algorithm, data, and constraint information should support clarification and modeling without replacing site SOPs, interlocks, or quality release.
- Public literature, internal cases, and vendor materials should be labeled separately. Source links and entry mappings belong in a separate tracking file and are not part of this knowledge-base index.
