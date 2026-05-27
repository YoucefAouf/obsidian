Optional task (mainly for full WBC), since trunk pose and com pose are different, if we have high level planners that gives us both trunk and com motion then we can use this task:
Cost:
$$ {|| J_{com} \ddot{q} + \dot{J}_{com} \dot{q} - \ddot x^{des}_{com} ||}^2_{W_{com}} $$
where:
- $J_{com} \in \mathbb{R}^{3\times(6+12)}$ : com Jacobian.
- $\dot q \in \mathbb{R}^{6+12}$ : generalized velocity.
- $\ddot x^{des}_{com} \in \mathbb{R}^{3}$ : desired com position.
- $W_{com} \in \mathbb{R}^{3\times3}$ : Weight matrix for the com tracking task
