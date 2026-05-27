This is the execution layer that transforms the High level motion trajectories (CoM pose/wrench) into joint level commands.

# State representation
#### Configuration:
Generalized pose and velocities:
$$ q = \begin{bmatrix} p_b \\ q_b \\ q_j \end{bmatrix} \ , \ \dot q = \begin{bmatrix} v_b \\ \omega_b \\ \dot q_j \end{bmatrix} $$
where:
- $p_b, v_b \in \mathbb{R}^3$ : base **position** and **linear** **velocity**
- $q_b \in \mathbb{S}^3, \omega_b \in \mathbb{R}^3$ : base **quaternion** and **angular** **velocity**
- $q_j, \dot q_j \in \mathbb{R}^{12}$ : joint **positions** and **velocities**

## Tasks
We can link task variables to joint space using [[Forward Kinematics]] 
# Main formulations

- [[A- Velocity WBC]]
- [[C- Full WBC]]

