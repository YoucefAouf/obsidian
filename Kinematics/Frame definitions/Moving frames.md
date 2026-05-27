The transformation matrix from the planning frame $B_0$ to the current body frame $B$ is given by: 
$$
    \prescript{B_0}{B}{T} = 
    \begin{bmatrix}
        \prescript{B_0}{B}{R} & p^{B_0} _{B}\\
        0^\top & 1
    \end{bmatrix}
    \tag{eq.BiB}
$$

### Foot position
The foot position in the frame $B$ could be derived as follows:
$$
    \begin{bmatrix}
        p^{B}_{f} \\ 1
    \end{bmatrix} = (\prescript{B_0}{B}{T})^{-1} \,
    \begin{bmatrix}
        p^{B_0}_{f} \\ 1
    \end{bmatrix} 
$$
The foot position could be also expressed in the hip frame:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = (\prescript{B}{H}{T})^{-1} \, (\prescript{B_0}{B}{T})^{-1} \, \prescript{B_0}{H_0}{T} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
since $\prescript{B_0}{H_0}{T} = \prescript{B}{H}{T}$:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = (\prescript{B}{H}{T})^{-1} \, (\prescript{B_0}{B}{T})^{-1} \, \prescript{B}{H}{T} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
we can substitute with \ref{eq.Th} and \ref{eq.BiB}:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = 
    \begin{bmatrix}
        I_3 & p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix}^{-1}
    \begin{bmatrix}
        \prescript{B_0}{B}{R} & p^{B_0} _{B}\\
        0^\top & 1
    \end{bmatrix}^{-1}
     \begin{bmatrix}
        I_3 & p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
Expanding the inverse:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = 
    \begin{bmatrix}
        I_3 & -p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix}
    \begin{bmatrix}
        \prescript{B_0}{B}{R}^\top & -\prescript{B_0}{B}{R}^\top p^{B_0} _{B}\\
        0^\top & 1
    \end{bmatrix}
     \begin{bmatrix}
        I_3 & p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
Direct multiplication:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = 
    \begin{bmatrix}
        \prescript{B_0}{B}{R}^\top & -\prescript{B_0}{B}{R}^\top p^{B_0} _{B}- p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix}
    \begin{bmatrix}
        I_3 & p^{B}_{H} \\
        0^\top & 1
    \end{bmatrix} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
Once more:
$$
    \begin{bmatrix}
        p^{H}_{f} \\ 1
    \end{bmatrix} = 
    \begin{bmatrix}
        \prescript{B_0}{B}{R}^\top & \prescript{B_0}{B}{R}^\top (p^{B}_{H} - p^{B_0}_{B}) - p^{B}_{H}\\
        0^\top & 1
    \end{bmatrix} \, 
    \begin{bmatrix}
        p^{H_0}_{f} \\ 1
    \end{bmatrix}
$$
Therefore:
$$\boxed{
    p^{H}_{f} = \prescript{B_0}{B}{R}^\top (p^{H_0}_{f} + p^{B}_{H} - p^{B_0}_{B}) - p^{B}_{H}}
    \tag{eq.pfH}
$$
Alternatively, if $p^{B_0}_{f}$ is known:
$$
    p^{H}_{f} = \prescript{B_0}{B}{R}^\top  (p^{B_0}_{f} - p^{B_0}_{B}) - p^{B}_{H}
$$

### Foot velocity
We can derive \ref{eq:pfH} w.r.t. time:
$$
    \dot{p}_{f}^{H} = \frac{\partial \prescript{B_0}{B}{R}^\top}{\partial t}   (p^{H_0}_{f} + p^{B}_{H} - p^{B_0}_{B}) + \prescript{B_0}{B}{R}^\top \frac{\partial(p^{H_0}_{f} + p^{B}_{H} - p^{B_0}_{B})}{\partial t}
$$
Therefore:
$$\boxed{
    \dot{p}_{f}^{H} = \frac{\partial \prescript{B_0}{B}{R}^\top}{\partial t} (p^{H_0}_{f} + p^{B}_{H} - p^{B_0}_{B}) + \prescript{B_0}{B}{R}^\top \left( \dot{p}^{H_0}_{f} + \dot{p}^{B_0}_{B} \right)}
$$

# Frame $B_0$ as reference frame
Let $B_0$ denote the body frame at which the foot trajectory was planned, during the motion of the CoM from $C_0$: the CoM trajectory reference, to $B$: the current body position.\\
The transformation between $B_0$ and $B$ is given by:
$$
    \prescript{B_0}{B}{T} = \prescript{C_0}{B_0}{T^{-1}} \prescript{C_0}{B}{T}
