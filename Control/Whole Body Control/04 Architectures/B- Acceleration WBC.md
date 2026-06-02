
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
$$ z = \begin{bmatrix} \ddot q \\ f_c \end{bmatrix} \in \mathbb{R}^{d} $$
where:
- $\ddot q \in \mathbb{R}^{6+n}$ : generalized acceleration.
- $f_c \in \mathbb{R}^{n_{st}}$ : contact forces.
- $d = 6 + n + n_{st}$: dimension of the optimization variables.

---
# Tasks
## Base pose task
#### Task space variable:
Base pose:
$$x_{b} = \begin{bmatrix} p_{b} \\ q_{b} \end{bmatrix} \in \mathbb{R}^{3}\times \mathbb{S}^3  $$
Velocity:
$$ \dot{x}_{b} = J_{b} \dot{q} $$
Acceleration:
$$ \ddot{x}_{b} = J_{b} \ddot{q} + \dot{J}_{b} \dot{q}$$
where:
- $p_{b} \in \mathbb{R}^{3}$ : base position.
- $q_{b} \in \mathbb{S}^3$ : base orientation.
- $J_{b} \in \mathbb{R}^{6\times(6+n)}$ : base Jacobian.
#### Desired acceleration:
$$ \ddot x^{des}_{b} = \ddot x^{ref}_{b} + K_p e_{b} + K_d \dot e_{b} $$
with:
$$ e_{b} 
= \begin{bmatrix} p^{ref}_{b} - p_{b} \\ \text{Log}(R^{ref} R^\top)^\vee \end{bmatrix} $$
$$ \dot e_{b} 
= \begin{bmatrix} v^{ref}_{b} - v_{b} \\ \omega^{ref}_{b} - \omega_{b} \end{bmatrix} $$
where:
- $\ddot x^{des}_{b}, \ddot x^{ref}_{b} \in \mathbb{R}^{6}$ : desired/reference base acceleration.
- $e_{b}, \dot e_{b}\in \mathbb{R}^{3}$ : base position/velocity error.
- $p^{ref}_{b}\in \mathbb{R}^{3}$ : reference base position.
- $v^{ref}_{b}, \omega^{ref}_{b}\in \mathbb{R}^{3}$ : reference base linear/angular velocity.
- $R, R^{ref}\in SO3$ : current/reference base orientation as rotation matrix.
- $K_p \in \mathbb{R}^{6\times6}$ : Proportional gains matrix.
- $K_d \in \mathbb{R}^{6\times6}$ : Derivative gains matrix.
#### Least squares objective:
$$ \frac{1}{2} {|| J_{b} \ddot{q} + \dot{J}_{b} \dot{q} - \ddot x^{des}_{b} ||}^{2}_{W_{b}} $$
where:
- $W_{b} \in \mathbb{R}^{6\times6}$ : Positive semidefinite weight matrix
#### Expanded QP form:
$$\boxed{ \frac{1}{2} z^\top \begin{bmatrix} H_{b} & 0 \\ 0 & 0 \end{bmatrix} z + \begin{bmatrix} g^\top_{b} & 0 \end{bmatrix} z }$$
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
## Force regularization
#### Least squares objective:
$$ \frac{1}{2} {|| f_{c} ||}^2_{W_{f}} $$
where:
- $W_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : Weight matrix for the force regularization task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} z^\top \begin{bmatrix} 0 & 0 \\ 0 & H_{f} \end{bmatrix} z + \begin{bmatrix} 0 & g^\top_{f} \end{bmatrix} z }$$
with:
$$ H_{f} = W_{f} $$
$$ g_{f} = 0_{n_{st}\times 1} $$
where:
- $H_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : force regularization quadratic cost matrix.
- $g_{f} \in \mathbb{R}^{n_{st}}$ : force regularization linear cost vector.
---
# Constraints:
### Floating base dynamics constraint
The core of full WBC, [[Floating base dynamics#Unactuated dynamics]] represent the coupling between optimization variables:
$$M_{b}(q) \ddot{q} + h_{b}(q,\dot{q})= J_{c,b}^\top f_{c} $$
$$ M_{b}(q) \ddot{q} - J_{c,b}^\top f_{c} = -h_{b}(q,\dot{q}) $$
**QP equality form:**
$$\boxed{ A_{fb} z = b_{fb} }$$
with:
$$ A_{fb} = \begin{bmatrix} M_b \ , \ -J_{c,b}^\top \end{bmatrix}$$
$$ b_{fb} = -h_{b}$$
where:
- $A_{fb} \in \mathbb{R}^{6\times d}$ : floating base dynamics equality constraint matrix.
- $b_{fb} \in \mathbb{R}^{6}$ : floating base dynamics equality constraint vector.
### Stance acceleration
$$ \ddot{x}^{des}_{st} = J_{st} \ddot{q} + \dot{J}_{st} \dot{q} = 0 $$
where:
- $\ddot x^{des}_{st} \in \mathbb{R}^{n_{st}}$ : desired stance feet acceleration.
- $J_{st} \in \mathbb{R}^{n_{st}\times(6+n)}$ : stance feet Jacobian.
Rearrange terms:
$$J_{st} \ddot{q} = - \dot{J}_{st} \dot{q}$$
Could also be a soft constraint: ${|| J_{st} \ddot{q} + \dot{J}_{st} \dot{q} ||}^2_{W_{st}}$ , for mud or whenever contacts aren't fixed
**QP equality form:**
$$\boxed{ A_{st} z = b_{st} }$$
with:
$$ A_{st} = \begin{bmatrix} J_{st} & 0_{n_{st}\times n_{st}} \end{bmatrix}$$
$$ b_{st} = - \dot{J}_{st} \dot{q}$$
where:
- $A_{st} \in \mathbb{R}^{n_{st}\times d}$ : stance equality constraint matrix.
- $b_{st} \in \mathbb{R}^{n_{st}}$ : stance equality constraint vector.
### Joint limits
We constraint joint velocities from both [[Joint limits#Velocity limits]] and [[Joint limits#Position limits]]:
We use discrete time updates assuming constant velocity and acceleration in $\Delta t$:
$$ \dot q_j = \frac{q_j(k+1) - q_j(k)}{\Delta t} \ , \ \ddot q_j = \frac{\dot q_j(k+1) - \dot q_j(k)}{\Delta t}$$
We can substitute $q(k+1)$ with upper and lower limits:
$$ \frac{q_{j,min} - q_j(k)}{\Delta t} \le \dot q_j \le \frac{q_{j,max} - q_j(k)}{\Delta t} $$
Therefore, the velocity limits are:
$${\dot q_{j,min}^* = \max \left[ \dot q_{j,min}, \frac{q_{j,min} - q_j(k)}{\Delta t} \right] }$$
$${\dot q_{j,max}^* = \min \left[ \dot q_{j,max}, \frac{q_{j,max} - q_j(k)}{\Delta t} \right]}$$
Similarly for $\dot q_j(k+1)$, we can obtain the joint acceleration limits:
$$ \ddot q_{j,min} = \frac{\dot q_{j,min}^* - \dot q_j(k)}{\Delta t} \le \ddot q_j \le \frac{\dot q_{j,max}^* - \dot q_j(k)}{\Delta t} = \ddot q_{j,max}$$
**QP inequality form:**
$$\boxed{ l_{j} \le C_j {z} \le u_{j} }$$
with:
$$ C_{j} = \begin{bmatrix} 0_{n\times 6} & I_{n\times n} & 0_{n\times n_{st}} \end{bmatrix}$$
$$ l_{j} = \ddot q_{j,min}$$
$$ u_{j} = \ddot q_{j,max}$$
where:
- $C_{j} \in \mathbb{R}^{n\times d}$ : joint acceleration inequality constraint matrix.
- $l_{j} \in \mathbb{R}^{n}$ : joint acceleration lower inequality bounds.
- $u_{j} \in \mathbb{R}^{n}$ : joint acceleration upper inequality bounds.


---
# Full optimization problem (QP form)
Finally we have:
$$\tag{QP} \begin{aligned} \min_{z} & \quad \frac{1}{2}z^{T}Hz+g^{T}z \\ \text{s.t.}&\left\{ \begin{array}{ll} Az = b, \\ l \leq Cz \leq u.\\ \end{array} \right. \end{aligned} $$
with:
$$
\begin{aligned}
H &= 
\begin{bmatrix}
H_{b} + H_{sw} & 0 \\
0 & H_{f}
\end{bmatrix},
&
g &=
\begin{bmatrix}
g_{sw} + g_{b} \\
g_{f}
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
C &= C_j,
&
l &= l_j,
&
u &= u_j
\end{aligned}
$$
where:
- $n_{eq} = 6+n_{st}$
- $n_{in} = n$
- $H \in \mathbb{R}^{d\times d}$ : Hessian matrix.
- $g \in \mathbb{R}^{d}$ : gradient vector
- $A \in \mathbb{R}^{n_{eq}\times d}$ : equality constraint matrix.
- $b \in \mathbb{R}^{n_{eq}}$ : equality constraint vector.
- $C \in \mathbb{R}^{n_{in}\times d}$ : inequality constraint matrix.
- $l \in \mathbb{R}^{n_{in}}$ : lower inequality bounds.
- $u \in \mathbb{R}^{n_{in}}$ : upper inequality bounds.

---
# Controller Pipeline

Acceleration-level WBC solves for the desired generalized accelerations and contact forces:

$$
z^* =
\begin{bmatrix}
\ddot q^* \\
f_c^*
\end{bmatrix}
$$

The actuator torques are obtained from the [[Floating base dynamics]]:

$$
\tau_{\mathrm{wbc}}
=
S \left(
M(q)\ddot q^*
+
h(q,\dot q)
-
J_c^\top f_c^*
\right)
$$

The low-level controller computes the commanded torques using PD feedback:

$$
\tau_{\mathrm{cmd}}
=
\tau_{\mathrm{wbc}}
+
K_p (q_{\mathrm{des}} - q_j)
+
K_d (\dot q_{\mathrm{des}} - \dot q_j)
$$

where:
- $q_{\mathrm{des}}$ and $\dot q_{\mathrm{des}}$ are the desired joint positions and velocities, respectively, obtained from [[Inverse Kinematics]],
- $K_p$ and $K_d$ are the proportional and derivative gain matrices.