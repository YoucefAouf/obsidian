# Min norm
#### Least squares objective:
$$ \frac{1}{2} {|| f_{c} ||}^2_{W_{f}} $$
where:
- $W_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : Weight matrix for the force regularization task.
#### Expanded QP form:
$$\boxed{ \frac{1}{2} f_{c}^\top H_{f}  f_{c} + g^\top_{f} f_{c} }$$
with:
$$ H_{f} = W_{f} $$
$$ g_{f} = 0_{n_{st}\times 1} $$
where:
- $H_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : force regularization quadratic cost matrix.
- $g_{f} \in \mathbb{R}^{n_{st}}$ : force regularization linear cost vector.
- - -
# Reference tracking
#### Least squares objective:
$$ \frac{1}{2} {|| f_{c} - f^{ref}_c ||}^2_{W_{f}} $$
where:
- $f_{c}, f^{ref}_c \in \mathbb{R}^{n_{st}}$ : desired/reference contact forces.
- $W_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : Weight matrix for the force regularization task.

#### Expanded QP form:
$$\boxed{ \frac{1}{2} f_{c}^\top H_{f}  f_{c} + g^\top_{f} f_{c} }$$
with:
$$ H_{f} = W_{f} $$
$$ g_{f} = - W_{f} f^{ref}_c $$
where:
- $H_{f} \in \mathbb{R}^{n_{st}\times n_{st}}$ : force regularization quadratic cost matrix.
- $g_{f} \in \mathbb{R}^{n_{st}}$ : force regularization linear cost vector.