$$
$$
    \prescript{B_0}{B}{T} = 
    \begin{bmatrix}
        \prescript{C_0}{B_0}{R^\top} & -\prescript{C_0}{B_0}{R^\top} \, p^{C_0} _{B_0}\\
        0^\top & 1
    \end{bmatrix}
    \begin{bmatrix}
        \prescript{C_0}{B}{R} & p^{C_0} _{B}\\
        0^\top & 1
    \end{bmatrix}
$$
$p^{C_0}_{B}$ and $\prescript{C_0}{B}{R}$ are the commanded CoM position and orientation w.r.t. frame $C_0$.
$$
    \prescript{B_0}{B}{T} = 
    \begin{bmatrix}
        \prescript{C_0}{B_0}{R^\top} \prescript{C_0}{B}{R} & \prescript{C_0}{B_0}{R^\top} (p^{C_0} _{B} - p^{C_0} _{B_0})\\
        0^\top & 1
    \end{bmatrix}
    \tag{eq:T.{B0}.{B}}
$$
Therefore:
$$\boxed{
    p^{B_0}_{B} = 
        \prescript{C_0}{B_0}{R^\top} (p^{C_0} _{B} - p^{C_0} _{B_0})}
    \label{eq:p^{B_0}_{B}}
$$
$$\boxed{
    \prescript{B_0}{B}{R} = \prescript{C_0}{B_0}{R^\top} \prescript{C_0}{B}{R}}
    \tag{eq:R.{B0}.{B}}
$$
Time derivatives:
$$
    \dot{p}^{B_0}_{B} = \frac{\partial \prescript{C_0}{B_0}{R^\top}}{\partial t} (p^{C_0} _{B} - p^{C_0} _{B_0}) + \prescript{C_0}{B_0}{R^\top} (\dot{p}^{C_0} _{B} - \dot{p}^{C_0} _{B_0})
$$
$$
    \frac{\partial \prescript{B_0}{B}{R}}{\partial t} = \frac{\partial \prescript{C_0}{B_0}{R^\top}}{\partial t} \prescript{C_0}{B}{R} + \prescript{C_0}{B_0}{R^\top} \frac{\partial \prescript{C_0}{B}{R}}{\partial t}
$$
We have \cite{zhao2016time}: 
$$
    \frac{\partial \prescript{C_0}{B}{R}}{\partial t} = [\omega_{B/C_0}^{C_0}]_\times \, \prescript{C_0}{B}{R}  \quad , \quad 
    \frac{\partial \prescript{C_0}{B}{R^\top}}{\partial t} = \prescript{C_0}{B}{R}^\top \, [\omega_{B/C_0}^{C_0}]_\times^\top = - \prescript{C_0}{B}{R}^\top \, [\omega_{B/C_0}^{C_0}]_\times
    \label{eq:R^{C_0}_{B}_derivatives}
$$
with $\omega_{B/C_0}^{C_0}$ the angular velocity of frame $B$ w.r.t. frame $C_0$ expressed in frame $C_0$.
IMU returns $\omega_{B/W}^{B}$, so to calculate $\omega_{B/C_0}^{C_0}$ we need to transform the gyroscope output:
$$
    \omega_{B/C_0}^{C_0} = \prescript{C_0}{B}{R} \, \omega_{B/W}^{B} - \omega_{C_0/W}^{C_0}
$$
$\omega_{C_0/W}^{C_0}$ is the output of the gyroscope at frame $C_0$ (should be recorded).
### Code implementation
For a quasi-static gait where CoM starts and ends motion with $w = 0$, we have: $\frac{\partial \prescript{C_0}{B_0}{R}}{\partial t} = 0_3$
From this simplification we can write:
$$\boxed{
    \dot{p}^{B_0}_{B} =
    \prescript{C_0}{B_0}{R^\top} (\dot{p}^{C_0} _{B} - \dot{p}^{C_0} _{B_0})}
$$
and:
$$\boxed{
    \frac{\partial \prescript{B_0}{B}{R}}{\partial t} = 
    \prescript{C_0}{B_0}{R^\top} [\omega_{B/C_0}^{C_0}]_\times \, \prescript{C_0}{B}{R}}
$$

# Frame $B_0$ as reference frame + CoM motion re-planning
If during the execution of the swing trajectory, the body planner generates a new CoM motion, the transformation between $C_0$ and $B$ becomes:
$$
    \prescript{C_0}{B}{T} = \prescript{C_0}{C_1}{T} \prescript{C_1}{B}{T}
