---
layout: post
title: Optimal trajectory of a point mass in 3D space given initial and final position and velocity
---

The problem I want to discuss in this post is "what is the optimal trajectory of a point mass going from a point A to a point B?".

Before starting solving the problem assume:
1. the mass is 1kg
1. no friction forces are considered
2. no energy recovery/accumulation system is possible

With optimal trajectory I mean the trajectory that minimize the "used energy" for going from A to B.
This statement can be formalized as the following minimization problem:
$$
  \begin{aligned}
    \min\limits_{F_x, F_y, F_z} J &= \min\limits_{F_x, F_y, F_z} \int\limits_{0}^{T} \dfrac{1}{2}(F_x^2 + F_y^2 + F_z^2) \dd t \\
    &\text{subject to} \\
    \begin{cases}
        \dot x = u \\
        \dot y = v \\
        \dot z = w \\
        \dot u = F_x \\
        \dot v = F_y \\
        \dot w = F_z - g \\
    \end{cases}
    \quad &\text{with}
    \begin{cases}
        x(0) = x_A \quad x(T) = x_B \\ 
        y(0) = y_A \quad y(T) = y_B\\
        z(0) = z_A \quad z(T) = z_B\\
        u(0) = u_A \quad u(T) = u_B\\
        v(0) = v_A \quad v(T) = v_B\\
        w(0) = w_A \quad w(T) = w_B\\
    \end{cases}
  \end{aligned}
$$

This kind of optimization problem can be solved via the Pontryagin minimum principle.

The hamiltonian of the system is:
$$
H = \lambda_1 u + \lambda_2 v + \lambda_3 w + \lambda_4 F_x + \lambda_5 F_y + \lambda_6 (F_z-g) + \dfrac{1}{2}(F_x^2+F_y^2+F_z^2)
$$

So the set of equations resulting from Pontryagin principle is:
$$
\begin{aligned}
\pdv{H}{F_x} = \lambda_4 + F_x = 0
&\implies
F_x = - \lambda_4
\\
\pdv{H}{F_y}=0
&\implies F_y = -\lambda_5
\\
\pdv{H}{F_z}=0
&\implies F_z = -\lambda_6
\end{aligned}
$$

$$
\begin{aligned}
\dot \lambda_1 = - \pdv{H}{x} = 0
&\implies
\lambda_1 = \lambda_{10}
\\
\dot \lambda_2 = - \pdv{H}{y} = 0
&\implies
\lambda_2 = \lambda_{20}
\\
\dot \lambda_3 = - \pdv{H}{z} = 0
&\implies
\lambda_3 = \lambda_{30}
\\
\dot \lambda_4 = - \pdv{H}{u} = - \lambda_1
&\implies
\lambda_4 = - \lambda_{10} t + \lambda_{40}
\\
\dot \lambda_4 = - \pdv{H}{v} = - \lambda_2
&\implies
\lambda_5 = - \lambda_{20} t + \lambda_{50}
\\
\dot \lambda_6 = - \pdv{H}{w} = - \lambda_3
&\implies
\lambda_6 = - \lambda_{30} t + \lambda_{60}
\end{aligned}
$$

The optimal dynamics is then described by the following equations:
$$
\begin{aligned}
	\dot u &= \lambda_{10} t + \lambda_{40} \\
	\dot v &= \lambda_{20} t + \lambda_{50} \\
	\dot w &= \lambda_{30} t + \lambda_{60} - g \\
	\dot x &= u \\
	\dot y &= v \\
	\dot z &= w \\
\end{aligned}
$$

Which solution is:
$$
\begin{aligned}
	u(t) &= \lambda_{10} \dfrac{t^2}{2} + \lambda_{40} t + u_A \\
	v(t) &= \lambda_{20} \dfrac{t^2}{2} + \lambda_{50} t + v_A \\
	w(t) &= \lambda_{30} \dfrac{t^2}{2} + (\lambda_{60}-g) t + w_A\\
	x(t) &= \lambda_{10} \dfrac{t^3}{6} + \lambda_{40} \dfrac{t^2}{2} + u_A t + x_A\\
	y(t) &= \lambda_{20} \dfrac{t^3}{6} + \lambda_{50} \dfrac{t^2}{2} + v_A t + y_A\\
	z(t) &= \lambda_{30} \dfrac{t^3}{6} + (\lambda_{60} - g) \dfrac{t^2}{2} + v_A t + z_A\\ \\
\end{aligned}
$$

The values of $\lambda_{i0}$ with $i=1\dots6$ are computed considering that
$$
\begin{aligned}
u(T) = u_B \\
v(T) = v_B \\
w(T) = w_B \\
x(T) = x_B \\
y(T) = y_B \\
z(T) = z_B
\end{aligned}
$$
