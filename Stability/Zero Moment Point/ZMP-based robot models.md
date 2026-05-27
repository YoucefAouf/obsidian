# Full Centroidal model
The formulas found in the literature are:
$$x_{zmp} = x_c - \frac{(z_c - z_{zmp})\ddot{x}_{c} + \dot{L}_y / m}{\ddot{z}_{c} + g} \quad , \quad y_{zmp} = y_c - \frac{(z_c - z_{zmp})\ddot{y}_{c} - \dot{L}_x / m}{\ddot{z}_{c} + g}$$
# Variable Height Inverted Pendulum Model (VHIPM)
Assumption:
- No angular momentum change: $\dot{L} \approx 0$
$$p_{zmp} = p_c - \frac{(z_c - z_{zmp})}{\ddot{z}_{c} + g} \ddot{p}_{c}$$
# Linear Inverted Pendulum Model (LIPM)
Assumptions:
- No angular momentum change: $\dot{L} \approx 0$
- Fixed CoM height: $z_c - z_{zmp} = h$
- Fixed CoM height: $\ddot{z}_c \approx 0$
$$p_{zmp} = p_c - \frac{h}{g} \ddot{p}_{c}$$
so:
$$\ddot{p}_{c} = \omega^2 (p_c - p_{zmp})$$
with $\omega = \sqrt{g/h}$