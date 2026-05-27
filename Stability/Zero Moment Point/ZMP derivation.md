Sum of forces applied to the system:
$$
\sum_i f_{i} + m g + \sum_k F_k= m \ddot{p}_c \tag{ZMP-1}
$$

Sum of moments applied to the system:
$$
    \sum_i (p_i - p_c) \times f_i + \sum_k (p_k - p_c) \times F_k = \dot{L}
$$
We introduce $p_{zmp}$:
$$\begin{align}
    (p_i - p_c) &= (p_i - p_{zmp}) + (p_{zmp} - p_c) \\
    (p_k - p_c) &= (p_k - p_{zmp}) + (p_{zmp} - p_c)
\end{align} $$
So:
$$
    \sum_i (p_i - p_{zmp}) \times f_i + (p_{zmp} - p_c) \times (\sum_i f_i + \sum_k F_k) + \sum_k (p_k - p_{zmp}) \times F_k = \dot{L}
$$
from (ZMP-1)
$$
    \sum_i f_{i} + \sum_k F_k= m \ddot{p}_c -m g
$$
Substitute with it:
$$
    \sum_i (p_i - p_{zmp}) \times f_i + (p_{zmp} - p_c) \times (m \ddot{p}_c -m g) + \sum_k (p_k - p_{zmp}) \times F_k = \dot{L}
$$
Rearrange terms:
$$
    \sum_i (p_i - p_{zmp}) \times f_i + (p_c - p_{zmp}) \times (m g - m \ddot{p}_c) + \sum_k (p_k - p_{zmp}) \times F_k = \dot{L} 
    \tag{ZMP-2}
$$
ZMP definition: $\tau_{zmp}(GRF)|_{x,y} = 0$, meaning that:
$$
    \sum_i (p_i - p_{zmp}) \times f_i = \begin{bmatrix}
        0 \\ 0 \\ \dots
    \end{bmatrix} 
$$
And given that:
$$
    (p_c - p_{zmp}) \times (m g - m \ddot{p}_c) = \begin{bmatrix}
        x_c - x_{zmp} \\ y_c - y_{zmp} \\ z_c - z_{zmp}
    \end{bmatrix} \times \begin{bmatrix}
        m(- \ddot{x}_{c}) \\ m(-\ddot{y}_{c}) \\ m(-g -\ddot{z}_{c})
    \end{bmatrix} = \begin{bmatrix}
        m (y_c - y_{zmp}) (-g -\ddot{z}_{c}) + m \ddot{y}_{c} (z_c - z_{zmp})\\
        -m \ddot{x}_{c} (z_c - z_{zmp}) - m(-g -\ddot{z}_{c}) (x_c - x_{zmp}) \\
        \dots
    \end{bmatrix}
$$
and:
$$
    \begin{aligned}
        \sum_k (p_k - p_{zmp}) \times F_k &= \sum_k \left( \begin{bmatrix}
            x_k - x_{zmp} \\ y_k - y_{zmp} \\ z_k - z_{zmp}
        \end{bmatrix} \times \begin{bmatrix} 
            F_{k,x} \\ F_{k,y} \\ F_{k,z}
        \end{bmatrix} \right) = \begin{bmatrix}
            \sum_k (y_k - y_{zmp})F_{k,z} - \sum_k(z_k - z_{zmp})F_{k,y} \\
            \sum_k(z_k - z_{zmp})F_{k,x} - \sum_k(x_k - x_{zmp})F_{k,z} \\
            \dots
        \end{bmatrix} 
    \end{aligned}
$$
We can then write from (ZMP-2) the following:
$$
    \begin{bmatrix}
        m (y_c - y_{zmp}) (-g -\ddot{z}_{c}) + m \ddot{y}_{c} (z_c - z_{zmp})\\
        -m \ddot{x}_{c} (z_c - z_{zmp}) - m(-g -\ddot{z}_{c}) (x_c - x_{zmp}) \\
        \dots
    \end{bmatrix} + \begin{bmatrix}
        \sum_k(y_k - y_{zmp})F_{k,z} - \sum_k(z_k - z_{zmp})F_{k,y} \\
        \sum_k(z_k - z_{zmp})F_{k,x} - \sum_k(x_k - x_{zmp})F_{k,z} \\
        \dots
    \end{bmatrix} = \begin{bmatrix}
        \dot{L}_x \\ \dot{L}_y \\ \dot{L}_z
    \end{bmatrix}
$$
We end up with two equations with two unknowns($z_{zmp}$ depends on the assumption, mainly chosen at the ground level):
$$\begin{align}
    m (y_c - y_{zmp}) (-g -\ddot{z}_{c}) + m \ddot{y}_{c} (z_c - z_{zmp}) + \sum_k(y_k - y_{zmp})F_{k,z} - \sum_k(z_k - z_{zmp})F_{k,y} &= \dot{L}_x \\
    -m \ddot{x}_{c} (z_c - z_{zmp}) - m(-g -\ddot{z}_{c}) (x_c - x_{zmp}) + \sum_k(z_k - z_{zmp})F_{k,x} - \sum_k(x_k - x_{zmp})F_{k,z} &= \dot{L}_y
\end{align}$$
We solve for $x_{zmp}$ and $y_{zmp}$:
$$\begin{align}
    x_{zmp} &= \frac{mx_c(g +\ddot{z}_{c}) - m \ddot{x}_{c} (z_c - z_{zmp}) + \sum_k(z_k - z_{zmp})F_{k,x} - \sum_k x_k F_{k,z} -\dot{L}_y}{m(g +\ddot{z}_{c}) - \sum_k F_{k,z}}  \\
    y_{zmp} &= \frac{m y_c (g +\ddot{z}_{c}) - m \ddot{y}_{c} (z_c - z_{zmp}) + \sum_k(z_k - z_{zmp})F_{k,y} - \sum_k y_k F_{k,z} + \dot{L}_x}{m(g + \ddot{z}_{c}) - \sum_k F_{k,z}}
\end{align}$$
If $\sum_k F_k = F_{mud}$ applied at the foot tip of the swing leg ($p_k = p_f$):
$$
    \boxed{\begin{aligned}
    x_{zmp} &= \frac{mx_c(g +\ddot{z}_{c}) - m \ddot{x}_{c} (z_c - z_{zmp}) + (z_f - z_{zmp})F_{mud,x} - x_f F_{mud,z} -\dot{L}_y}{m(g +\ddot{z}_{c}) - F_{mud,z}}  \\
    y_{zmp} &= \frac{m y_c (g +\ddot{z}_{c}) - m \ddot{y}_{c} (z_c - z_{zmp}) + (z_f - z_{zmp})F_{mud,y} - y_f F_{mud,z} + \dot{L}_x}{m(g + \ddot{z}_{c}) - F_{mud,z}}
    \end{aligned}}
    \tag{ZMP-3}
$$