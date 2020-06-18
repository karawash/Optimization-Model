# Optimization-Model

A restaurant prepares salad dishes. Two of these salads are Greek salad and Italian salad. Greek salad earns a profit of $5 per unit and Italian salad earns $8 per unit. Three major resources are utilized in the production process: vegetable, labor, and cutting time. It takes 1/2 lbs of vegetables to make a Greek salad, and 1/3 lbs to make a Italian salad. Greek salad requires 1 labor; Italian salad requires 1 labor. It takes 1/4 hr cutting time per Greek salad, and 1/5 hr per Italian salad. For the current planning period, 500 lbs vegetable, 800 hour labor, and 400 hour cutting time are available. The restaurant would like to maximize profit during the current planning period, within allowable resources.

## Formulation
Decision Variables: Number of Greek salad to make, number of Italian salad to make
Objective Function: Maximize profit
Constraints: Must not exceed resource availability in vegetable, labor, and cutting time hour

### Decision Variables
X number of Greek salad to make

Y number of Italian salad to make

### Objective function
The objective function is simply an expression that determines how much profit will be earned if X Greek salad is made and Y Italian salad is made. We also indicate whether we want to maximize ("Max") or minimize ("Min") the objective function.

#### Max 5 X + 8 Y (profit, $)

### Constraints
Finally, we need to write expressions for the constraints. In this example, we are limited by the amounts of the resources - vegetable, labor, and cutting time - available. For vegetables, we know that 5000 lbs are available. We also know that each Greek salad unit uses 1/2 lbs, and each Italian uses 1/3 lbs. Therefore, if we make X and Y Greek and Italian salad, respectively, it will require 1/2 X + 1/3 Y lbs of vegetable. In order for this product mix to be possible, this combined total must not exceed 500 lbs. We write this in mathematical terms as follows: 1/2 X + 1/3 Y <= 500 (vegetable, lbs). Using similar logic, we write the constraints for labor and cutting time as follows: 1 X + 1 Y <= 800 (labor, hour), 1/4 X + 1/5 Y <= 400 (cutting time, hour). Sure we should not forget to have non negative constraints so add X >= 0 and Y >= 0

## Voila! Build A Complete Formula

### Decision Variables
 X number of Greek salad to make
 
 Y number of Italian salad to make
 
 Max 5 X + 8 Y (profit, $)

### Subject to
 1/2 X + 1/3 Y <= 500 (vegetable, lbs)
 
 1 X + 1 Y <= 800 (labor, hour)
 
 1/4 X + 1/5 Y <= 400 (cutting time, hour)
 
 X, Y >= 0


## Let us optimize model in python using Scipy

### use python libraries numpy and scipy
 import numpy as np
 
 from scipy.optimize import minimize

### Decision Variables
 Let x[0] represents X (number of Greek salad to make)
 
 Let x[1] represents Y (number of Italian salad to make)

### Objective
#### Max 5X + 8Y (profit, $)
 def objective(x, sign=1.0):
 
 return sign*(5*x[0] + 8*x[1])

### Derivative of objective
 def objective_deriv(x, sign=1.0):
 
   #### Derivative on x[0]
 
   dfdx0 = sign*(5)
 
   #### Derivative on x[1]
 
   dfdx1 = sign*(8)
 
   return np.array([ dfdx0, dfdx1 ])
 

### Subject to
#### Constraint1
#### 1/2 X + 1/3 Y <= 500 (vegetable, lbs)
 def constraint1(x):
 
 return 1/2*x[0] + 1/3*x[1] - 500.0

#### Constraint2
#### 1 X + 1 Y <= 800 (labor, hour)
 def constraint2(x):
 
 return 1*x[0] + 1*x[1] - 800.0

#### Constraint3
#### 1/4 X + 1/5 Y <= 400 (cutting time, hour)
 def constraint3(x):
 
 return 1/3*x[0] + 1/5*x[1] - 400.0

#### initial guesses choose number >=0
 n = 2
 
 x0 = np.zeros(n)
 
 x0[0] = 1.0
 
 x0[1] = 2.0

#### show initial objective
 print('Initial Objective: ' + str(objective(x0)))

#### bounds >=0 (I used <=100 as an example)
 b = (0.0,None)
 
 bnds = (b, b)

#### merge constraints objects
 con1 = {'type': 'ineq', 'fun': constraint1}
 
 con2 = {'type': 'ineq', 'fun': constraint2}
 
 con3 = {'type': 'ineq', 'fun': constraint3}
 
 cons = ([con1,con2,con3])

#### optimize
#### for minimization keep args=(1.0,)
#### for maximization use args=(-1.0,)
 solution = minimize(objective,x0,args=(-1.0,), jac=objective_deriv, method='SLSQP', bounds=bnds,constraints=cons)
 x = solution.x

#### show final objective
 print('Final Objective: ' + str(objective(x)))

#### print solution
 print('Solution')
 
 print('X = ' + str(x[0]))
 
 print('Y = ' + str(x[1]))
 
