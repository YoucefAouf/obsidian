The rigid-body dynamics of the robot are written in generalized coordinates as
$$M(q)\ddot{q} + h(q,\dot{q})= S^\top \tau + J_c^\top f \tag{FBD}$$
where $q = [q_b^\top \; q_j^\top]^\top \in \mathbb{R}^{6+n_j}$ denotes the generalized coordinates, with $q_b$ the floating-base position and orientation and $q_j$ the actuated joint coordinates, $M(q) \in \mathbb{R}^{(6+n_j)\times(6+n_j)}$ is the mass matrix, $h(q,\dot{q})$ include Coriolis, centrifugal, and gravity terms, $\tau \in \mathbb{R}^{n_j}$ is the vector of commanded joint torques, $S = [0_{n_j\times6}\; I_{n_j}]$ is the actuation selection matrix, $J_c$ is the contact Jacobian, and $f$ represents contact forces.

A more explicit formulation is:
$$ \begin{bmatrix} M_{bb} & M_{bj} \\ M_{jb} & M_{jj} \end{bmatrix} 
\begin{bmatrix} \ddot{q}_b \\ \ddot{q}_j \end{bmatrix} 
+ \begin{bmatrix} h_b \\ h_j \end{bmatrix} 
= \begin{bmatrix} 0 \\ \tau \end{bmatrix}
+ \begin{bmatrix} J^\top_{c,b} \\ J^\top_{c,j} \end{bmatrix} f$$
For a quadruped robot with 18 DoF only 12 are actuated; as a result, the system is underactuated
$$\dim{\tau} < \dim{q}$$
We can decompose the full dynamics into:
- Unactuated dynamics, corresponding to the floating-base equations  
- Actuated dynamics, corresponding to the joint-space equations.
# Unactuated dynamics
We only select the top 6 rows of the full dynamics:
$$M_{b}(q) \ddot{q} + h_{b}(q,\dot{q})= J_{c,b}^\top f_{c} $$
A more explicit formulation is:
$$ \begin{bmatrix} M_{bb} & M_{bj} \end{bmatrix} 
\begin{bmatrix} \ddot{q}_b \\ \ddot{q}_j \end{bmatrix} 
+ h_b
= J^\top_{c,b}f$$

# Actuated dynamics
We only select the bottom 12 rows of the full dynamics:
$$M_{j}(q) \ddot{q} + h_{j}(q,\dot{q})= J_{c,j}^\top f_{c} $$
A more explicit formulation is:
$$ \begin{bmatrix} M_{jb} & M_{jj} \end{bmatrix} 
\begin{bmatrix} \ddot{q}_b \\ \ddot{q}_j \end{bmatrix} 
+ h_j
= \tau
+ J^\top_{c,j} f_c$$