---
title: "Tutorial: PID Control"
nav_order: 6
has_children: true
---
# Closed-Loop PID Control

This is an intermediate tutorial on how to set up PID control on a simple arm or lift using CTRE TalonSRX onboard PID control.

WPILib has built-in PID controllers and encoder classes, which run on the RoboRIO. They work well, but the RIO only runs once every 20ms. The built-in PID controller on the Talon runs at 1000Hz by default, and has plug-and-play support for CTRE Mag Encoders. This setup works really well, and also allows you to use the Phoenix Tuner tool to tune the controller.

This tutorial will also show you how to configure a subsystem to perform self-health checks and shut down if anything breaks. This can be important not only for protecting people, but also for protecting the robot if a sensor gets dislodged during a competition.
