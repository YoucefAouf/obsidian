We have three tasks:
$$ \min_{\ddot q, f_c} \ \ 
\left( \frac{1}{2} \ddot{q}^\top H_{b} \ddot{q} + g^\top_{b} \ddot{q} \right)
+ \left( \frac{1}{2} \ddot{q}^\top H_{sw} \ddot{q} + g^\top_{sw} \ddot{q} \right)
+ \left( \frac{1}{2} f_{c}^\top H_{f} f_{c} + g^\top_{f} \right)$$
We can write them in terms of $x = \begin{bmatrix} \ddot q , \ f_c \end{bmatrix}^\top$:

$$ \min_{x} \ \ 
\left( \frac{1}{2} x^\top \begin{bmatrix} H_{b} & 0 \\ 0 & 0 \end{bmatrix} x + \begin{bmatrix} g^\top_{b} & 0 \end{bmatrix} x \right) +
\left( \frac{1}{2} x^\top \begin{bmatrix} H_{sw} & 0 \\ 0 & 0 \end{bmatrix} x + \begin{bmatrix} g^\top_{sw} & 0 \end{bmatrix} x \right) +
\left( \frac{1}{2} x^\top \begin{bmatrix} 0 & 0 \\ 0 & H_{f} \end{bmatrix} x + \begin{bmatrix} 0 & g^\top_{f} \end{bmatrix} x \right)$$

