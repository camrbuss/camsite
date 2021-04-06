---
layout: projects
name: Pen Plotter Inverse Kinematics
title: Pen Plotter Inverse Kinematics
description: Non-linear Inverse Kinematics with ROS2 and an ODrive motor controller
image: /assets/inversekinematics.png
permalink: /inversekinematics/
---
<center><img src="{{ '/assets/inversekinematics/inversekinematics2.png' | prepend: site.baseurl }}" /></center><br/>

So I (and my family, thanks!) paid thousands of dollars for a mechanical engineering degree, but yet in my day job I really only use my problem solving skills. To prove to myself that I did learn something in the controls, mechatronics, differential equations, and electronics classes that I took, I wanted to solve a non-trivial kinematics equation and implement it in real life.

Stepper motors are all our favorite “hammer” when it comes to motion control, but I have always wanted to understand how more complex coordinated motion controllers work when talking to more advanced motor controllers. What this mainly means is writing a controller to output velocity or acceleration, instead of position. With this requirement, I selected the ODrive motor controller as it is a great open-source project that accepts CAN bus messages to control brushless dc motors via acceleration, velocity, or position mode.

The next choice I had to make was, from what device should I send the ODrive commands. Initially I wanted to use a STM32F4 board, but because I had never implemented inverse kinematics before, I figured I should stick with something that had better visualization support. I had used ROS before, and saw that ROS2 was coming out, so I jumped on the ROS2 train. ROS2 has a slight learning curve, but with the power of RVIZ2 and URDF, it was easy to stay excited. In order to get ROS2 talking to ODrive, I could have used the python3 ODrive tool, but I didn’t like the idea of non-deterministic USB, so I kept with CAN bus in the hopes that I could go fully embedded in the future. To get my computer talking to the ODrive via CAN, I created a [SocketCAN driver in ROS2](https://github.com/camrbuss/ros2_odrive_can).

After getting the main hardware and software building blocks mostly sorted out, it was onto the actual inverse kinematics implementation. Of course I could have used the MoveIt ROS package, but the goal of this project was to use my math knowledge, so I used sympy to solve for the Jacobian. In general I just used the law of cosines to solve the forward kinematics, calculus to find the derivative of position, and some linear algebra to find the Jacobian. See the derivation below.

Finally I needed a way to send the machine paths, and I didn’t feel like writing a G-code interpreter, so I wrote a [Inkscape plugin](https://github.com/camrbuss/nodes_to_csv) to output a CSV file of text/paths.

With some debugging using RVIZ2, and rqt I was able to get the whole machine moving by specifying end effector positions. It was a bit surprising how “unique” (small) the work area was, but it didn’t bother me too much as I chose the configuration so that I couldn’t just grab the kinematic equations off the internet.

Video of it plotting:

<iframe src="https://photos.google.com/share/AF1QipPPnjT9cYvOpzw3maEUniWhxGXRZ54Yp2YPvis3V7f94QGGNLeYnc7Xe7INOP2KOQ/photo/AF1QipNL1OQD7o0dDj5wc_waviOCP1g5O4mTzMKHk847?key=WVc0NnNYMFV6QTJWNEdINXJ4cFI5OENySjBpcG13"> </iframe>


<center><img src="{{ '/assets/inversekinematics/inversekinematics3.png' | prepend: site.baseurl }}" /></center><br/>
<center><img src="{{ '/assets/inversekinematics/inversekinematics4.png' | prepend: site.baseurl }}" /></center><br/>

Implement in C++ with a(t) and b(t) coming in as discrete measurements, x'(t) and y'(t) being set by the user, and a'(t) and b'(t) being sent to the motor controllers.

``` cpp
float j = -L3 * (std::pow(L1, 2) - std::pow(L2, 2) - std::pow(a, 2) + 2.0 * a * b - std::pow(b, 2));
float k = A1 + std::acos((1.0 / 2.0) * (std::pow(L1, 2) - std::pow(L2, 2) + std::pow(a - b, 2)) / (L1 * (-a + b)));
float l = 2.0 * L1 * sqrt(1 - 1.0 / 4.0 * std::pow(std::pow(L1, 2) - std::pow(L2, 2) + std::pow(a - b, 2), 2) / (std::pow(L1, 2) * std::pow(a - b, 2))) * std::pow(a - b, 2);
float f = j * std::sin(k) / l;
float g = j * std::cos(k) / l;

this->a_vel_setpoint_ = x_vel_ - ((-1.0f + f) * y_vel_ / g);
this->b_vel_setpoint_ = x_vel_ - ((f * y_vel_) / g);
```