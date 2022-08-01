---
layout: post
title:  "Robotics Notes 2"
date:   2022-8-1 23:14:00 +0800
categories: [Notes, Robotics]
---

## Movement of Robots

- Locomotion: Mechanical process that generates motion (Legs, Wheels, Flippers...)
- Kinematics: Position, velocity, acceleration...
- Dynamics: motion caused by forces ($\frac{d}{dx} mv = F$)
- Motion Control: choice of input to generate motion for a goal  
  
--------------

Pose: body-fixed frame, spatial variable, position (x, y) and orientation (ex, ey) (for 2D)

Pose of a robot: origin of BFF, orientation of BFF, w.r.t. reference frame {A}

Pose in 2D: $\begin{bmatrix}
    x \\
    y \\
    \theta
\end{bmatrix}$, (x, y) is the location of robot centre, $\theta$ is the relative orientation of the robot.

Velocity in 2D:   
- body-frame velocity in inertial coordinates: d/dt (x y) = (vx vy)
- body-frame velocity in body coordinates: (u 0) = $\left( \begin{matrix}
    cos(\theta) & sin(\theta) \\
    -sin(\theta) & cos(\theta) \\
\end{matrix} \right) \left( \begin{matrix}
    v_x \\
    v_y \\
\end{matrix} \right)$
- Angular velocity $\theta$: anti-clockwise, body velocity. 

--------------

Angles of rotation of the wheels are configuration varibles or states.

--------------

Distinction between configuration state (internal) and pose state (external)  

Configuration: (q1, q2, q3,..., qn)  
Pose: (PE, RE)

Pose = $f$(Configuration)  
Configuration = $f^{-1}$(Pose)  

f is not one-to-one.  

-------------

Wheel angles are not related algebraically to the position.
