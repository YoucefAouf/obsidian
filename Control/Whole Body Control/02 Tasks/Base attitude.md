# Velocity-based
#### Task space variable: 
Angular velocity:
$$ \omega_{b} = J_{\omega} \dot{q} $$
where:
- $\omega_{b} \in \mathbb{R}^3$ : base angular velocity.
- $J_{\omega} \in \mathbb{R}^{3\times(6+n)}$ : base orientation Jacobian.
#### Desired velocity:
$$ \omega^{des}_{b} = \omega^{ref}_{b} + K_p e_{R} $$
with:
$$ e_{R} = \text{Log}(R^{ref} R^\top)^\vee $$
where:
- $\omega^{des}_{b}, \omega^{ref}_{b} \in \mathbb{R}^{3}$ : desired/reference base angular velocity.
- $e_{R}\in \mathbb{R}^{3}$ : base orientation error.
- $R, R^{ref}\in SO3$ : current/reference base orientation as rotation matrix.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{\omega} \dot q - \omega^{des}_b ||}^2_{W_{\omega}} $$
where:
- $W_{\omega} \in \mathbb{R}^{3\times3}$ : Weight matrix for the base attitude tracking task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \dot{q}^\top  H_{\omega} \dot{q} + g^\top_{\omega} \dot{q} }$$
with:
$$ H_{\omega} = J^\top_{\omega} W_{\omega} J_{\omega}$$
$$ g_{\omega} = - J^\top_{\omega} W_{\omega} {\omega^{des}_{b}}^\top$$
where:
- $H_{\omega} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base pose task quadratic cost matrix.
- $g_{\omega} \in \mathbb{R}^{6+n}$ : base pose task linear cost vector.
- - -
# Acceleration-based
#### Task space variable:
Angular velocity:
$$ \omega_{b} = J_{\omega} \dot{q} $$
Angular acceleration:
$$ \dot \omega_{b} = J_{\omega} \ddot{q} + \dot{J}_{\omega} \dot{q}$$
where:
- $\omega_{b} \in \mathbb{R}^{3}$ : base angular velocity.
- $J_{\omega} \in \mathbb{R}^{3\times(6+n)}$ : base angular Jacobian.
#### Desired acceleration:
$$ \dot \omega^{des}_{b} = \dot \omega^{ref}_{b} + K_p e_{R} + K_d (\omega^{ref}_{b} - \omega_{b}) $$
with:
$$ e_{R} = \text{Log}(R^{ref} R^\top)^\vee $$
where:
- $\dot \omega^{des}_{b}, \dot \omega^{ref}_{b} \in \mathbb{R}^{3}$ : desired/reference base angular acceleration.
- $\omega^{ref}_{b}\in \mathbb{R}^{3}$ : reference base angular velocity.
- $e_{R} \in \mathbb{R}^{3}$ : base orientation error.
- $R, R^{ref}\in SO3$ : current/reference base orientation as rotation matrix.
- $K_p \in \mathbb{R}^{3\times3}$ : Proportional gains matrix.
- $K_d \in \mathbb{R}^{3\times3}$ : Derivative gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{\omega} \ddot{q} + \dot{J}_{\omega} \dot{q} - \dot \omega^{des}_{\omega} ||}^{2}_{W_{\omega}} $$
where:
- $W_{\omega} \in \mathbb{R}^{3\times3}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \ddot{q}^\top H_{\omega} \ddot{q} + g^\top_{\omega} \ddot{q} }$$
with:
$$ H_{\omega} = J^\top_{\omega} W_{\omega} J_{\omega}$$
$$ g_{\omega} = J^\top_{\omega} W_{\omega} (\dot{J}_{\omega} \dot{q} - \dot{\omega}^{des}_{b})$$
where:
- $H_{\omega} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base pose task quadratic cost matrix.
- $g_{\omega} \in \mathbb{R}^{6+n}$ : base pose task linear cost vector.