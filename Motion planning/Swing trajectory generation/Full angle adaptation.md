Instead of rotating the knee position with $\theta$, we can construct the curve that represents the leg geometry $r(u)$:
$$
    r(u) = \begin{bmatrix}
        x_r(u) \\ z_r(u)
    \end{bmatrix}
$$

$$
    p(u) = p^H_f + R(\phi)\ r(u)
$$
where $R(\phi)$ rotates the curve around the pivot $p^H_f$ to match the endpoint with $p^H_{knee}$:
$$
    R(\phi) = \begin{bmatrix}
        cos(\phi) & -sin(\phi) \\
        sin(\phi) & cos(\phi)
    \end{bmatrix}
$$
with:
$$
    \phi = \text{atan2}(z^H_{knee} - z^H_f, x^H_{knee} - x^H_f)
$$
We have then:
$$\begin{align}
    x(u) &= x^H_f + x_r(u) \cos(\phi) - z_r(u) \sin(\phi) \\
    z(u) &= z^H_f + x_r(u) \sin(\phi) + z_r(u) \cos(\phi)
\end{align}$$

### Evaluation
Given $dz$ as:
$$
    z(u) = z^H_f + dz
$$
we can solve for $u$:
$$
    x_r(u) \sin(\phi) + z_r(u) \cos(\phi) = dz
$$
where $dz$, and $\phi$ are constants. Once we have $u$, we can evaluate $dx$:
$$
    dx = x_r(u) \cos(\phi) - z_r(u) \sin(\phi)
$$