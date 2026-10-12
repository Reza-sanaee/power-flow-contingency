## Attribution and study scope

The submitted project is attributed to Ashkan Yousefnia (401102829) and Reza Sanaee Nezhad (401101502). The source does not identify separate individual contributions. A PowerWorld case studies two iterative solvers, reactive compensation, tap adjustment, parallel equipment, load shedding, and three contingencies. All 27 experiment report pages and their simulator tables/screenshots are retained; this edition translates the Persian portions and expands the technical interpretation of the English portions.

## 1. Gauss-Seidel iteration

The report records a single-iteration view, a run allowing 100 iterations, running status, bus-flow listing, and bus mismatch listing. A maximum-iteration setting is not the same as a measured convergence count. Assess convergence from the reported residuals and solver status. The screenshots are preserved so the actual simulator state remains inspectable rather than replaced by a generic claim of success.

## 2. Newton-Raphson and Jacobian

The corresponding Newton sequence includes one-iteration and 100-iteration settings, flow and mismatch views, and the power-flow Jacobian. Newton's local linearization can converge more rapidly near a suitable solution, but the recorded screenshots do not constitute a controlled timing benchmark. Both methods solve the same nonlinear steady-state equations; a method's convergence behavior is separate from network dynamic stability.

## 3. Shunt compensation and voltage limits

With the capacitor banks connected, the report states that all bus magnitudes lie between 0.95 and 1.05 per unit. It then disconnects the bus-3 bank and displays the modified network, voltages, and losses. The report observes reduced voltages away from the controlled generator buses and higher active/reactive loss entries. Removing local capacitive support increases reactive transfer requirements. Under a constant-power load model, lower voltage can increase current demand and resistive loss. At fixed real power and voltage, decreasing power factor raises current. The causal explanation must use the actual load model. Capacitor support can reduce line current and losses and support voltage, but excessive compensation can cause overvoltage. The finite case comparison does not establish equipment lifetime or a universal optimal capacitor size.

## 4. Tap adjustment

The recorded tap ratio 0.93130 brings bus 3 close to 1.0 per unit, corresponding to its 345-kV nominal level. The follow-up table is described as within all voltage limits. The controlled target is bus 3, not bus 7 or bus 1. Tap direction depends on the modeled transformer-ratio convention. The case file and tabulated bus voltages specify the observation.

## 5. Parallel transformers

The report adds an identical transformer between buses 1 and 4 and records the resulting sharing. It then doubles the added transformer's reactance and displays the changed flows and bus voltages. For ideal parallel branches with compatible voltage ratios and comparable angle assumptions, lower impedance carries more current; unequal reactances prevent equal sharing. Network controls and resistances can alter the simple inverse-reactance estimate. The submitted text notes changes in reactive generation/consumption and lower voltages at buses other than the regulated generator buses in the doubled-reactance case. These are results of that edited case. The screenshots retain actual values; no new dispatch simulation is claimed here.

## 6. Line outage and corrective load reduction

After removing line 3-4, the report reduces bus-3 load to P = 40 MW and Q = 30 MVAr. Relative to the specified 50 MW/34 MVAr, this sheds 10 MW and 4 MVAr. The edited load is a demonstrated manual case adjustment, not a proved minimum shedding solution or an OPF result. A minimum would require a defined optimization objective and constraints, together with a search or optimization certificate.

## 7. Parallel transmission line

Adding an identical line between buses 3 and 4 yields before/after flow and total-loss views. The report observes smaller active and reactive losses. An additional path can reduce branch loading and effective transfer impedance, but actual improvement depends on the operating point, control response, and constraints. The retained tables show the submitted example; a general reliability claim needs contingency screening as well as the base-case comparison.

## 8. Contingency-analysis workflow and results

The recorded workflow solves the base circuit, opens Tools/Contingency Analysis, inserts contingencies and actions, and selects Run. Its creation and result dialogs are preserved.

| Contingency | Recorded outcome | Interpretation boundary |
|---|---|---|
| Open line 3-4 | buses 3 and 7 below the stated lower voltage bound | steady-state voltage violation in the reported case |
| Open line 5-7 | solver does not converge to a displayed solution | investigate topology, controls, initialization, and feasibility; not a demonstrated dynamic-instability result |
| Move generator from bus 2 to bus 3 | no reported voltage-limit violation | limited to this modeled replacement and criterion |

Generator relocation is a comparison scenario, not ordinary immediate outage reserve unless a feasible replacement unit is actually available. Also check branch ratings, reactive limits, and island balance before declaring a case secure under broader criteria.

## 9. Discussion: method selection

Gauss-Seidel has a simple update and modest storage requirements. It can need many iterations and be sensitive to operating point and initialization. Newton-Raphson forms/factors a Jacobian and uses more work per step, but often converges in fewer steps. The report prefers Newton for larger networks. Avoid claiming inherently higher final accuracy when both methods satisfy the same residual tolerance.

## 10. Discussion: compensation, OPF data, and resilience

Shunt banks supply reactive power locally, supporting voltage and reducing reactive transfer where appropriately sized. Their location, switching, voltage limits, and resonance/control interactions matter. The supplied case demonstrates particular voltage/loss effects rather than a complete compensation design. OPF requires topology and impedances; generator capabilities and active/reactive limits; load data; voltage and branch constraints; transformer controls; objective/cost functions; and a specified dispatch policy. Startup costs and ramp limits belong to formulations that explicitly model commitment or time coupling, rather than every static OPF. The report proposes reserve generation, stronger/parallel transmission paths, suitable transformers, capacitor banks, and smarter protection. These can contribute to resilience, but their benefit needs scenario-specific study and coordination. Proposed protective systems and reserve plants were not implemented in this project. The source case files and outputs are organized separately to support later reproduction in PowerWorld.
