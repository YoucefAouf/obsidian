
---
# State representation
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
$$ z =  \dot q  \in \mathbb{R}^{d} $$
where:
- $\dot q \in \mathbb{R}^{d}$ : generalized velocity.
- $d = 6 + n$: dimension of the optimization variables.
---
# Tasks:
#### Base position task
Least squares objective from [[Base position#Velocity-based]]:
$$ \frac{1}{2} {|| J_b \dot q - \dot{x}^{des}_b ||}^2_{W_{b}} $$
#### Base attitude task
Least squares objective from [[Base attitude#Velocity-based]]:
$$ \frac{1}{2} {|| J_{\omega} \dot q - {\omega}^{des}_{b} ||}^2_{W_{{\omega}}} $$
#### Swing foot task
Least squares objective from [[Swing Task#Velocity-based]]

$$ \frac{1}{2} {|| J_{sw} \dot q - \dot x^{des}_{sw} ||}^2_{W_{sw}} $$
---
# Constraints:
### Stance velocity

$$ \dot{x}^{des}_{st} = J_{st} \dot{q} = 0 $$
where:
- $\dot x^{des}_{st} \in \mathbb{R}^{n_{st}}$ : desired stance feet velocity.
- $J_{st} \in \mathbb{R}^{n_{st}\times(6+n)}$ : stance feet Jacobian.
Could also be a soft constraint: ${|| J_{st} \dot q ||}^2_{W_{st}}$ , for mud or whenever contacts aren't fixed
**QP equality form:**
$$\boxed{ A_{st} z = b_{st} }$$
with:
$$ A_{st} = J_{st} $$
$$ b_{st} = 0_{n_{st}\times 1} $$
where:
- $A_{st} \in \mathbb{R}^{n_{st}\times d}$ : stance equality constraint matrix.
- $b_{st} \in \mathbb{R}^{n_{st}}$ : stance equality constraint vector.
## Joint limits
We constraint joint velocities from both [[Joint limits#Velocity limits]] and [[Joint limits#Position limits]]:
We use discrete time updates assuming constant velocity in $\Delta t$:
$$ \dot q_j = \frac{q_j(k+1) - q_j(k)}{\Delta t} $$
We can substitute:
$$ \frac{q_{j,min} - q_j(k)}{\Delta t} \le \dot q_j \le \frac{q_{j,max} - q_j(k)}{\Delta t} $$
Therefore, the velocity limits are:
$$ \dot q_{j,min}^* = \max \left[ \dot q_{j,min}, \frac{q_{j,min} - q_j(k)}{\Delta t} \right] \le \dot q_j \le \min \left[ \dot q_{j,max}, \frac{q_{j,max} - q_j(k)}{\Delta t} \right] = \dot q_{j,max}^* $$
**QP inequality form:**
$$\boxed{ l_{j} \le C_j {z} \le u_{j} }$$
with:
$$ C_{j} = \begin{bmatrix} 0_{n\times 6} & I_{n\times n} \end{bmatrix}$$
$$ l_{j} = \dot q_{j,min}^*$$
$$ u_{j} = \dot q_{j,max}^*$$
where:
- $C_{j} \in \mathbb{R}^{n\times d}$ : joint velocity inequality constraint matrix.
- $l_{j} \in \mathbb{R}^{n}$ : joint velocity lower inequality bounds.
- $u_{j} \in \mathbb{R}^{n}$ : joint velocity upper inequality bounds.

---
# Full optimization problem (QP form)
Finally we have:
$$\tag{QP} \begin{aligned} \min_{z} & \quad \frac{1}{2}z^{T}Hz+g^{T}z \\ \text{s.t.}&\left\{ \begin{array}{ll} Az = b, \\ l \leq Cz \leq u.\\ \end{array} \right. \end{aligned} $$
with:
$$
\begin{aligned}
H &= H_{b} + H_{\omega} + H_{sw}
&
g &= g_{b} + g_{\omega} + g_{sw}
\\[1em]
A &= A_{st}
&
b &= b_{st}
\\[1em]
C &= C_j,
&
l &= l_j,
&
u &= u_j
\end{aligned}
$$
where:
- $n_{eq} = n_{st}$
- $n_{in} = n$
- $H \in \mathbb{R}^{d\times d}$ : Hessian matrix.
- $g \in \mathbb{R}^{d}$ : gradient vector
- $A \in \mathbb{R}^{n_{eq}\times d}$ : equality constraint matrix.
- $b \in \mathbb{R}^{n_{eq}}$ : equality constraint vector.
- $C \in \mathbb{R}^{n_{in}\times d}$ : inequality constraint matrix.
- $l \in \mathbb{R}^{n_{in}}$ : lower inequality bounds.
- $u \in \mathbb{R}^{n_{in}}$ : upper inequality bounds.
---
# Controller pipeline
Velocity-level WBC solves for the desired generalized velocity:
$$
z^* = \dot q^*
$$
The desired joint positions are obtained through discrete-time integration of the commanded joint velocities:
$$q_{j,\mathrm{des}}(k+1)
=
q_{j,\mathrm{des}}(k)
+
\dot q_j^* \Delta t$$
The low-level controller computes the commanded torques using PD control with gravity compensation:
$$\tau_{\mathrm{cmd}}
=
\tau_g
+
K_p (q_{j,\mathrm{des}} - q_j)
+
K_d (\dot q_j^* - \dot q_j)$$
where:
- $\tau_g$ is the gravity compensation torque,
- $K_p$ and $K_d$ are the proportional and derivative gain matrices.