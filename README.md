# PINNs-Laminar-Flow-Sudden-Construction-Channel
**Paper from what the idea came from:** https://arxiv.org/pdf/2002.10558.pdf

**Original Implementation (Tensorflow 1):** https://github.com/Raocp/PINN-laminar-flow

**Paper**:
https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/blob/main/VKR_TroshinOf.pdf

![Channel drawio (4)](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/fcdd2e3c-a3d1-49eb-b4f0-3a2aac3e3386)

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

![uvp_step](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/f9023aa2-a48f-4ebc-a6fa-b01a5d2a20d6)
![image](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/f947ca14-314c-4e87-ade9-41b1b8c17651)

![Arhitecture drawio (7)](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/42d07f2f-f958-41e8-9579-2b65690d4256)
