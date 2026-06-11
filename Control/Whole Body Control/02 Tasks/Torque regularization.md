# Min norm
#### Least squares objective:
$$ \frac{1}{2} {|| \tau ||}^2_{W_{\tau}} $$
where:
- $W_{\tau} \in \mathbb{R}^{n_{st}\times n_{st}}$ : Weight matrix for the torque regularization task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} \tau^\top H_{\tau}  \tau + g^\top_{\tau} \tau }$$
with:
$$ H_{\tau} = W_{\tau} $$
$$ g_{\tau} = 0_{n\times 1} $$
where:
- $H_{\tau} \in \mathbb{R}^{n\times n}$ : torque regularization quadratic cost matrix.
- $g_{\tau} \in \mathbb{R}^{n}$ : torque regularization linear cost vector.