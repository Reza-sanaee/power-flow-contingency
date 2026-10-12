# Seven-bus power-flow and contingency-analysis assignment

## Purpose and modeling assumptions

Solve unknown bus voltages and network complex-power flows for known loads, generation schedules, and operating limits. Use a balanced three-phase steady-state model represented by a one-line diagram. Power flow supports operational planning and contingency screening; a converged steady-state solution alone does not establish transient or small-signal stability. Choose base apparent power S_base and voltage bases consistent with transformer ratios. In per unit, a quantity equals its actual value divided by its base. For three-phase bases, I_base = S_base/(sqrt(3)*V_LL,base) and Z_base = V_LL,base^2/S_base. Correctly convert equipment impedance to the chosen common base. The experiment uses S_base = 100 MVA.

## Nodal equations and bus types

Let I = Y_bus V and S_i = P_i+jQ_i = V_i*conj(I_i). Net injection equals generation minus load. With Y_ij = G_ij+jB_ij and angle difference theta_i-theta_j, P_i = sum_j abs(V_i)*abs(V_j)*(G_ij*cos(theta_i-theta_j)+B_ij*sin(theta_i-theta_j)); Q_i = sum_j abs(V_i)*abs(V_j)*(G_ij*sin(theta_i-theta_j)-B_ij*cos(theta_i-theta_j)).

| Bus type | Specified | Solved |
|---|---|---|
| Slack/swing | voltage magnitude and reference angle | real/reactive injection |
| PV/generator | real injection and voltage magnitude | angle and reactive injection |
| PQ/load | real and reactive injection | voltage magnitude and angle |

A slack bus balances losses and mismatch within the modeled connected network. Reactive-generation limits can cause a PV bus to become a PQ bus at its bound. Voltage limits and line/transformer ratings are additional operating constraints, not automatically satisfied by solving the equations.

## Gauss-Seidel method

For A x = b, update x_i using newly updated components before i and previous-iteration components after i. In power flow, replace unknown current injections by conj(S_i/V_i) to obtain the voltage iteration. For PQ buses use specified P/Q. At a PV bus compute reactive injection, check generation bounds, update angle, and enforce specified magnitude if the reactive control is feasible. The slack voltage remains fixed. State initialization, iteration order, tolerance, and maximum iterations. Nonconvergence can reflect initialization, conditioning, control limits, or an infeasible case. It does not by itself prove physical dynamic instability.

## Newton-Raphson method

Linearize the nonlinear residual around the current state. The scalar method uses x_next = x-f(x)/f'(x), a tangent step, rather than the distinct derivative-free secant method. For a vector system solve J*delta_x = mismatch and update angles and PQ magnitudes with consistent signs. With n buses and m PQ buses, excluding the slack gives n-1+m unknowns and equations under the stated ordering. Check residual and step tolerances, reactive limits, and failed/singular solves. Newton often converges in fewer iterations than Gauss-Seidel, especially for large cases, but iterations and convergence are not universally independent of network size or operating condition. Each iteration requires Jacobian construction/factorization and additional storage.

## Contingencies and PowerWorld workflow

An N-1 screening removes one specified element and checks whether the remaining steady-state case satisfies operating criteria. The wider software also offers economic dispatch, OPF, and fault analysis. These available tools are background, not all required completed experiments. The examples illustrate a three-bus network, a Newton tangent step, a two-bus case, an OPF dialog, and a fault-analysis dialog.

## Seven-bus case data

Use two generators and four loads with shunt capacitor banks at buses 3 and 7. The 345-kV lines have impedance 0.1019+j0.5912 ohm/km. Line lengths L1-L5 are 500,100,300,200,100 km. Transformers have reactance j0.1580 per unit. Generator maximum outputs are 100 MW. Line ratings are 100 MVA and transformer ratings 150 MVA. Participation factors are four at bus 1 and six at bus 2; unspecified parameters use the supplied defaults.

| Bus | Pg MW | Qg/table MVAr | Pl MW | Ql MVAr | Type code | Qmax MVAr | Qmin MVAr | kV |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | 55 | 32 | 0 | 0 | 2 | 40 | -6 | 34.5 |
| 2 | 82 | 33 | 26 | 0 | 1 | 40 | -6 | 34.5 |
| 3 | 0 | 37 | 50 | 34 | 3 | 0 | 0 | 345 |
| 4 | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 345 |
| 5 | 0 | 0 | 0 | 0 | 3 | 0 | 0 | 345 |
| 6 | 0 | 0 | 40 | 23 | 3 | 0 | 0 | 345 |
| 7 | 0 | 10 | 20 | 15 | 3 | 0 | 0 | 345 |

Codes are 1 slack, 2 PV, 3 PQ. Reactive entries at buses 3/7 relate to the stated shunts and must not be mistaken for unrestricted generator controls. 

## Part I: seven required experiments

1. Build the case, select Run Mode, and solve with Gauss-Seidel one iteration at a time. Then clear Do Only One Iteration and allow up to 100 iterations. Inspect Bus Power Flows and Bus Real/Reactive Power Mismatches.
2. Repeat with Newton-Raphson and inspect the Jacobian through Case Information, Other, Power Flow Jacobian. Compare residual behavior under a common stopping rule.
3. Apply 0.95-1.05 per-unit voltage limits. Disconnect the bus-3 capacitor bank and record all voltage and loss changes.
4. Change the bus-1/bus-4 transformer tap to bring bus 3 toward 1.0 per unit while retaining all bus limits.
5. Add a parallel transformer between buses 1 and 4, first with identical parameters, then with twice the reactance. Examine sharing and voltage effects.
6. Remove line 3-4 and find how much bus-3 load must be shed to keep its voltage above 0.95 per unit.
7. Add an identical parallel line 3-4 and compare line flows and total losses.

## Part II and discussion

Use the contingency-analysis tool with the stated +/-5-percent voltage criterion. Examine removal of line 3-4, removal of line 5-7, and replacing the bus-2 generator with a similar bus-3 generator. Record solve status, voltage/flow violations, and assumptions for each case. Discuss advantages of both solution methods; the role and limits of shunt compensation; topology, equipment, cost, and constraints needed for OPF; and ways to improve reliability/resilience. Distinguish proposed measures from verified contingency improvements. The original English Word assignment has been rendered to a 13-page PDF so that embedded equations, drawings, tables, and dialog figures remain readable.
