# Velocity-based
#### Task space variable:
Base position:
$$ x_{b} = p_{b} \in \mathbb{R}^{3}  $$
Velocity:
$$ \dot{x}_{b} = J_{b} \dot{q} $$
where:
- $x_b, \dot{x}_b \in \mathbb{R}^3$ : base position/velocity.
- $J_{b} \in \mathbb{R}^{3\times(6+n)}$ : base translation Jacobian.
#### Desired velocity:
$$ \dot{x}^{des}_{b} = \dot{x}^{ref}_{b} + K_p (x^{ref}_{b} - x_{b}) $$
where:
- $\dot{x}^{des}_{b}, \dot{x}^{ref}_{b} \in \mathbb{R}^{3}$ : desired/reference base velocity.
- $x^{ref}_{b}\in \mathbb{R}^{3}$ : reference base position.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_b \dot{q} - \dot{x}^{des}_b ||}^2_{W_{b}} $$
where:
- $W_{b} \in \mathbb{R}^{3\times3}$ : Weight matrix for the base tracking task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \dot{q}^\top  H_{b} \dot{q} + g^\top_{b} \dot{q} }$$
with:
$$ H_{b} = J^\top_{b} W_{b} J_{b}$$
$$ g_{b} = - J^\top_{b} W_{b} {\dot{x}^{des}_{b}}^\top$$
where:
- $H_{b} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base position task quadratic cost matrix.
- $g_{b} \in \mathbb{R}^{6+n}$ : base position task linear cost vector.
- - -
# Acceleration-based
#### Task space variable:
Base position:
$$ x_{b} = p_{b} \in \mathbb{R}^{3}  $$
Velocity:
$$ \dot{x}_{b} = J_{b} \dot{q} $$
Acceleration:
$$ \ddot{x}_{b} = J_{b} \ddot{q} + \dot{J}_{b} \dot{q}$$
where:
- $x_{b}, \dot{x}_{b}, \ddot{x}_{b} \in \mathbb{R}^{3}$ : base position/velocity/acceleration.
- $J_{b} \in \mathbb{R}^{3\times(6+n)}$ : base translation Jacobian.
#### Desired acceleration:
$$ \ddot{x}^{des}_{b} = \ddot{x}^{ref}_{b} + K_p (x^{ref}_{b} - x_{b}) + K_d (\dot{x}^{ref}_{b} - \dot{x}_{b}) $$
where:
- $\ddot{x}^{des}_{b} \in \mathbb{R}^{3}$ : desired base linear acceleration.
- $x^{ref}_{b}, \dot{x}^{ref}_{b}, \ddot{x}^{ref}_{b}\in \mathbb{R}^{3}$ : reference base position/velocity/acceleration.
- $K_p, K_d \in \mathbb{R}^{3\times3}$ : proportional/derivative gains matrices.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{b} \ddot{q} + \dot{J}_{b} \dot{q} - \ddot{x}^{des}_{b} ||}^{2}_{W_{b}} $$
where:
- $W_{b} \in \mathbb{R}^{3\times3}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \ddot{q}^\top H_{b}\ddot{q} + g^\top_{b} \ddot{q} }$$
with:
$$ H_{b} = J^\top_{b} W_{b} J_{b}$$
$$ g_{b} = J^\top_{b} W_{b} (\dot{J}_{b} \dot{q} - \ddot{x}^{des}_{b})$$
where:
- $H_{b} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base position task quadratic cost matrix.
- $g_{b} \in \mathbb{R}^{6+n}$ : base position task linear cost vector.