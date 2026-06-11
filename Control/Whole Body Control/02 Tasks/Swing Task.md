# Velocity-based
#### Task space variable:
Foot swing position:
$$x_{sw} = p_{sw} = \begin{bmatrix} x \\ y \\ z \end{bmatrix} \in \mathbb{R}^{3} $$
Velocity:
$$ \dot{x}_{sw} = J_{sw} \dot{q} $$
where:
- $p_{sw} \in \mathbb{R}^{3}$ : swing foot position.
- $J_{sw} \in \mathbb{R}^{3\times(6+n)}$ : swing foot Jacobian.
#### Desired velocity:
$$ \dot x^{des}_{sw} = \dot x^{ref}_{sw} + K_p (x^{ref}_{sw} - x_{sw}) $$
where:
- $\dot x^{des}_{sw} \in \mathbb{R}^{3}$ : desired swing foot velocity.
- $x^{ref}_{sw}, \dot x^{ref}_{sw} \in \mathbb{R}^{3}$ : reference swing foot position/velocity.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{sw} \dot q - \dot x^{des}_{sw} ||}^2_{W_{sw}} $$
where:
- $W_{sw} \in \mathbb{R}^{3\times3}$ : Weight matrix for the swing foot tracking task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} z^\top  H_{sw} z + g^\top_{sw} z }$$
with:
$$ H_{sw} = J^\top_{sw} W_{sw} J_{sw}$$
$$ g_{sw} = - J^\top_{sw} W_{sw} {\dot{x}^{des}_{sw}}^\top$$
where:
- $H_{sw} \in \mathbb{R}^{3\times 3}$ : base pose task quadratic cost matrix.
- $g_{sw} \in \mathbb{R}^{3}$ : base pose task linear cost vector.
- - -
# Acceleration-based
#### Task space variable:
Foot swing position:
$$x_{sw} = p_{sw} = \begin{bmatrix} x \\ y \\ z \end{bmatrix} \in \mathbb{R}^{3} $$
Velocity:
$$ \dot{x}_{sw} = J_{sw} \dot{q} $$
Acceleration:
$$ \ddot{x}_{sw} = J_{sw} \ddot{q} + \dot{J}_{sw} \dot{q}$$
where:
- $p_{sw} \in \mathbb{R}^{3}$ : swing foot position.
- $J_{sw} \in \mathbb{R}^{3\times(6+n)}$ : swing foot Jacobian.
#### Desired acceleration:
$$ \ddot x^{des}_{sw} = \ddot x^{ref}_{sw} + K_p (x^{ref}_{sw} - x_{sw}) + K_d (\dot x^{ref}_{sw} - \dot x_{sw}) $$
where:
- $\ddot x^{des}_{sw} \in \mathbb{R}^{3}$ : desired swing foot acceleration.
- $x^{ref}_{sw}, \dot x^{ref}_{sw}, \ddot x^{ref}_{sw} \in \mathbb{R}^{3}$ : reference swing foot position/velocity/acceleration.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
- $K_d \in \mathbb{R}^{3\times3}$ : Derivative gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{sw} \ddot{q} + \dot{J}_{sw} \dot{q} - \ddot x^{des}_{sw} ||}^{2}_{W_{sw}} $$
where:
- $W_{sw} \in \mathbb{R}^{3\times3}$ : Positive semidefinite weight matrix.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} z^\top \begin{bmatrix} H_{sw} & 0 \\ 0 & 0 \end{bmatrix} z + \begin{bmatrix} g^\top_{sw} & 0 \end{bmatrix} z }$$
with:
$$ H_{sw} = J^\top_{sw} W_{sw} J_{sw}$$
$$ g_{sw} = J^\top_{sw} W_{sw} (\dot{J}_{sw} \dot{q} - \ddot{x}^{des}_{sw})$$
where:
- $H_{sw} \in \mathbb{R}^{(6+n)\times (6+n)}$ : swing foot task quadratic cost matrix.
- $g_{sw} \in \mathbb{R}^{6+n}$ : swing foot task linear cost vector.