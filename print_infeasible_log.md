# how to print infeasible log?

eg. Somehow, one of the variable inside my model violates one constraint, which makes my model infeasible:

```python
WARNING: Loading a SolverResults object with a warning status into model=xxxx;
    message from solver=Model was proven to be infeasible.
```

Is there a way to ask the solver, the reason of the infeasibility?

So for example, lets assume I got a variable called `x`, and if I define following 2 constraints, model will be ofc. infeasible.

```
const1:
    x >= 10

const2:
    x <= 5
```

And what I want to achieve that pointing out the constraints and variable which causes this infeasibility, so that I can fix it. Otherwise with my big model it is kinda hard to get what causing this infeasibility. **(Expectation)**

```
IN: write_some_comment
OUT: variable "x" cannot fulfill "const1" and "const2" at the same time.
```

## solution

```python
from pyomo.util.infeasible import log_infeasible_constraints
# ...
# SolverFactory('your_solver').solve(model)
# ...
log_infeasible_constraints(model)
```

## Annotation

Plus, it will only output infeasible constraints, instead of an immediate sentence as above, 'cannot fulfill constraint simultaneously'. 

In 10th Dec. 2020, when solving problems with a fixed Bas input from Excel manual version, Terminal can only print infeasible constraints. However, the real problem came from infeasible variables. Munual Bas map contains negative values, while the variable in the model was given a domain 'NonNegativeReals'. Hence an error was reported in the first constraint, which calls the pyomo Var in the model.

reference link:

[variables - Finding out reason of Pyomo model infeasibility - Stack Overflow](https://stackoverflow.com/questions/51044262/finding-out-reason-of-pyomo-model-infeasibility)
