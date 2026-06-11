Optional task (mainly for full WBC), since base pose and com pose are different, if we have high level planners that gives us base and/or com motion then we can use this task:
# Velocity-based
#### Task space variable:
Com position:
$$ x_{com} = p_{com} \in \mathbb{R}^{3}  $$
Velocity:
$$ \dot{x}_{com} = J_{com} \dot{q} $$
where:
- $x_{com}, \dot{x}_{com} \in \mathbb{R}^3$ : com position/velocity.
- $J_{com} \in \mathbb{R}^{3\times(6+n)}$ : com translation Jacobian
#### Desired velocity:
$$ \dot{x}^{des}_{com} = \dot{x}^{ref}_{com} + K_p (x^{ref}_{com} - x_{com}) $$
where:
- $\dot{x}^{des}_{com}, \dot{x}^{ref}_{com} \in \mathbb{R}^{3}$ : desired/reference com velocity.
- $x^{ref}_{com}\in \mathbb{R}^{3}$ : reference com position.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{com} \dot{q} - \dot{x}^{des}_{com} ||}^2_{W_{com}} $$
where:
- $W_{com} \in \mathbb{R}^{3\times3}$ : Weight matrix for the com tracking task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \dot{q}^\top  H_{com} \dot{q} + g^\top_{com} \dot{q} }$$
with:
$$ H_{com} = J^\top_{com} W_{com} J_{com}$$
$$ g_{b} = - J^\top_{com} W_{com} {v^{des}_{com}}^\top$$
where:
- $H_{com} \in \mathbb{R}^{(6+n)\times (6+n)}$ : com position task quadratic cost matrix.
- $g_{com} \in \mathbb{R}^{6+n}$ : com position task linear cost vector.
- - -
# Acceleration-based
#### Task space variable:
Com pose:
$$ x_{com} = p_{com} \in \mathbb{R}^{3}  $$
Velocity:
$$ \dot{x}_{com} = J_{com} \dot{q} $$
Acceleration:
$$ \ddot{x}_{com} = J_{com} \ddot{q} + \dot{J}_{com} \dot{q}$$
where:
- $x_{com}, \dot{x}_{com}, \ddot{x}_{com} \in \mathbb{R}^{3}$ : com position/velocity/acceleration.
- $J_{com} \in \mathbb{R}^{3\times(6+n)}$ : com translation Jacobian.
#### Desired acceleration:
$$ \ddot{x}^{des}_{com} = \ddot{x}^{ref}_{com} + K_p (x^{ref}_{com} - x_{com}) + K_d (\dot{x}^{ref}_{com} - \dot{x}_{com}) $$
where:
- $\ddot{x}^{des}_{com} \in \mathbb{R}^{3}$ : desired com linear acceleration.
- $x^{ref}_{com}, \dot{x}^{ref}_{com}, \ddot{x}^{ref}_{com}\in \mathbb{R}^{3}$ : reference com position/velocity/acceleration.
- $K_p, K_d \in \mathbb{R}^{3\times3}$ : proportional/derivative gains matrices.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{com} \ddot{q} + \dot{J}_{com} \dot{q} - \ddot{x}^{des}_{com} ||}^{2}_{W_{com}} $$
where:
- $W_{com} \in \mathbb{R}^{3\times3}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \ddot{q}^\top H_{com}\ddot{q} + g^\top_{com} \ddot{q} }$$
with:
$$ H_{com} = J^\top_{com} W_{com} J_{com}$$
$$ g_{com} = J^\top_{com} W_{com} (\dot{J}_{com} \dot{q} - \ddot{x}^{des}_{com})$$
where:
- $H_{com} \in \mathbb{R}^{(6+n)\times (6+n)}$ : com position task quadratic cost matrix.
- $g_{com} \in \mathbb{R}^{6+n}$ : com position task linear cost vector.