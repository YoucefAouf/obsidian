
---
# Floating base system
#### Generalized coordinates:
$$ q = \begin{bmatrix} p_b \\ q_b \\ q_j \end{bmatrix} \in \mathbb{R}^{3}\times \mathbb{S}^3 \times \mathbb{R}^{n}$$
where:
- $p_b \in \mathbb{R}^3$ : base position.
- $q_b \in \mathbb{S}^3$ : base quaternion.
- $q_j \in \mathbb{R}^{n}$ : joint positions.
- $n = 12$ : actuated joints.
$$ \dot q = \begin{bmatrix} v_b \\ \omega_b \\ \dot q_j \end{bmatrix} \in \mathbb{R}^{6+n}$$
where:
- $v_b \in \mathbb{R}^3$ : base linear velocity.
- $\omega_b \in \mathbb{R}^3$ : base angular velocity.
- $\dot q_j \in \mathbb{R}^{n}$ : joint velocities.
#### Optimization variables:
$$ x = \begin{bmatrix} \ddot q \\ f_c \\ \tau \end{bmatrix} \in \mathbb{R}^{d}$$
where:
- $\ddot q \in \mathbb{R}^{6+n}$ : generalized acceleration
- $f_c \in \mathbb{R}^{n_{st}}$ : contact forces.
- $\tau \in \mathbb{R}^{n}$ : joint torque
- $d = (6 + n) + n_{st} + n$: dimension of the optimization variables.

