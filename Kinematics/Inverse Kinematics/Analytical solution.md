We have \ref{M.xyz}: 
$$\begin{align}
x &= -l_{2}s_{1}-l_{3}s_{12} \tag{eq.x} \\
y &= -l_{1}c_{0}+s_{0}(l_{2}c_{1}+l_{3}c_{12}) \tag{eq.y} \\
z &= -l_{1}s_{0}-c_{0}(l_{2}c_{1}+l_{3}c_{12}) \tag{eq.z}
\end{align}$$
We square both sides of the equations \eqref{eq_x}, \eqref{eq_y}, and \eqref{eq_z}:
$$\begin{align}
x^{2} &= l_{2}^{2} s_{1}^{2} + l_{3}^{2} s_{12}^{2} + 2 l_{2} l_{3} s_{1} s_{12} \tag{eq.x2} \\
y^{2} &= l_{1}^{2} c_{0}^{2} + s_{0}^{2} (l_{2} c_{1} + l_{3} c_{12})^{2} - 2 l_{1} c_{0} s_{0} (l_{2} c_{1} + l_{3} c_{12}) \tag{eq.y2} \\
z^{2} &= l_{1}^{2} s_{0}^{2} + c_{0}^{2} (l_{2} c_{1} + l_{3} c_{12})^{2} + 2 l_{1} c_{0} s_{0} (l_{2} c_{1} + l_{3} c_{12}) \tag{eq.z2}
\end{align}$$
We sum the equations \eqref{eq_x2}, \eqref{eq_y2}, and \eqref{eq_z2}:
$$
    x^{2} + y^{2} + z^{2} = l_{1}^{2} + l_{2}^{2} + l_{3}^{2} + 2 l_{2} l_{3} c_{2}
    \tag{eq.x2y2z2}
$$
Now we can solve for $c_{2}$:
$$
    c_{2} = \frac{x^{2} + y^{2} + z^{2} - l_{1}^{2} - l_{2}^{2} - l_{3}^{2}}{ 2 l_{2} l_{3}}
    \tag{eq.c2}
$$
We know that $\sin{q_2} < 0$, since $-2.72 < q_2 < -0.84$, therefore:

$$
    q_2 = - \text{acos}(\frac{x^{2} + y^{2} + z^{2} - l_{1}^{2} - l_{2}^{2} - l_{3}^{2}}{ 2 l_{2} l_{3}})
    \tag{eq.theta2}
$$
We sum \eqref{eq.y2} and \eqref{eq.z2}:
$$
    y^{2} + z^{2} = l_{1}^{2} + (l_{2} c_{1} + l_{3} c_{12})^{2} \tag{eq.y2z2}
$$
$$
    \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} = l_{2} c_{1} + l_{3} c_{12}
    \tag{eq.sqrt}
$$
% need to do something about this plus or minus. because the plus is the one that works
We substitute with \eqref{eq_sqrt} in \eqref{eq_y} and \eqref{eq_z}:
$$
    y = -l_{1}c_{0} \pm s_{0}\sqrt{y^{2} + z^{2} - l_{1}^{2}}
    \tag{eq.Y}
$$
$$
    z = -l_{1}s_{0} \pm c_{0}\sqrt{y^{2} + z^{2} - l_{1}^{2}}
    \tag{eq.Z}
$$
To solve for \(q_{2}\) we need to write the equations \eqref{eq.Y} and \eqref{eq.Z} in matrix form:
$$
    \begin{bmatrix}
        y\\
        z
    \end{bmatrix} = \begin{bmatrix}
        -l_{1} & \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} \\
        \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} & -l_{1}
    \end{bmatrix} \begin{bmatrix}
        c_{0}\\
        s_{0}
    \end{bmatrix}
    \tag{M.YZ}
$$
We solve for $c_{0}$ and $s_{0}$:
$$
    \begin{bmatrix}
        c_{0}\\
        s_{0}
    \end{bmatrix} = \begin{bmatrix}
        -l_{1} & \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} \\
        \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} & -l_{1}
    \end{bmatrix}^{-1} \begin{bmatrix}
        y\\
        z
    \end{bmatrix}
    \tag{M.c0s0}