$$
with $C_1$ being the new CoM motion reference frame (instead of $C_0$):
$$
    p^{C_0}_{B} = 
    \prescript{C_0}{C_1}{R} \ p^{C_1} _{B} 
    +  p^{C_0} _{C_1}
$$
Time derivative:
$$
    \dot{p}^{C_0}_{B} = 
    \frac{\partial \prescript{C_0}{C_1}{R}}{\partial t} p^{C_1} _{B}
    + \prescript{C_0}{C_1}{R} \ \dot{p}^{C_1} _{B}
    + \dot{p}^{C_0}_{C_1}
$$
### Code implementation
For a quasi-static gait where CoM starts and ends motion with $w = 0$, we have: 
$\frac{\partial \prescript{C_0}{C_1}{R}}{\partial t} = 0_3$\\
From this simplification we can write:
$$
    \dot{p}^{C_0}_{B} = 
    \prescript{C_0}{C_1}{R} \ \dot{p}^{C_1} _{B}
    + \dot{p}^{C_0}_{C_1}
$$
\textbf{Important note:}\\
Unlike translations that are estimated from the CoM motion commands, all orientations including $\prescript{C_0}{B}{R}$ and other angular velocities are based on the IMU. Therefore, CoM motion re-planning has no effect on the way we calculate them.

# Frame $B_0$ as reference frame +$n$ CoM motion re-plannings
The transformation between $C_0$ and $B$ is given by:
$$
    \prescript{C_0}{B}{T} = \prescript{C_0}{C_1}{T} \prescript{C_1}{C_2}{T} \ \dots \prescript{C_{n}}{B}{T}
$$
Therefore:
$$\boxed{
    \begin{bmatrix}
        p^{C_0}_{B} \\ 1
    \end{bmatrix} = 
        \prescript{C_0}{C_1}{T} \prescript{C_1}{C_2}{T} \ \dots \prescript{C_{n-1}}{C_{n}}{T}
        \begin{bmatrix}
            p^{C_{n}}_{B} \\ 1
        \end{bmatrix}}
$$
Time derivative (need to focus):
$$
    \begin{bmatrix}
        p^{C_0}_{B} \\ 1
    \end{bmatrix} = 
        \begin{bmatrix} \prescript{C_0}{C_1}{R} & p^{C_0}_{C_1} \\ 0^\top & 1 \end{bmatrix}
        \begin{bmatrix} \prescript{C_1}{C_2}{R} & p^{C_1}_{C_2} \\ 0^\top & 1 \end{bmatrix}
        \dots
        \begin{bmatrix} \prescript{C_{n-1}}{C_{n}}{R} & p^{C_{n-1}}_{C_{n}} \\ 0^\top & 1 \end{bmatrix}
        \begin{bmatrix}
            p^{C_{n}}_{B} \\ 1
        \end{bmatrix}
$$
$$
    \begin{bmatrix}
        p^{C_0}_{B} \\ 1
    \end{bmatrix} = 
        \begin{bmatrix} 
            \begin{array}{c:c}
                \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R} &
                p^{C_0}_{C_1} + \prescript{C_0}{C_1}{R} \ p^{C_1}_{C_2} + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \ p^{C_2}_{C_3} + \dots 
                + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ p^{C_{n-1}}_{C_{n}}\\
                0^\top & 1 
            \end{array}
        \end{bmatrix}
        \begin{bmatrix}
            p^{C_{n}}_{B} \\ 1
        \end{bmatrix}
$$
Therefore:
$$
        p^{C_0}_{B} = 
            \left(\prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R}\right) p^{C_{n}}_{B}
            + \left(p^{C_0}_{C_1} + \prescript{C_0}{C_1}{R} \ p^{C_1}_{C_2} + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \ p^{C_2}_{C_3} + \dots 
            + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ p^{C_{n-1}}_{C_{n}}\right)
$$
If we take the time derivative of both sides we get (using $\dot{\Box}$ for better visuals):
$$
    \begin{split}
    \dot{p}^{C_0}_{B} &= 
        \left(\prescript{C_0}{C_1}{\dot{R}} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R}\right) p^{C_{n}}_{B}
        + \left(\prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{\dot{R}} \dots \prescript{C_{n-1}}{C_{n}}{R}\right) p^{C_{n}}_{B} + \dots \\
        & + \left(\prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{\dot{R}}\right) p^{C_{n}}_{B}
        + \left(\prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R}\right) \dot{p}^{C_{n}}_{B} \\
        & + \Bigg[ \dot{p}^{C_0}_{C_1} + \left(\prescript{C_0}{C_1}{\dot{R}} \ p^{C_1}_{C_2} + \prescript{C_0}{C_1}{R} \ \dot{p}^{C_1}_{C_2}\right) 
        + \left(\prescript{C_0}{C_1}{\dot{R}} \prescript{C_1}{C_2}{R} \ p^{C_2}_{C_3} + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{\dot{R}} \ p^{C_2}_{C_3} + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \ \dot{p}^{C_2}_{C_3}\right) + \dots \\
        & + \Big( \prescript{C_0}{C_1}{\dot{R}} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ p^{C_{n-1}}_{C_{n}}
        + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{\dot{R}} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ p^{C_{n-1}}_{C_{n}} + \dots \\
        &+ \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{\dot{R}} \ p^{C_{n-1}}_{C_{n}}
        + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ \dot{p}^{C_{n-1}}_{C_{n}}\Big) \Bigg]
    \end{split}
