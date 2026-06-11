Optional task for solving redundancy, if the WBC optimal solution is far from desired pose. Usually with low weight, just to guide.
# Velocity-based
#### Desired Velocity:
$$ \dot q^{des} = K_p (q^{ref} - q) $$
where:
- $\dot q^{des} \in \mathbb{R}^{6+n}$ : desired generalized velocity profile.
- $K_p \in \mathbb{R}^{(6+n)\times(6+n)}$ : Proportional gains matrix.
- $q^{ref} \in \mathbb{R}^{6+n}$ : reference configuration (nominal standing pose, or the solution of IK).
- $q \in \mathbb{R}^{6+n}$ : current robot configuration.

#### Least square objective:
$$ {|| \dot{q} - \dot{q}^{des} ||}^2_{W_{\dot{q}}} $$
where:
- $\dot q \in \mathbb{R}^{6+n}$ : Generalized velocity.
- $W_{\dot{q}} \in \mathbb{R}^{(6+n)\times(6+n)}$ : Weight matrix for the posture regularization task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \dot{q}^\top H_{\dot{q}}  \dot{q} + g^\top_{\dot{q}} \dot{q} }$$
with:
$$ H_{\dot{q}} = W_{\dot{q}} $$
$$ g_{\dot{q}} = - W_{\dot{q}} \dot{q}^{des} $$
where:
- $H_{\dot{q}} \in \mathbb{R}^{(6+n)\times (6+n)}$ : force regularization quadratic cost matrix.
- $g_{\dot{q}} \in \mathbb{R}^{6+n}$ : force regularization linear cost vector.
- - -
# Acceleration-based
#### Desired Acceleration:
$$ \ddot q^{des} = \ddot q^{ref} + K_p (q^{ref} - q) + K_d (\dot q^{ref} - \dot q) $$
where:
- $\ddot q^{des} \in \mathbb{R}^{6+n}$ : desired generalized acceleration profile.
- $K_p, K_d \in \mathbb{R}^{(6+n)\times(6+n)}$ : proportional/derivative gains matrices.
- $q^{ref}, \dot{q}^{ref} \in \mathbb{R}^{6+n}$ : reference configuration (nominal standing pose, or the solution of IK).
- $q, \dot{q} \in \mathbb{R}^{6+n}$ : current robot configuration.

#### Least square objective:
$$ {|| \ddot q - \ddot q^{des} ||}^2_{W_{\ddot{q}}} $$
where:
- $\ddot q \in \mathbb{R}^{6+n}$ : Generalized acceleration.
- $W_{\ddot{q}} \in \mathbb{R}^{(6+n)\times(6+n)}$ : Weight matrix for the posture regularization task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \ddot{q}^\top H_{\ddot{q}}  \ddot{q} + g^\top_{\ddot{q}} \ddot{q} }$$
with:
$$ H_{\ddot{q}} = W_{\ddot{q}} $$
$$ g_{\ddot{q}} = - W_{\ddot{q}} \ddot{q}^{des} $$
where:
- $H_{\ddot{q}} \in \mathbb{R}^{(6+n)\times (6+n)}$ : force regularization quadratic cost matrix.
- $g_{\ddot{q}} \in \mathbb{R}^{6+n}$ : force regularization linear cost vector.