$$
We get the equations for $c_{0}$ and $s_{0}$:
$$
    c_{0} = \frac{1}{y^{2} + z^{2}} (- l_{1} y \pm  z \sqrt{y^{2} + z^{2} - l_{1}^{2}})
    \tag{eq.c0}
$$
$$
    s_{0} = \frac{1}{y^{2} + z^{2}} (- l_{1} z \pm  y \sqrt{y^{2} + z^{2} - l_{1}^{2}})
    \tag{eq.s0}
$$
From \eqref{eq.c0} and \eqref{eq.s0}, we can use $atan2$ to calculate $q_0$:
$$
    q_0 = \text{atan2}(- l_{1} z \pm  y \sqrt{y^{2} + z^{2} - l_{1}^{2}}, - l_{1} y \pm  z \sqrt{y^{2} + z^{2} - l_{1}^{2}})
    \tag{eq.theta0}
$$
% now I do theta 1
We can rewrite \eqref{eq.sqrt} as:
$$
    \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} = l_{2} c_{1} + l_{3} ( c_{1} c_{1} - s_{1} s_{2})
    \tag{eq.sqrt2}
$$
We can rearrange it to isolate $c_{1}$ and $s_{1}$:
$$
    \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}} = (l_{2} + l_{3} c_{2})c_{1} + (- l_{3} s_{2} ) s_{1}
    \tag{eq.sqrt3}
$$
By doing the same with \eqref{eq.x}, we get:
$$
    x = (- l_{3} s_{2})c_{1} - (l_{2} + l_{3} c_{2}) s_{1}
    \tag{eq.X}
$$
To solve for $\theta_{1}$ we need to write the equations \eqref{eq.X} and \eqref{eq.sqrt3} in matrix form:
$$
    \begin{bmatrix}
        x\\
        \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}}
    \end{bmatrix} = \begin{bmatrix}
        - l_{3} s_{2} & - (l_{2} + l_{3} c_{2}) \\
        (l_{2} + l_{3} c_{2}) & - l_{3} s_{2}
    \end{bmatrix} \begin{bmatrix}
        c_{1}\\
        s_{1}
    \end{bmatrix}
    \tag{M.Xsqrt3}
$$
We solve for $c_{1}$ and $s_{1}$:
$$
    \begin{bmatrix}
        c_{1}\\
        s_{1}
    \end{bmatrix} = \begin{bmatrix}
        - l_{3} s_{2} & - (l_{2} + l_{3} c_{2}) \\
        (l_{2} + l_{3} c_{2}) & - l_{3} s_{2}
    \end{bmatrix}^{-1} \begin{bmatrix}
        x\\
        \pm \sqrt{y^{2} + z^{2} - l_{1}^{2}}
    \end{bmatrix}
    \tag{M.c1s1}
$$
We get the equations for $c_{1}$ and $s_{1}$:
$$
    c_{1} = \frac{1}{l_{2}^{2} + l_{3}^{2} + 2 l_{2} l_{3} c_{2}} (- l_{3} s_{2} x \pm  (l_{2} + l_{3} c_{2}) \sqrt{y^{2} + z^{2} - l_{1}^{2}})
    \tag{eq.c1}
$$
$$
    s_{1} = \frac{1}{l_{2}^{2} + l_{3}^{2} + 2 l_{2} l_{3} c_{2}} (- (l_{2} + l_{3} c_{2}) x \pm  l_{3} s_{2} \sqrt{y^{2} + z^{2} - l_{1}^{2}})
    \tag{eq.s1}
$$
From \eqref{eq.c1} and \eqref{eq.s1}, we can use $atan2$ to calculate $q_1$:
$$
    q_1 = \text{atan2}(-(l_{2}+l_{3}c_{2})x \pm l_{3}s_{2}\sqrt{y^{2}+z^{2}-l_{1}^{2}}, -l_{3}s{2}x \pm  (l_{2}+l_{3}c_{2}) \sqrt{y^{2}+z^{2}-l_{1}^{2}})
