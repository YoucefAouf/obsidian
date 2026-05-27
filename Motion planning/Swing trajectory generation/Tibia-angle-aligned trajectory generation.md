The tibia angle w.r.t. the base could be calculated at any time $t$ using the rigid kinematic chain.\\
Given a foot position in the hip frame $p^H_f$, joint values could be calculated using inverse kinematics\ref{M_IK_geometrical}:
$$
    q = \begin{bmatrix}
        q_0 \\ q_1 \\ q_2
    \end{bmatrix} =
    \mathbf{IK}(p^H_f)
$$
We can then get the knee joint position w.r.t. the Hip frame as follows:
$$
    p^{H}_{knee} = \begin{bmatrix}
        -l_2 s_1 \\
        -l_1 c_0 + l_2 s_0 c_1 \\
        -l_1 s_0 - l_2 c_0 c_1
    \end{bmatrix}
$$
The line between $p^H_f$ and $p^H_{knee}$ is parameterized as:
$$
    p(t) = p^H_f + t (p^H_{knee} - p^H_f)
$$
If we work in the Z-X plane:
$$\begin{align}
    x(t) &= x^H_f + t (x^H_{knee} - x^H_f) 
    \label{eq:x(t)} \\
    z(t) &= z^H_f + t (z^H_{knee} - z^H_f) \label{eq:z(t)}
\end{align}$$
From \ref{eq:z(t)} we can get:
$$
    t = \frac{z(t) - z^H_f}{z^H_{knee} - z^H_f} 
$$
Substitute in \ref{eq:x(t)}:
$$
    x(t) = x^H_f + \frac{z(t) - z^H_f}{z^H_{knee} - z^H_f}  (x^H_{knee} - x^H_f)
$$
### Evaluation
We introduce increments $dz$ and $dx$ from the foot position:
$$\begin{align}
    z(t) &= z^H_f + dz \\
    x(t) &= x^H_f + dx
\end{align}$$
with $dz$ being a fixed step, and:
$$
    dx = \frac{dz}{z^H_{knee} - z^H_f}  (x^H_{knee} - x^H_f)
$$