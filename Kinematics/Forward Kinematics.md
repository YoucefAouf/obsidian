Forward kinematics is a transformation from **Joint space** to **Cartesian space**
$$
    p_f^H = \mathbf{FK}(q) = 
    \begin{bmatrix}
      -l_{2}s_{1}-l_{3}s_{12}\\
      -l_{1}c_{0}+s_{0}(l_{2}c_{1}+l_{3}c_{12}) \\
      -l_{1}s_{0}-c_{0}(l_{2}c_{1}+l_{3}c_{12}) \\
    \end{bmatrix}
    \tag{M.xyz}
$$

# Jacobian
Linear velocity:
$$
    \dot{p}_f^H = J(q) \dot{q}
    \tag{eq.linearVelocity}
$$
Jacobian matrix:
$$
    J(q) = \frac{\partial p_f^H}{\partial q} = \begin{bmatrix}
      0 & -l_{2}c_{1}-l_{3}c_{12} & -l_{3}c_{12} \\
      l_{1}s_{0}+c_{0}(l_{2}c_{1}+l_{3}c_{12}) & -s_{0}(l_{2}s_{1}+l_{3}s_{12}) & -l_{3}s_{0}s_{12} \\
      -l_{1}c_{0}+s_{0}(l_{2}c_{1}+l_{3}c_{12}) & c_{0}(l_{2}s_{1}+l_{3}s_{12}) & l_{3}c_{0}s_{12} \\
    \end{bmatrix}
    \tag{eqj}
$$
Linear acceleration:
$$
    \ddot{p}_f^H = J \ddot{q} + \dot{J} \dot{q}
    \tag{eq.linearAcceleration}
$$
Jacobian time derivative:
$$
\dot J_{(:,0)} =
\begin{bmatrix}
0 \\
l_{1}c_{0}\dot q_{0}
- s_{0}(l_{2}c_{1}+l_{3}c_{12})\dot q_{0}
- c_{0}\!\left(l_{2}s_{1}\dot q_{1}+l_{3}s_{12}(\dot q_{1}+\dot q_{2})\right) \\
l_{1}s_{0}\dot q_{0}
+ c_{0}(l_{2}c_{1}+l_{3}c_{12})\dot q_{0}
- s_{0}\!\left(l_{2}s_{1}\dot q_{1}+l_{3}s_{12}(\dot q_{1}+\dot q_{2})\right)
\end{bmatrix}
$$

$$
\dot J_{(:,1)} =
\begin{bmatrix}
l_{2}s_{1}\dot q_{1}
+ l_{3}s_{12}(\dot q_{1}+\dot q_{2}) \\
- c_{0}(l_{2}s_{1}+l_{3}s_{12})\dot q_{0}
- s_{0}\!\left(l_{2}c_{1}\dot q_{1}+l_{3}c_{12}(\dot q_{1}+\dot q_{2})\right) \\
- s_{0}(l_{2}s_{1}+l_{3}s_{12})\dot q_{0}
+ c_{0}\!\left(l_{2}c_{1}\dot q_{1}+l_{3}c_{12}(\dot q_{1}+\dot q_{2})\right)
\end{bmatrix}
$$

$$
\dot J_{(:,2)} =
\begin{bmatrix}
l_{3}s_{12}(\dot q_{1}+\dot q_{2}) \\
- l_{3}\!\left(c_{0}s_{12}\dot q_{0}+s_{0}c_{12}(\dot q_{1}+\dot q_{2})\right) \\
- l_{3}\!\left(s_{0}s_{12}\dot q_{0}-c_{0}c_{12}(\dot q_{1}+\dot q_{2})\right)
\end{bmatrix}
$$
