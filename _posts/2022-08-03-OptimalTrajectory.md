---
layout: post
title: What is the best way to go from A to B?
---

The problem I want to discuss in this post is how it is possible to optimize a trajectory of a body.

This kind of problem is very present in engineering, some examples are:

1. spacecraft trajectory optimization
2. drone trajectory optimization
3. car trajectory optimization
4. robotic arm motion planning

For the sake of simplicity let's consider the case of a point mass moving in 3D space that want to go from a point A to a point B with initial velocity and final velocity specified.

Before continuing solving the problem assume:

1. the mass is 1kg
2. no friction forces are considered
3. no energy recovery/accumulation system is possible
4. the control force can be applied in all directions and has no upper/lower limit

In order to solve this problem we need to define in a precise way what we mean with optimal trajectory.

One possible criteria of optimality is the total energy used to go from A to B, so, the optimal path from A to B will minimize the total energy required to follow that path.

In order to keep the calculation simple enough, the optimality criteria discussed here is half the average value of the squared force multiplied by the time needed to go from A to B, namely,

$$
J = \int\limits_{0}^{T} \dfrac{1}{2} (F_x^2 + F_y^2 + F_z^2) \dd t
$$

where $J$ denotesthe cost function while $F_x$, $F_y$ and $F_z$ denotes the control force acting on the body in the $x$, $y$, and $z$ directions.

Once we have clarified that the optimal trajectory will minimize the cost function just defined, in order to solve the problem, we need to formalize it.

The optimal trajectory can be found by solving the following constrained minimization problem:

$$
  \min\limits_{F_x, F_y, F_z} J = \min\limits_{F_x, F_y, F_z} \int\limits_{0}^{T} \dfrac{1}{2}(F_x^2 + F_y^2 + F_z^2) \dd t
$$

$$
  \text{subject to }
  \begin{cases}
      \dot x = u \\
      \dot y = v \\
      \dot z = w \\
      \dot u = F_x \\
      \dot v = F_y \\
      \dot w = F_z - g \\
  \end{cases}
  \quad \text{with }
  \begin{cases}
      x(0) = x_A \quad x(T) = x_B \\ 
      y(0) = y_A \quad y(T) = y_B\\
      z(0) = z_A \quad z(T) = z_B\\
      u(0) = u_A \quad u(T) = u_B\\
      v(0) = v_A \quad v(T) = v_B\\
      w(0) = w_A \quad w(T) = w_B\\
  \end{cases}
$$

where $x$, $y$, and $z$ are functions of time that denotes the coordinates of the point mass in the space; $u$, $v$, and $w$ are functions of time that denotes the velocity of the point mass in the $x$, $y$, and $z$ directions; $g$ is a constant that represent the gravity acceleration ($9.81$ m/s$^2$) and $T$ is a parameter that represent the final time at which the point mass will reach point $B$.

The dot over a function represents the time derivative of that function. For example, $\dot x$ is the time derivative of $x$; $\dot y$ is the time derivative of $y$ and so on. 

Note: minimizing $J$ is not enough, we need also to impose the equations of the dynamics of the mass in order to achieve a result that have sense. If we don't impose that, for example, $\dot x = u$, at the end of the optimization process, the quantity $u$ will not represents the velocity along the $x$ direction. Moreover, if we don't impose that $\dot u = F_x$, the mass motion will not respect the Newton's laws of motion.

This kind of optimization problem can be solved via the [Pontryagin minimum (or maximum) principle](https://en.wikipedia.org/wiki/Pontryagin's_maximum_principle).

To apply this principle we need to define the hamiltonian $H$ of the system as:

$$
  H = \lambda_1 u + \lambda_2 v + \lambda_3 w + \lambda_4 F_x + \lambda_5 F_y + \lambda_6 (F_z-g) + \dfrac{1}{2}(F_x^2+F_y^2+F_z^2)
$$

Pontryagin's principle says that the optimal control strategy shall obey the following equations:

$$
\begin{cases}
    \pdv{H}{F_x} = 0 \\
    \pdv{H}{F_y}=0\\
    \pdv{H}{F_z}=0
  \end{cases}
  \quad \text{and} \quad
  \begin{cases}
    \dot \lambda_1 = - \pdv{H}{x} \\
    \dot \lambda_2 = - \pdv{H}{y} \\
    \dot \lambda_3 = - \pdv{H}{z} \\
    \dot \lambda_4 = - \pdv{H}{u} \\
    \dot \lambda_4 = - \pdv{H}{v} \\
    \dot \lambda_6 = - \pdv{H}{w} 
  \end{cases}
$$

The solutions of the above equations are:

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

Substituting these results in the equations of the dynamics of the mass will lead to:

$$
\begin{cases}
    \dot u = \lambda_{10} t + \lambda_{40} \\
    \dot v = \lambda_{20} t + \lambda_{50} \\
    \dot w = \lambda_{30} t + \lambda_{60} - g \\
    \dot x = u \\
    \dot y = v \\
    \dot z = w \\
  \end{cases}
$$

So the optimal trajectory of the point mass in time will be the following:

$$
\begin{aligned}
    u(t) &= \lambda_{10} \dfrac{t^2}{2} + \lambda_{40} t + u_A \\
    v(t) &= \lambda_{20} \dfrac{t^2}{2} + \lambda_{50} t + v_A \\
    w(t) &= \lambda_{30} \dfrac{t^2}{2} + (\lambda_{60}-g) t + w_A\\
    x(t) &= \lambda_{10} \dfrac{t^3}{6} + \lambda_{40} \dfrac{t^2}{2} + u_A t + x_A\\
    y(t) &= \lambda_{20} \dfrac{t^3}{6} + \lambda_{50} \dfrac{t^2}{2} + v_A t + y_A\\
    z(t) &= \lambda_{30} \dfrac{t^3}{6} + (\lambda_{60} - g) \dfrac{t^2}{2} + w_A t + z_A\\ \\
  \end{aligned}
$$

where the values of $\lambda_{10}, \dots, \lambda_{60}$ can be computed considering that

$$
\begin{cases}
    u(T) = u_B \\
    v(T) = v_B \\
    w(T) = w_B \\
    x(T) = x_B \\
    y(T) = y_B \\
    z(T) = z_B
  \end{cases}
$$
