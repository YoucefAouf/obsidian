# Motor control
Joint motors are controlled with a Feed-Forward plus Proportional Derivative (PD) controller:
$$ \tau_{cmd} = \tau_{ff} + K_p (q_d - q) + K_d (\dot{q}_d - \dot{q})$$
with:
$$\tau_{ff} = \tau_{g} = g(q)$$
# Feed forward
Take the FRBD:
$$M(q)\ddot{q} + C(q,\dot{q})\dot{q} + g(q) = S^\top \tau + J(q)^\top f,$$
Under quasi-static assumption ($\ddot q \approx 0, \dot q \approx 0$) we can have:
$$g(q) = S^\top \tau + J(q)^\top f,$$
We split the equation to separate the actuated joints:
$$\begin{bmatrix}
    g_b(q) \\ g_j(q)
\end{bmatrix}=
\begin{bmatrix}
    0 \\ \tau
\end{bmatrix}
+
\begin{bmatrix}
    J_b(q)^\top \\ J_j(q)^\top
\end{bmatrix}
 f$$
