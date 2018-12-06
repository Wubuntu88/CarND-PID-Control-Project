# CarND-PID-controller writeup

--

## Effects of the PID terms in the controller

The PID acronym of the PID controller corresponds to the 
'proportional', 'integral', and 'derivative' components of the controller.
In the case of this assignment, the PID controller will help steer the car.
the equation for the output of the controller is the following:

steering = -K_p * CTE - K_d * diff_CTE - K_i * int_CTE

* P: the P term is the CTE (cross track error). This term corresponds to how off the steering angle is from the direction the car should be going.  This term will allow the controller to move the car directly in the 
direction it needs to turn.
* diff_CTE : CTE(t) - CTE(t-1).  This term helps the controller avoid oversteering and ping ponging back and forth.  When the vehicle is converging to the correct position, CTE(t) - CTE(t-1) will be the opposite sign of CTE, thus providing a moderating effect on the the 'P' term.  This 'D' helps in avoiding the repeated oversteering problems of a 'P' controller.
* int_CTE : sum of all CTEs over time.  This term helps the controller when there is a bias in the behavior of the vehicle, such as if the wheels were misaligned.
* K_p, K_d, and K_i are coeffients to the 'P', 'D', and 'I' terms that adjust the strength to which 
each term is multiplied.  For example, a high absolute value of the 'K_p' term will result in high oscillations and oversteering.  A high absolute value of the 'K_d' term will moderate the oversteering.

Here are some examples of the different weights for each term of the PID controller.

#### low abs(p) abs(d) == 0 - in this case the vehicle slowly oscillates out of control.
##### P=-0.13_I=0.0_D=0.0
[alt small_p_small_d](videos/PID_P=-0.13_I=0.0_D=0.0.mov)

#### low abs(p) high abs(d) - in this case the vehicle does not strongly converge to the center of the road.  It does, however, stay on the road.
##### P=-0.13_I=0.0_D=-7.0
[alt small_p_high_d](videos/PID_P=-0.13_I=0.0_D=-7.0.mov)

#### high abs(p) high abs(d) - in this case the vehicle will slowly oscillate until the oscillations will get out of control.
##### P=-3.13_I=0.0_D=-7.0
[alt high_p_high_d](videos/PID_P=-3.13_I=0.0_D=-7.0.mov)


#### medium abs(p) hight abs(d) - in this case the car has minor oscillations driving straight and handles the curves well.
##### P=-0.3_I=0.0_D=-7.0
[alt medium_p_high_d](video/PID_P=-0.3_I=0.0_D=-7.0.mov)

I tuned the parameters manually.  I observed the behavior of the first 3 videos and make changes to the parameters accordingly.  For the first  set of parameters (P=-0.13_I=0.0_D=0.0), I observed that the car quickly oscillated out of control with the derivative term equal to 0.

I set the derivative term to -7, (P=-0.3_I=0.0_D=-7.0). The car did not oscillate out of control, but it did ping pong enough on the straight away road portions that it would be annoying for a passenger.

I then set the P value to -3.13 for the following set of parameters: (P=-3.13_I=0.0_D=-7.0).  This made for oscillations that became out of control.

The final set of parameters I tested were (P= -0.13, I= 0.0, D= -7.0).  I chose P= -0.13 because I wanted to lower the p term to stop the ping pong aspect.  This succeeded in stoping the ping ponging.

My final set of parameters were (P= -0.13, I= 0.0, D= -7.0).  These parameters gave a nice balance between a smooth ride on a straight away and handling the corners without going off the road.
