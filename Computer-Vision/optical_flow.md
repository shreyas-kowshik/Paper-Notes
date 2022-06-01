Brightness constancy assumption - brightness is almost constant as we go from f(x + dx,y+dy,d + dt) - therefore we can first order approximate ‘f’.

$f_x(u) + f_y(v) + f_t = 0$ - optical flow equation
It has two unf knowns - u and v
u and v constitute the optical flow - we want to find with what velocity and what direction each pixel moves.

We divide the flow into two parts - parallel and normal (in the u-v space of the line obtained from the equation of optical flow).

Horn and Schkunk 
They formulated it like an optimization problem. 
J = $\int\int f_xu + f_yv + f_t + \lambda(u_x^2 + u_y^2 + v_x^2 + v_y^2)$
We know that for all pixels on the same object, they move with almost same velocities. Thus u_x is generally small. U_x measures the change in pixel velocities in the x direction. Thus it is almost zero for all points on the same object. SImilarly for u_y,v_x and v_y. The first term is the brightness constraint while the second is the smoothness constraint.
f_x,f_y and f_t are computed from masks. f_x is actually computed for the two images at t and t+dt and the results are averaged out. This kind of gives us a better estimate of the value rather than taking only one image. For the time domain a mask of [-1] is applied to the first image and of [1] is applied to the second image.

Lucas Kanade
They took a neighborhood of 3x3 around each pixel. Now we have 9 equations of optical flow and two variables u and v. We can invert the coefficient matrix by transposing it and then calculating [u v]^T. But rather we formulate it as an optimization problem and minimise the so as to obtain u and v.

Note - KLT does not work for large flow when a large object moves. This is because of the neighborhood constraint, that we can only get information local to a pixel. In such cases it is used with a pyramid to obtain good results.

See the video for math explanation.
Reference - https://www.youtube.com/watch?v=5VyLAH8BhF8&ab_channel=UCFCRCV
