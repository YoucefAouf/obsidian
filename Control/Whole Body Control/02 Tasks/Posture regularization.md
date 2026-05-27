Optional task for solving redundancy, if the WBC optimal solution is far from desired pose. Usually with low weight, just to guide.
Desired Velocity:
$$ \dot q^{des}_{post} = K_p (q^{ref} - q) $$
where:
- $\dot q^{des}_{post} \in \mathbb{R}^{6+12}$ : desired generalized velocity profile.
- $K_p \in \mathbb{R}^{(6+12)\times(6+12)}$ : Proportional gains matrix.
- $q^{ref} \in \mathbb{R}^{6+12}$ : reference configuration (nominal standing pose, or the solution of IK).
- $q \in \mathbb{R}^{6+12}$ : current robot configuration.

Cost:
$$ {|| \dot q - \dot q^{des}_{post} ||}^2_{W_{post}} $$
where:
- $\dot q \in \mathbb{R}^{6+12}$ : Generalized velocity.
- $W_{post} \in \mathbb{R}^{(6+12)\times(6+12)}$ : Weight matrix for the posture regularization task.
