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
  \begin{cases}
    \min\limits_{F_x, F_y, F_z} J = \min\limits_{F_x, F_y, F_z} \int\limits_{0}^{T} \dfrac{1}{2}(F_x u + F_y v + F_z w)^2 dt \\
    \dot x = u \\
    \dot y = v \\
    \dot z = w \\
    \dot u = F_x \\
    \dot v = F_y \\
    \dot w = F_z - g \\
    x(0) = x_A \quad x(T) = x_B \\ 
    y(0) = y_A \quad y(T) = y_B\\
    z(0) = z_A \quad z(T) = z_B\\
    u(0) = u_A \quad u(T) = u_B\\
    v(0) = v_A \quad v(T) = v_B\\
    w(0) = w_A \quad w(T) = w_B\\
  \end{cases}
$$
