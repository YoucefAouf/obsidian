# Velocity-based
#### Task space variable:
Base height:
$$ x_{z} = z_b \in \mathbb{R}  $$
Velocity:
$$ \dot x_{z} = J_{z} \dot{q} $$
where:
- $x_{z}, \dot x_{z} \in \mathbb{R}$ : base height/velocity in $z$ direction.
- $J_{z} \in \mathbb{R}^{1\times(6+n)}$ : base height Jacobian.
#### Desired velocity:
$$ \dot x^{des}_{z} = \dot x^{ref}_{z} + k_p (x^{ref}_z - x_{z}) $$
where:
- $\dot x^{des}_{z}, \dot x^{ref}_{z} \in \mathbb{R}$ : desired/reference base velocity in $z$ direction.
- $x^{ref}_{z}\in \mathbb{R}$ : reference base height.
- $k_p \in \mathbb{R}$ : Proportional gain.
#### Least squares objective:
$$ \frac{1}{2} {|| J_z \dot q - \dot x^{des}_z ||}^2_{w_{z}} $$
where:
- $w_{z} \in \mathbb{R}$ : Weight for the base height tracking task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \dot{q}^\top  H_{z} \dot{q} + g^\top_{z} \dot{q} }$$
with:
$$ H_{z} = J^\top_{z} w_{z} J_{z}$$
$$ g_{z} = - J^\top_{z} w_{z} \dot{x}^{des}_{z}$$
where:
- $H_{z} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base height task quadratic cost matrix.
- $g_{z} \in \mathbb{R}^{6+n}$ : base height task linear cost vector.
- - -
# Acceleration-based
#### Task space variable:
Base height:
$$ x_{z} = z_b \in \mathbb{R}  $$
Velocity:
$$ \dot x_{z} = J_{z} \dot{q} $$
Acceleration:
$$ \ddot{x}_{z} = J_{z} \ddot{q} + \dot{J}_{z} \dot{q}$$
where:
- $x_{z}, \dot x_{z}, \ddot x_{z} \in \mathbb{R}$ : base height/velocity/acceleration in $z$ direction.
- $J_{z} \in \mathbb{R}^{1\times(6+n)}$ : base height Jacobian.
#### Desired acceleration:
$$ \ddot x^{des}_{z} = \ddot x^{ref}_{z} + k_p (x^{ref}_{z} - x_{z}) + k_d (\dot x^{ref}_{z} - \dot x_{z}) $$
where:
- $\dot x^{des}_{z}, \dot x^{ref}_{z} \in \mathbb{R}$ : desired/reference base acceleration in $z$ direction.
- $x^{ref}_{z}, \dot x^{ref}_{z}\in \mathbb{R}$ : reference base height/velocity in $z$ direction.
- $k_p, k_d \in \mathbb{R}$ : proportional/derivative gain.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{z} \ddot{q} + \dot{J}_{z} \dot{q} - \ddot x^{des}_{z} ||}^{2}_{w_{z}} $$
where:
- $w_{z} \in \mathbb{R}$ : Weight for the base height tracking task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \ddot{q}^\top H_{z} \ddot{q} + g^\top_{z} \ddot{q} }$$
with:
$$ H_{z} = J^\top_{z} w_{z} J_{z}$$
$$ g_{z} = J^\top_{z} w_{z} (\dot{J}_{z} \dot{q} - \ddot{x}^{des}_{z})$$
where:
- $H_{z} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base pose task quadratic cost matrix.
- $g_{z} \in \mathbb{R}^{6+n}$ : base pose task linear cost vector.