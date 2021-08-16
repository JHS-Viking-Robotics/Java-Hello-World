# WPI Project Overview

Now that we have a copy of this repository on our computer, we can start to look at the different components and what they do. Check out the [official documentation](//TODO:-add-link-to-wpi-docs) for more detailed information on each part. For this tutorial, we will only talk about a few files, and also how to build and deploy code to a robot.

## RobotContainer

Command-style programming ```abstracts``` robot hardware into Java classes, which are managed by the class ```RobotContainer```. This class should own each subsystem, any commands which should be used, game controllers and joysticks, and should map said controllers to said commands.

## Subsystems

Each group of hardware devices on the robot should be managed in a subsystem class in the code, which is then owned by [RobotContainer](#robotcontainer). This level of ```abstraction``` allows us to call for a subsystem to do some thing or report some value. This is much simpler and cleaner than directly telling a motor to do x, or a piston to output y pressure, or calculating a position from a sensor.

A subsystem class should be responsible for managing all of its corresponding hardware on the robot, monitoring and updating the state of the physical system, and providing diagnostic information to the ```Driver Station```. In this tutorial, we will be creating a ```Drivetrain``` subsystem which contains 4 motor controllers and 2 encoders.

## Commands

The most useful part of a WPI command-style program is the built-in ```CommandScheduler``` class. This is a Java singleton class managed by ```Robot.java```, meaning that there can only be one instance at a time and it needs to be owned by ```Robot.java``` (the code for this is already set up by default). The scheduler takes in ```Command``` classes that you create, and will run them depending on what buttons you choose as triggers. You can find out more in the [official WPI docs](//TODO:add-wpi-docs-link).

A command should be able to call methods on our subsystems, and should not have to worry about much else. This frees us up to create very intricate commands, and enable very useful functionality on the robot. For this tutorial, we will simply create some basic ```DriveTeleop```, ```DriveAutonomous```, and ```DriveStraight``` commands which use our ```Drivetrain``` subsystem.

## Constants

Java has built in class modifiers for constant values, and we add these to ```Constants.java``` for use in our code. We add the values and subclasses to ```Constants.java``` with the ```public static final``` modifiers, and import them to our other classes with the following:

```java
import frc.robot.Constants.SubClass;
```

Examples for good constants are CAN Bus ID's, default configurations, PIDF constants, etc.
