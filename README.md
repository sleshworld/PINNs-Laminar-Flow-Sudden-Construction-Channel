# PINNs-Laminar-Flow-Sudden-Construction-Channel
**Paper from what the idea came from:** https://arxiv.org/pdf/2002.10558.pdf

**Original Implementation (Tensorflow 1):** https://github.com/Raocp/PINN-laminar-flow

**Paper**:
https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/blob/main/VKR_TroshinOf.pdf

![Channel drawio (4)](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/82229eeb-5600-483c-9786-e47ed4fb0780)

**Navier-Stokes Equation**

$$ \nabla \cdot \textbf{v} = 0 $$
$$ \frac{\partial{\textbf{v}}}{\partial{t}} + (\textbf{v} \cdot \nabla)\textbf{v} = -\frac{1}{\rho}\nabla p + \frac{\mu}{\rho}\nabla^2\textbf{v} + \textbf{g} $$
$$ \textbf{v} = (u, v) $$


This representation of the equations is complicated for PINN because of the presence of second-order derivatives, so we transform:

$$\frac{\partial{\textbf{v}}}{\partial{t}} + (\textbf{v} \cdot \nabla)\textbf{v} = -\frac{1}{\rho}\nabla \sigma + \textbf{g}$$
$$\sigma = p \textbf{I} + \mu(\nabla\textbf{v} + \nabla\textbf{v}^T)$$

, where $\sigma$ - is a stress tensor, and $p = -tr(\sigma)/2$.

---

$$\sigma = p \textbf{I} + \mu(\nabla\textbf{v} + \nabla\textbf{v}^T), $$

где $\textbf{I}$- unit tensor.

$$ \sigma = \begin{bmatrix}
s_{11} & s_{12} \\
s_{21} & s_{22}
\end{bmatrix} $$

- $s_{11} = -p + 2\mu \frac{\partial u}{\partial x}$
- $s_{22} = -p + 2\mu \frac{\partial v}{\partial y}$
- $s_{12} = s_{21} = \mu ( \frac{\partial u}{\partial y} +  \frac{\partial v}{\partial x})$
- $p = - \frac{s_{11} + s_{22}}{2} = -tr(\sigma)/2$

In this paper, instead of the continuity equation, the velocity is calculated from the current function.
$$[u, v, w]=\nabla \times [0, 0, \psi]$$

![uvp_step](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/23ed764b-f30b-41e4-9ba4-88b0531b8912)


![White_Arhitecture drawio (1)](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/f7ce7014-2b9e-4d37-9a71-9ea708ba8004)