$$
### Code implementation
For a quasi-static gait where CoM starts and ends motion with $w = 0$, we have: 
$ \prescript{C_{i-1}}{C_i}{\dot{R}} = 0_3, $ for $i = 1:n$\\
From this simplification we can write:
$$
    \dot{p}^{C_0}_{B} = 
        \left(\prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R}\right) \dot{p}^{C_{n}}_{B}
        + \left( \dot{p}^{C_0}_{C_1} + \prescript{C_0}{C_1}{R} \ \dot{p}^{C_1}_{C_2}
        + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \ \dot{p}^{C_2}_{C_3} + \dots 
        + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ \dot{p}^{C_{n-1}}_{C_{n}}\right)
$$
Which could be written in matrix form as:
$$
    \begin{bmatrix}
        \dot{p}^{C_0}_{B} \\ 1
    \end{bmatrix} = 
        \begin{bmatrix} 
            \begin{array}{c:c}
                \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-1}}{C_{n}}{R} &
                \dot{p}^{C_0}_{C_1} + \prescript{C_0}{C_1}{R} \ \dot{p}^{C_1}_{C_2} + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \ \dot{p}^{C_2}_{C_3} + \dots 
                + \prescript{C_0}{C_1}{R} \prescript{C_1}{C_2}{R} \dots \prescript{C_{n-2}}{C_{n-1}}{R} \ \dot{p}^{C_{n-1}}_{C_{n}}\\
                0^\top & 1 
            \end{array}
        \end{bmatrix}
        \begin{bmatrix}
            \dot{p}^{C_{n}}_{B} \\ 1
        \end{bmatrix}
$$
Or we can also split it into $n$ matrix multiplications, and we define the velocity-level homogeneous transforms:
$$
    \begin{bmatrix}
        \dot{p}^{C_0}_{B} \\ 1
    \end{bmatrix} = 
        \underbrace{ \begin{bmatrix} \prescript{C_0}{C_1}{R} & \dot{p}^{C_0}_{C_1} \\ 0^\top & 1 \end{bmatrix} }_{\prescript{C_0}{C_1}{T^{(v)}}}
        \underbrace{ \begin{bmatrix} \prescript{C_1}{C_2}{R} & \dot{p}^{C_1}_{C_2} \\ 0^\top & 1 \end{bmatrix} }_{\prescript{C_1}{C_2}{T^{(v)}}}
        \dots
        \underbrace{ \begin{bmatrix} \prescript{C_{n-1}}{C_{n}}{R} & \dot{p}^{C_{n-1}}_{C_{n}} \\ 0^\top & 1 \end{bmatrix} }_{\prescript{C_{n-1}}{C_{n}}{T^{(v)}}}
        \begin{bmatrix}
            \dot{p}^{C_{n}}_{B} \\ 1
        \end{bmatrix}
$$
Finally:
$$\boxed{
    \begin{bmatrix}
        \dot{p}^{C_0}_{B} \\ 1 
    \end{bmatrix} = 
        \prescript{C_0}{C_1}{T^{(v)}} \prescript{C_1}{C_2}{T^{(v)}} \ \dots \prescript{C_{n-1}}{C_{n}}{T^{(v)}}
        \begin{bmatrix}
            \dot{p}^{C_{n}}_{B} \\ 1
        \end{bmatrix}}
$$
### Important detail:
In the code we use the $k$ index for the latest CoM motion reference frame to define $\prescript{C_0}{C_k}{T}$ and $\prescript{C_0}{C_k}{T^{(v)}}$, then we multiply them by $\prescript{C_k}{C_{k+1}}{T}$ and $\prescript{C_k}{C_{k+1}}{T^{(v)}}$ respectively, each time a new CoM motion is generated.
### Future note:
We could alternatively use the Adjoint matrix to be more precise and general, but the approach above make the implementation in the code easier.