---
# Tasks
## Base pose task
#### Task space variable:
Base pose:
$$x_{b} = \begin{bmatrix} p_{b} \\ \Theta_{b} \end{bmatrix} \in \mathbb{R}^{6} $$
Velocity:
$$ \dot{x}_{b} = J_{b} \dot{q} $$
Acceleration:
$$ \ddot{x}_{b} = J_{b} \ddot{q} + \dot{J}_{b} \dot{q}$$
where:
- $p_{b} \in \mathbb{R}^{3}$ : base position
- $\Theta_{b} \in \mathbb{R}^{3}$ : Euler angle representation of the base orientation
- $J_{b} \in \mathbb{R}^{6\times(6+n)}$ : base Jacobian.
#### Desired acceleration:
$$ \ddot x^{des}_{b} = \ddot x^{ref}_{b} + K_p (x^{ref}_{b} - x_{b}) + K_d (\dot x^{ref}_{b} - \dot x_{b}) $$
where:
- $\ddot x^{des}_{b} \in \mathbb{R}^{6}$ : desired base acceleration.
- $x^{ref}_{b}, \dot x^{ref}_{b}, \ddot x^{ref}_{b} \in \mathbb{R}^{6}$ : reference base position/velocity/acceleration.
- $K_p \in \mathbb{R}^{6\times6}$ : Proportional gains matrix.
- $K_d \in \mathbb{R}^{6\times6}$ : Derivative gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{b} \ddot{q} + \dot{J}_{b} \dot{q} - \ddot x^{des}_{b} ||}^{2}_{W_{b}} $$
where:
- $W_{b} \in \mathbb{R}^{6\times6}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} x^\top \begin{bmatrix} H_{b} & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} x + \begin{bmatrix} g_{b}^\top & 0 & 0 \end{bmatrix} x }$$
with:
$$ H_{b} = J^\top_{b} W_{b} J_{b}$$
$$ g_{b} = J^\top_{b} W_{b} (\dot{J}_{b} \dot{q} - \ddot{x}^{des}_{b})$$
where:
- $H_{b} \in \mathbb{R}^{(6+n)\times (6+n)}$ : base pose task quadratic cost matrix.
- $g_{b} \in \mathbb{R}^{6+n}$ : base pose task linear cost vector.
## Swing foot task
#### Task space variable:
Foot swing position:
$$x_{sw} = p_{sw} = \begin{bmatrix} x \\ y \\ z \end{bmatrix} \in \mathbb{R}^{3} $$
Velocity:
$$ \dot{x}_{sw} = J_{sw} \dot{q} $$
Acceleration:
$$ \ddot{x}_{sw} = J_{sw} \ddot{q} + \dot{J}_{sw} \dot{q}$$
where:
- $p_{sw} \in \mathbb{R}^{3}$ : swing foot position
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
- $W_{sw} \in \mathbb{R}^{3\times3}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} x^\top \begin{bmatrix} H_{sw} & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} x + \begin{bmatrix} g^\top_{sw} & 0 & 0 \end{bmatrix} x }$$
with:
$$ H_{sw} = J^\top_{sw} W_{sw} J_{sw}$$
$$ g_{sw} = J^\top_{sw} W_{sw} (\dot{J}_{sw} \dot{q} - \ddot{x}^{des}_{sw})$$
where:
- $H_{sw} \in \mathbb{R}^{(6+n)\times (6+n)}$ : swing foot task quadratic cost matrix.
- $g_{sw} \in \mathbb{R}^{6+n}$ : swing foot task linear cost vector.
## Force regularization
#### Least squares objective:
$$ \frac{1}{2} {|| f_{c} ||}^2_{W_{f}} $$
where:
- $W_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : Weight matrix for the force regularization task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} x^\top \begin{bmatrix} 0 & 0 & 0 \\ 0 & H_{f} & 0 \\ 0 & 0 & 0\end{bmatrix} x + \begin{bmatrix} 0 & g^\top_{f} & 0 \end{bmatrix} x }$$
with:
$$ H_{f} = W_{f} $$
$$ g_{f} = 0_{n_{st}\times 1} $$
where:
- $H_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : force regularization quadratic cost matrix.
- $g_{f} \in \mathbb{R}^{n_{st}}$ : force regularization linear cost vector.
## Torque regularization
#### Least squares objective:
$$ \frac{1}{2} {|| \tau ||}^2_{W_{\tau}} $$
where:
- $W_{\tau} \in \mathbb{R}^{n\times n}$ : Weight matrix for the torque regularization task
#### Expanded QP form:
$$\boxed{ \frac{1}{2} x^\top \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & H_{\tau} \end{bmatrix} x + \begin{bmatrix} 0 & 0 & g^\top_{\tau} \end{bmatrix} x }$$
with:
$$ H_{\tau} = W_{\tau} $$
$$ g_{\tau} = 0_{n\times 1} $$
where:
- $H_{\tau} \in \mathbb{R}^{n\times n}$ : torque regularization quadratic cost matrix.
- $g_{\tau} \in \mathbb{R}^{n}$ : torque regularization linear cost vector.
---
# Constraints:
## Floating base dynamics constraint
The core of full WBC, [[Floating base dynamics]] represent the coupling between optimization variables:
$$M(q)\ddot{q} + h(q,\dot{q})= S^\top \tau + J_c^\top f $$
$$ M(q) \ddot{q} - J_{c}^\top f_{c} - S^\top \tau=  -h(q,\dot{q}) $$
#### QP equality form:
$$\boxed{ A_{fb} x = b_{fb} }$$
with:
$$ A_{fb} = \begin{bmatrix} M(q) \ , \ -J_{c}^\top \ , \ -S^\top \end{bmatrix}$$
$$ b_{fb} = -h(q,\dot{q})$$
where:
- $A_{fb} \in \mathbb{R}^{(6+n)\times d}$ : floating base dynamics equality constraint matrix.
- $b_{fb} \in \mathbb{R}^{6+n}$ : floating base dynamics equality constraint vector.
### Stance acceleration
$$ \ddot{x}^{des}_{st} = J_{st} \ddot{q} + \dot{J}_{st} \dot{q} = 0 $$
where:
- $\ddot x^{des}_{st} \in \mathbb{R}^{n_{st}}$ : desired stance feet acceleration.
- $J_{st} \in \mathbb{R}^{n_{st}\times(6+n)}$ : stance feet Jacobian.
Rearrange terms:
$$J_{st} \ddot{q} = - \dot{J}_{st} \dot{q}$$
Could also be a soft constraint: ${|| J_{st} \ddot{q} + \dot{J}_{st} \dot{q} ||}^2_{W_{st}}$ , for mud or whenever contacts aren't fixed
**QP equality form:**
$$\boxed{ A_{st} x = b_{st} }$$
with:
$$ A_{st} = \begin{bmatrix} J_{st} & 0 & 0 \end{bmatrix}$$
$$ b_{st} = - \dot{J}_{st} \dot{q}$$
where:
- $A_{st} \in \mathbb{R}^{n_{st}\times d}$ : stance equality constraint matrix.
- $b_{st} \in \mathbb{R}^{n_{st}}$ : stance equality constraint vector.
### Torque limits
Limits are represented in [[Joint limits#Torque limits]]:
$$ \tau_{min} \le \tau \le \tau_{max} $$
**QP inequality form:**
$$\boxed{ l_{\tau} \le C_{\tau} {x} \le u_{\tau} }$$
with:
$$ C_{\tau} = \begin{bmatrix} 0 & 0 & I_{n\times n} \end{bmatrix}$$
$$ l_{\tau} = \tau_{min}$$
$$ u_{\tau} = \tau_{max}$$
where:
- $C_{\tau} \in \mathbb{R}^{n\times d}$ : torque inequality constraint matrix.
- $l_{\tau} \in \mathbb{R}^{n}$ : torque lower inequality bounds.
- $u_{\tau} \in \mathbb{R}^{n}$ : torque upper inequality bounds.
### Joint limits
We constraint joint velocities from both [[Joint limits#Velocity limits]] and [[Joint limits#Position limits]]:
We use discrete time updates assuming constant velocity and acceleration in $\Delta t$:
$$ \dot q = \frac{q(k+1) - q(k)}{\Delta t} \ , \ \ddot q = \frac{\dot q(k+1) - \dot q(k)}{\Delta t}$$
We can substitute $q(k+1)$ with upper and lower limits:
$$ \frac{q_{min} - q(k)}{\Delta t} \le \dot q \le \frac{q_{max} - q(k)}{\Delta t} $$
Therefore, the velocity limits are:
$${\dot q_{min}^* = \max \left[ \dot q_{min}, \frac{q_{min} - q(k)}{\Delta t} \right] }$$
$${\dot q_{max}^* = \min \left[ \dot q_{max}, \frac{q_{max} - q(k)}{\Delta t} \right]}$$
Similarly for $\dot q(k+1)$, we can obtain the joint acceleration limits:
$$ \ddot q_{j,min} = \frac{\dot q_{j,min}^* - \dot q_j(k)}{\Delta t} \le \ddot q_j \le \frac{\dot q_{j,max}^* - \dot q_j(k)}{\Delta t} = \ddot q_{j,max}$$
**QP inequality form:**
$$\boxed{ l_{j} \le C_j {x} \le u_{j} }$$
with:
$$ C_{j} = \begin{bmatrix}  S & 0 & 0 \end{bmatrix}$$
$$ l_{j} = \ddot q_{j,min}$$
$$ u_{j} = \ddot q_{j,max}$$
where:
- $S \in \mathbb{R}^{n\times (6+n)}$ : selection matrix, same as in [[Floating base dynamics]].
- $C_{j} \in \mathbb{R}^{n\times d}$ : joint acceleration inequality constraint matrix.
- $l_{j} \in \mathbb{R}^{n}$ : joint acceleration lower inequality bounds.
- $u_{j} \in \mathbb{R}^{n}$ : joint acceleration upper inequality bounds.


---
# Full optimization problem (QP form)
Finally we have:
$$\tag{QP} \begin{aligned} \min_{x} & \quad \frac{1}{2}x^{T}Hx+g^{T}x \\ \text{s.t.}&\left\{ \begin{array}{ll} Ax = b, \\ l \leq Cx \leq u.\\ \end{array} \right. \end{aligned} $$
with:
$$
\begin{aligned}
H &= 
\begin{bmatrix}
H_{b} + H_{sw} & 0 & 0 \\
0 & H_{f} & 0 \\
0 & 0 & H_{\tau}
\end{bmatrix},
&
g &=
\begin{bmatrix}
g_{sw} + g_{b} \\
g_{f} \\
g_{\tau}
\end{bmatrix}
\\[1em]
A &=
\begin{bmatrix}
A_{fb} \\
A_{st}
\end{bmatrix},
&
b &=
\begin{bmatrix}
b_{fb} \\
b_{st}
\end{bmatrix}
\\[1em]
C &= 
\begin{bmatrix}
C_{j} \\
C_{\tau}
\end{bmatrix},
&
l &= 
\begin{bmatrix}
l_{j} \\
l_{\tau}
\end{bmatrix},
&
u &= 
\begin{bmatrix}
u_{j} \\
u_{\tau}
\end{bmatrix}
\end{aligned}
$$
where:
- $n_{eq} = 6+n_{st}$
- $n_{in} = n + n$
- $H \in \mathbb{R}^{d\times d}$ : Hessian matrix.
- $g \in \mathbb{R}^{d}$ : gradient vector
- $A \in \mathbb{R}^{n_{eq}\times d}$ : equality constraint matrix.
- $b \in \mathbb{R}^{n_{eq}}$ : equality constraint vector.
- $C \in \mathbb{R}^{n_{in}\times d}$ : inequality constraint matrix.
- $l \in \mathbb{R}^{n_{in}}$ : lower inequality bounds.
- $u \in \mathbb{R}^{n_{in}}$ : upper inequality bounds.
---
# Controller Pipeline

Torque-level WBC solves for the desired generalized accelerations, contact forces, and actuator torques:

$$
x^* =
\begin{bmatrix}
\ddot q^* \\
f_c^* \\
\tau^*
\end{bmatrix}
$$

The low-level controller computes the commanded torques using PD feedback around the optimal torque solution:

$$
\tau_{\mathrm{cmd}}
=
\tau^*
+
K_p (q_{\mathrm{des}} - q_j)
+
K_d (\dot q_{\mathrm{des}} - \dot q_j)
$$

where:
- $q_{\mathrm{des}}$ and $\dot q_{\mathrm{des}}$ are the desired joint positions and velocities, respectively, obtained from [[Inverse Kinematics]],
- $K_p$ and $K_d$ are the proportional and derivative gain matrices.