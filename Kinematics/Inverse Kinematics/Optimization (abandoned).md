# Optimization problem
In the case where full-body inverse kinematics (IK) to achieve the desired center-of-mass (CoM) pose $\mathbf{x}$ results in an infeasible configuration or lies outside the leg workspace, we solve the following optimization problem.
The optimization state consists of the CoM position and orientation:

$$
    \tag{eq:state.definition}
    \mathbf{x} = 
    \begin{bmatrix}
    p_{com}^{B} \
    \eta
    \end{bmatrix} =
    \begin{bmatrix}
    x & y & z & \phi & \theta & \psi
    \end{bmatrix}^\top
    \in \mathbb{R}^6
$$
where $p_{com}^{B} \in \mathbb{R}^3$  denotes the CoM position expressed in frame B, and
$\eta = \begin{bmatrix} \phi , \theta , \psi \end{bmatrix} \in \mathbb{R}^3$ denotes the CoM orientation parameterized by Euler angles.

# Cost Function
We define a weighted quadratic task-space cost:
$$
    \tag{eq:com.cost}
    \min_{\mathbf{x}} \, J(\mathbf{x}) = 
    \frac{1}{2}
    (\mathbf{x} - \mathbf{x}_d)^\top
    \mathbf{W}
    (\mathbf{x} - \mathbf{x}_d)
$$
where $\mathbf{W} \in \mathbb{R}^{6\times6}$ is a symmetric positive-definite task-space weighting matrix.

## Cost gradient
The gradient of the cost with respect to the task-space state 
$$
    \nabla J(\mathbf{x}) = 
    \frac{\partial J(\mathbf{x})}{\partial \mathbf{x}} = 
    \mathbf{W}(\mathbf{x} - \mathbf{x}_d)
    \in \mathbb{R}^6
$$

## Constraints
Each leg $i=0,1,2,3$, has three actuated joints, indexed by $j=0,1,2$:
$$
\tag{eq:joint.limits}
q_{j,\\min} \leq q_{i,j} \leq q_{j,\max}
$$
The relation between $q_i$ and $\mathbf{x}$ is given by \ref{eq:IK.numerical} , \ref{eq:pfH}:
$$
    q_i = \mathbf{IK}(p_{f,i}^{H})
$$
$$
    p_{f,i}^{H} = R^\top (p_{f,i}^{B} - p_{com}^{B}) - p_{h,i} \tag{eq:baseToHipFootTransform}
$$
with $R \in SO(3)$ the rotation matrix corresponding to the orientation $\eta$.


## Constraints Jacobian
$$
    J_i = \frac{\partial q_i}{\partial \mathbf{x}} = \begin{bmatrix}
        J_{i,p}, J_{i, \eta}
    \end{bmatrix}
$$

### Position Jacobian
$$
    J_{i,p} = \frac{\partial q_i}{\partial p_{f,i}^{H}}  .  \frac{\partial p_{f,i}^{H}}{\partial p_{com}^{B}}
$$
From \ref{eq_ikJacobian} and \ref{eq:baseToHipFootTransform}:
$$
    \frac{\partial q_i}{\partial p_{f,i}^{H}} = J_i^{-1}(p_{f,i}^{H}) \quad , \quad \frac{\partial p_{f,i}^{H}}{\partial p_{com}^{B}} = -R^\top
$$

### Orientation Jacobian
$$
    J_{i, \eta} = \frac{\partial q_i}{\partial p_{f,i}^{H}}  .  \frac{\partial p_{f,i}^{H}}{\partial \eta}
$$
From \ref{eq:baseToHipFootTransform}:
$$
    \frac{\partial p_{f,i}^{H}}{\partial \eta} = \Big(\frac{\partial R}{\partial \eta}\Big)^\top d \quad , \quad d = p_{f,i}^{B} - p_{com}^{B}
$$
Let: $c_\phi = \cos(\phi), s_\phi = \sin(\phi), \quad c_\theta = \cos(\theta), s_\theta = \sin(\theta), \quad c_\psi = \cos(\psi), s_\psi = \sin(\psi)$
$$
R = \begin{bmatrix}
c_\psi c_\theta & c_\psi s_\theta s_\phi - s_\psi c_\phi & c_\psi s_\theta c_\phi + s_\psi s_\phi \\
s_\psi c_\theta & s_\psi s_\theta s_\phi + c_\psi c_\phi & s_\psi s_\theta c_\phi - c_\psi s_\phi \\
-s_\theta & c_\theta s_\phi & c_\theta c_\phi
\end{bmatrix} \tag{eq:rotationMatrix}
$$
In code we construct the 3×3 orientation Jacobian per leg as
$$
\frac{\partial p_{f,i}^{H}}{\partial \eta}=
\big[\,(\partial R/\partial\phi)^\top d,\; (\partial R/\partial\theta)^\top d,\; (\partial R/\partial\psi)^\top d\,\big],
$$
i.e. each column is the derivative with respect to one Euler angle.
$$
\frac{\partial R}{\partial\phi} = 
\begin{bmatrix}
0 & s_{\phi}s_{\psi} + s_{\theta}c_{\phi}c_{\psi} & -s_{\phi}s_{\theta}c_{\psi} + s_{\psi}c_{\phi}\\[4pt]
0 & -s_{\phi}c_{\psi} + s_{\psi}s_{\theta}c_{\phi} & -s_{\phi}s_{\psi}s_{\theta} - c_{\phi}c_{\psi}\\[4pt]
0 & c_{\phi}c_{\theta} & -s_{\phi}c_{\theta}
\end{bmatrix}
$$
$$
\frac{\partial R}{\partial\theta} =
\begin{bmatrix}
- s_{\theta}c_{\psi} & s_{\phi}c_{\psi}c_{\theta} & c_{\phi}c_{\psi}c_{\theta}\\[4pt]
- s_{\psi} s_{\theta} & s_{\phi}s_{\psi}c_{\theta} & s_{\psi}c_{\phi}c_{\theta}\\[4pt]
- c_{\theta} & - s_{\phi}s_{\theta} & - s_{\theta} c_{\phi}
\end{bmatrix}
$$
$$
    \frac{\partial R}{\partial\psi} =
\begin{bmatrix}
- s_{\psi} c_{\theta} & - s_{\phi}s_{\psi}s_{\theta} - c_{\phi}c_{\psi} & s_{\phi}c_{\psi} - s_{\psi}s_{\theta}c_{\phi} \\[4pt]
c_{\psi} c_{\theta} & s_{\phi}s_{\theta}c_{\psi} - s_{\psi}c_{\phi} & s_{\phi}s_{\psi} + s_{\theta}c_{\phi}c_{\psi} \\[4pt]
0 & 0 & 0
\end{bmatrix}
$$


# Solver
we use the C++ library IFOPT, which is a wrapper of IPOPT
