
---
# Floating base system
#### Generalized coordinates:
$$ q = \begin{bmatrix} p_b \\ q_b \\ q_j \end{bmatrix} \in \mathbb{R}^{3}\times \mathbb{S}^3 \times \mathbb{R}^{n}$$
where:
- $p_b \in \mathbb{R}^3$ : base position.
- $q_b \in \mathbb{S}^3$ : base orientation.
- $q_j \in \mathbb{R}^{n}$ : joint positions.
- $n = 12$ : actuated joints.
$$ \dot q = \begin{bmatrix} v_b \\ \omega_b \\ \dot q_j \end{bmatrix} \in \mathbb{R}^{6+n}$$
where:
- $v_b \in \mathbb{R}^3$ : base linear velocity.
- $\omega_b \in \mathbb{R}^3$ : base angular velocity.
- $\dot q_j \in \mathbb{R}^{n}$ : joint velocities.
#### Optimization variables:
$$ z = \begin{bmatrix} \ddot q \\ f_c \\ \tau \end{bmatrix} \in \mathbb{R}^{d}$$
where:
- $\ddot q \in \mathbb{R}^{6+n}$ : generalized acceleration.
- $f_c \in \mathbb{R}^{n_{st}}$ : contact forces.
- $\tau \in \mathbb{R}^{n}$ : joint torque.
- $d = (6 + n) + n_{st} + n$: dimension of the optimization variables.

---
# Tasks
#### Base position task
Least squares objective from [[Base position#Acceleration-based]]:
$$ \frac{1}{2} {|| J_{b} \ddot{q} + \dot{J}_{b} \dot{q} - \ddot{x}^{des}_{b} ||}^{2}_{W_{b}} $$
#### Base attitude task
Least squares objective from [[Base attitude#Acceleration-based]]:
$$ \frac{1}{2} {|| J_{\omega} \ddot{q} + \dot{J}_{\omega} \dot{q} - \dot \omega^{des}_{\omega} ||}^{2}_{W_{\omega}} $$
#### Swing foot task
Least squares objective from [[Swing Task#Acceleration-based]]:
$$ \frac{1}{2} {|| J_{sw} \ddot{q} + \dot{J}_{sw} \dot{q} - \ddot{x}^{des}_{sw} ||}^{2}_{W_{sw}} $$
#### Force regularization
Least squares objective from [[Force regularization#Min norm]]:
$$ \frac{1}{2} {|| f_{c} ||}^2_{W_{f}} $$
#### Torque regularization
Least squares objective from [[Torque regularization]]:
$$ \frac{1}{2} {|| \tau ||}^2_{W_{\tau}} $$
---
# Constraints:
## Floating base dynamics constraint
The core of full WBC, [[Floating base dynamics]] represent the coupling between optimization variables:
$$M(q)\ddot{q} + h(q,\dot{q})= S^\top \tau + J_c^\top f $$
$$ M(q) \ddot{q} - J_{c}^\top f_{c} - S^\top \tau=  -h(q,\dot{q}) $$
#### QP equality form:
$$\boxed{ A_{fb} z = b_{fb} }$$
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
$$\boxed{ A_{st} z = b_{st} }$$
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
$$\boxed{ l_{\tau} \le C_{\tau} {z} \le u_{\tau} }$$
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
$$\boxed{ l_{j} \le C_j {z} \le u_{j} }$$
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
$$\tag{QP} \begin{aligned} \min_{z} & \quad \frac{1}{2}z^{T}Hz+g^{T}z \\ \text{s.t.}&\left\{ \begin{array}{ll} Az = b, \\ l \leq Cz \leq u.\\ \end{array} \right. \end{aligned} $$
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
z^* =
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