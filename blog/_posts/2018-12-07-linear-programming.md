---
layout: post
title: "Linear Programming in Python"
date: 2018-12-07
---

interesting stuff!! have not used my Excel Solver since... forever. LOL

---

Check out Pulp for intuitive linear programming:

https://github.com/coin-or/pulp

```python

from pulp import LpVariable, LpProblem, LpMinimize, value

x = LpVariable("x", 0, 3)
y = LpVariable("y", 0, 1)

# create problem
prob = LpProblem("myProblem", LpMinimize)
# add constraint
prob += x + y <= 2
# define objective
prob += -4*x + y
# solve
status = prob.solve()

# print results
print(f'x = {value(x)}, y = {value(y)}')

```

---

Check out scipy for *Constrained minimization of multivariate scalar functions*. Matrix multiplication is back to haunt me lol - was never very good at them.

https://docs.scipy.org/doc/scipy/reference/tutorial/optimize.html

```python

import numpy as np
from scipy.optimize import Bounds, LinearConstraint, NonlinearConstraint, minimize, rosen, rosen_der, rosen_hess

bounds = Bounds([0, -0.5], [1.0, 2.0])
linear_constraint = LinearConstraint([[1, 2], [2, 1]], [-np.inf, 1], [1, 1])

def cons_f(x):
    return [x[0]**2 + x[1], x[0]**2 - x[1]]
def cons_J(x):
    return [[2*x[0], 1], [2*x[0], -1]]
def cons_H(x, v):
    return v[0]*np.array([[2, 0], [0, 0]]) + v[1]*np.array([[2, 0], [0, 0]])

nonlinear_constraint = NonlinearConstraint(cons_f, -np.inf, 1, jac=cons_J, hess=cons_H)

x0 = np.array([0.5, 0])

res = minimize(rosen, x0, method='trust-constr', jac=rosen_der, hess=rosen_hess,
               constraints=[linear_constraint, nonlinear_constraint],
               options={'verbose': 1}, bounds=bounds)

print(res.x)

```

---

oh my - for beginners, check THIS out instead for scipy optimise if you need to solve for vectors: https://www.youtube.com/watch?v=cXHvC_FGx24

helped me immensely in my very beginner try to code some optimisation down below:

```python

from pulp import LpVariable, LpProblem, LpMinimize, value

exp1sec = LpVariable("exp1sec", 0, 1000)
exp1unsec = LpVariable("exp1unsec", 0, 1000)
exp2sec = LpVariable("exp2sec", 0, 1000)
exp2unsec = LpVariable("exp2unsec", 0, 1000)

# create problem
prob = LpProblem("myProblem", LpMinimize)
# add constraint
prob += exp1sec + exp1unsec == 160 # exp1 must add up
prob += exp2sec + exp2unsec == 90 # exp2 must add up
prob += exp1sec + exp2sec == 150 # sec must add up
prob += exp1unsec + exp2unsec == 100 # unsec must add up

# define objective
sec_weight = 0.1
unsec_weight = 1
prob += (sec_weight*exp1sec + unsec_weight*exp1unsec) + (sec_weight*exp2sec + unsec_weight*exp2unsec)
# solve
status = prob.solve()

# print results
print(f"""exp1sec = {value(exp1sec)}, exp1unsec = {value(exp1unsec)}, 
      exp2sec = {value(exp2sec)}, exp2unsec = {value(exp2unsec)}""")

```