$$
The inverse kinematics can be written:
$$
    q = \mathbf{IK}(p_{f}^H) = 
    \begin{bmatrix}
      \text{atan2}(-l_{1}z \pm y\sqrt{y^{2}+z^{2}-l_{1}^{2}}, -l_{1}y \pm z\sqrt{y^{2}+z^{2}-l_{1}^{2}}) \\ \\
      \text{atan2}(-(l_{2}+l_{3}c_{2})x \pm l_{3}s_{2}\sqrt{y^{2}+z^{2}-l_{1}^{2}}, -l_{3}s_{2}x \pm  (l_{2}+l_{3}c_{2}) \sqrt{y^{2}+z^{2}-l_{1}^{2}}) \\ \\
      - \text{acos}(\frac{x^{2} + y^{2} + z^{2} - l_{1}^{2} - l_{2}^{2} - l_{3}^{2}}{ 2 l_{2} l_{3}}) \\
    \end{bmatrix}
$$

# Jacobian
Joint velocity:
$$
    \dot{q} = J(q)^{-1} \dot{p}_{f}^H
    \tag{eq.jointVelocity}
$$
Joint acceleration:
$$
    \ddot{q} = J^{-1} (\ddot{p}_{f}^H - \dot{J} \dot{q})
    \tag{eq.jointAcceleration}
$$
Inverse Jacobian derived analytically from \ref{M.IK.geometrical} (AI converted C++ to \LaTeX):
$$
    J^{-1}(q) = \frac{\partial q}{\partial x}
    \tag{eq.ikJacobian}
$$
Intermediate Variables:
$$\begin{align}
r &= \sqrt{y^2 + z^2} \\
s &= \sqrt{r^2 - l_1^2} \\
u &= \frac{l_1^2 + l_2^2 + l_3^2 - x^2 - y^2 - z^2}{2 l_2 l_3} \\
D &= x^2 + s^2
\end{align}$$
$q_{0}$ derivatives:
$$\begin{align}
\frac{\partial q_0}{\partial x} &= 0 \\
\frac{\partial q_0}{\partial y} &= -\frac{z}{r^2} - \frac{\sigma l_1 y}{r^3 \sqrt{1 - \left(\frac{l_1}{r}\right)^2}} \\
\frac{\partial q_0}{\partial z} &= \frac{y}{r^2} - \frac{\sigma l_1 z}{r^3 \sqrt{1 - \left(\frac{l_1}{r}\right)^2}}
\end{align}$$
$q_{2}$ derivatives:
$$\begin{align}
\frac{\partial q_2}{\partial x} &= \frac{x}{l_2 l_3 \sqrt{1 - u^2}} \\
\frac{\partial q_2}{\partial y} &= \frac{y}{l_2 l_3 \sqrt{1 - u^2}} \\
\frac{\partial q_2}{\partial z} &= \frac{z}{l_2 l_3 \sqrt{1 - u^2}}
\end{align}$$
$q_{1}$ derivatives:
$$\begin{align}
\frac{\partial q_1}{\partial x} &= -\frac{s}{D} - \frac{\partial q_1}{\partial q_2} \frac{\partial q_2}{\partial x} \\
\frac{\partial q_1}{\partial y} &= \frac{xy}{Ds} - \frac{\partial q_1}{\partial q_2} \frac{\partial q_2}{\partial y} \\
\frac{\partial q_1}{\partial z} &= \frac{xz}{Ds} - \frac{\partial q_1}{\partial q_2} \frac{\partial q_2}{\partial z}
\end{align}$$
With $l_1 = 0.08$ m , $\sigma = 1$ for right legs, $l_1 = -0.08$ m, $\sigma = -1$ for left legs, and:
$$
\frac{\partial q_1}{\partial q_2} = \frac{l_3(l_3 - l_2 u)}{l_2^2 + l_3^2 - 2 l_2 l_3 u}
$$\\
The $3 \times 3$ full Jacobian:

$$
J^{-1}(q)= \begin{bmatrix}
\frac{\partial q_0}{\partial x} & \frac{\partial q_0}{\partial y} & \frac{\partial q_0}{\partial z} \\[0.5em]
\frac{\partial q_1}{\partial x} & \frac{\partial q_1}{\partial y} & \frac{\partial q_1}{\partial z} \\[0.5em]
\frac{\partial q_2}{\partial x} & \frac{\partial q_2}{\partial y} & \frac{\partial q_2}{\partial z}
\end{bmatrix}
$$