The robotic leg is not perfectly rectangular, it is curved near the foot. Therefore, to match the overall shape and better adapt to the terrain, an angle $\theta$ is introduced:
We can parameterize the new line:
$$
    p_{\theta}(t) = p^H_f + t\ R(\theta)(p^H_{knee} - p^H_f) 
$$
with:
$$
    R(\theta) = \begin{bmatrix}
        cos(\theta) & -sin(\theta) \\
        sin(\theta) & cos(\theta)
    \end{bmatrix}
$$
For $t = 1$, we can shift $p^H_{knee}$ to a new position:
$$\begin{align}
    x^H_{knee, \theta} &= x^H_f + (x^H_{knee} - x^H_f) \cos(\theta) - (z^H_{knee} - z^H_f) \sin(\theta) \\
    z^H_{knee, \theta} &= z^H_f + (x^H_{knee} - x^H_f) \sin(\theta) + (z^H_{knee} - z^H_f) \cos(\theta)
\end{align}$$
### Evaluation
$$\boxed{
    dx = \frac{dz}{z^H_{knee, \theta} - z^H_f}  (x^H_{knee, \theta} - x^H_f)}
$$