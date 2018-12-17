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

import numpy as np
from scipy.optimize import minimize

def obj(x):
    """The objective function"""
    return (x[0]*100 + x[1]*0) + (x[2]*60 + x[3]*90)

def constraint1(x):
    return x[0]*100 + x[1]*0 - 100

def constraint2(x):
    return x[2]*60 + x[3]*90 - 150


b = (0, 1)
bnds = (b, b, b, b)
con1 = {'type':'eq', 'fun': constraint1}
con2 = {'type':'eq', 'fun': constraint2}
cons = [con1, con2]

x0 = np.array([0, 0, 0, 0]) #initial weights

sol = minimize(obj, x0, method='SLSQP', 
               bounds=bnds, constraints=cons)

print(sol.x)

```