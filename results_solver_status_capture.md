The results.solver attribute contains a SolverInformation object.
Key attributes of this object are shown in the table below. 

| Attribute             | Explanation                                                                                                                                                                                                                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status                | Returns the solver status as a PyUtilib.Enum SolverStatus that can be: **ok, warning, error, aborted, or unknown.**                                                                                                                                                                                        |
| termination_condition | Returns the specific termination condition as reported by the **solver**. This is a PyUtilib enum TerminationCondition that can have different values, including optimal, infeasible, or unbounded. There are many different solver outcomes, and **depending on the solver**, you may see other outcomes. |
| termination_message   | String message returned by the solver summarizing the termination status. Tt also **depends on the solver.**                                                                                                                                                                                               |

if you print results.solver using gurobi when meeting troubles.

```
Status: warning
Return code: 0
Message: Problem proven to be infeasible or unbounded.
Termination condition: infeasibleOrUnbounded
Termination message: Problem proven to be infeasible or unbounded.
```

You can capture specific solving status referring above code.

i.e. when using gurobi, a termination condition can be TerminationCondition.infeasibleOrUnbounded.

If you want this status to be captured through gurobi solver, use

```python
 if results.solver.termination_condition == TerminationCondition.infeasibleOrUnbounded
```

when using ipopt, results.solver will be printed as

```
- Status: warning
  Message: Ipopt 3.11.1\x3a Converged to a locally infeasible point. Problem may be infeasible.
  Termination condition: infeasible
  Id: 200
  Error rc: 0
  Time: 0.2812974452972412
```

If you want this status to be captured through ipopt solver, use

```python
 if results.solver.termination_condition == TerminationCondition.infeasible
```
