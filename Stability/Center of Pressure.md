The sum of Ground Reaction Forces (GRF) are equivalent to a resultant force, applied at a point $p_{cop}$ where the horizontal components of the resultant moment at CoP are zero.:
$$\tau_{cop,x} = 0 \quad , \quad \tau_{cop,y} = 0$$
We can calculate the resulting moment at $p_{cop}$:
 $$\tau_{cop} = \sum_i (p_i - p_{cop}) \times f_{i} = \sum_i \left( \begin{bmatrix}
        x_i - x_{cop} \\ y_i - y_{cop} \\ z_i - z_{cop}
    \end{bmatrix} \times \begin{bmatrix}
        f_{i,x} \\ f_{i,y} \\ f_{i,z}
    \end{bmatrix} \right) = \begin{bmatrix}
        \sum_i(y_i - y_{cop}) f_{i,z} - \sum_i(z_i - z_{cop}) f_{i,y} \\
        \sum_i(z_i - z_{cop}) f_{i,x} - \sum_i(x_i - x_{cop}) f_{i,z} \\
        \sum_i(x_i - x_{cop}) f_{i,y} - \sum_i(y_i - y_{cop}) f_{i,x}
    \end{bmatrix}$$
Thus:
$$\begin{align}
   \tau_{cop,x} &= \sum_i(y_i - y_{cop}) f_{i,z} - \sum_i(z_i - z_{cop}) f_{i,y} = 0 \\ \tau_{cop,y} &= \sum_i(z_i - z_{cop}) f_{i,x} - \sum_i(x_i - x_{cop}) f_{i,z} = 0
\end{align}$$
We solve fore $x_{cop}$ and $y_{cop}$:
$$\begin{align}
    x_{cop} &= \frac{\sum_i x_i f_{i,z} - \sum_i(z_i - z_{cop}) f_{i,x}}{\sum_i f_{i,z}} \\
    y_{cop} &= \frac{\sum_i y_i f_{i,z} - \sum_i(z_i - z_{cop}) f_{i,y}}{\sum_i f_{i,z}}
\end{align}$$
Under the assumption of a flat terrain ($z_i = z_{cop}$):
$$\boxed{
    x_{cop} = \frac{\sum_i x_i f_{i,z}}{\sum_i f_{i,z}}
    \quad , \quad
    y_{cop} = \frac{\sum_i y_i f_{i,z}}{\sum_i f_{i,z}}}
$$