---
title: "Step 4: Basic Drive Command"
parent: "Tutorial: Java-Hello-World"
nav_order: 4
---
# Basic Drive Command

In the previous step, we created a new subsystem for interacting with our drivetrain from our code. Now we need to create a ```TeleopDrive``` command for when we want to drive our robot, as well as connect our Xbox controller as inputs for the command.

## Create The Command

Commands in our FRC project are high level actions or sequences for our robot to perform. A command can be fed to the ```CommandScheduler```, which will run commands and make sure that each subsystem is only part of 1 command at a time. The scheduler will run once every 20ms control cycle. Again, you can learn more about Command-Based programming in the [WPILib Documentation](https://docs.wpilib.org/en/latest/docs/software/commandbased/index.html).

Like for our subsystem, we will first need to create boilerplate code. In VS Code, right click on the folder ```/src/main/java/frc/robot/commands```, and choose ```Create a new class/command```. Select ```Command (New)```, and name the command ```TeleopDrive```. You should now have a new file in the ```commands``` folder called ```TeleopDrive.java```, and it should look like this:

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.CommandBase;

public class TeleopDrive extends CommandBase {
  /** Creates a new TeleopDrive. */
  public TeleopDrive() {
    // Use addRequirements() here to declare subsystem dependencies.
  }

  // Called when the command is initially scheduled.
  @Override
  public void initialize() {}

  // Called every time the scheduler runs while the command is scheduled.
  @Override
  public void execute() {}

  // Called once the command ends or is interrupted.
  @Override
  public void end(boolean interrupted) {}

  // Returns true when the command should end.
  @Override
  public boolean isFinished() {
    return false;
  }
}
```

Awesome! There are a few methods that were created for us. When a command is given to the ```CommandScheduler```, it will run the ```initialize()``` method on the first cycle. It will then run the ```execute()``` method every cycle until the method ```isFinished()``` returns ```true```. Once this occurs, the ```end()``` method will be ran, and the command object will be closed.

## Structuring The Command

Now that we have the basics out of the way, we can begin to build our command. The very first thing we need to do is determine which ```subsystems``` are required for the command, and let the CommandScheduler know. We can add the following lines to accomplish this:

```java
import edu.wpi.first.wpilibj2.command.CommandBase;
import frc.robot.subsystems.Drivetrain;

public class TeleopDrive extends CommandBase {

  private final Drivetrain drivetrain;

  /** Creates a new TeleopDrive. */
  public TeleopDrive(Drivetrain drivetrain) {
    // Use addRequirements() here to declare subsystem dependencies.
    this.drivetrain = drivetrain;
    addRequirements(this.drivetrain);
  }
```

Here we imported the ```Drivetrain``` subsystem class that we created earlier, declared a ```private final``` object of that class, and then added our ```Drivetrain``` class as a parameter of the ```TeleopDrive``` command constructor. The other thing we did is assign our member variable ```drivetrain``` to the subsystem that is passed into the constructor, and then feed this subsystem to ```addRequirements()```. This lets the command scheduler know which subsystems our command requires, so that no subsystem can be part of multiple active commands.

## Collecting Controller Input

For our ```TeleopDrive``` command, we intend to feed an Xbox controller to the command as input. However, when we pass normal values like ```double``` into our constructor field, we only receive one value, rather than a constant stream of values like from a controller joystick. The recommended way to handle this is to use a special Java class called a ```Supplier```, and then later we will use another advanced feature called a ```lambda``` to feed this ```Supplier```. For now, we can just add these new lines:

```java
import edu.wpi.first.wpilibj2.command.CommandBase;
import frc.robot.subsystems.Drivetrain;
import java.util.function.DoubleSupplier;

public class TeleopDrive extends CommandBase {

  private final Drivetrain drivetrain;
  private final DoubleSupplier throttle;
  private final DoubleSupplier rotation;

  /** Creates a new TeleopDrive. */
  public TeleopDrive(Drivetrain drivetrain, DoubleSupplier throttle, DoubleSupplier rotation) {
    // Use addRequirements() here to declare subsystem dependencies.
    this.drivetrain = drivetrain;
    this.throttle = throttle;
    this.rotation = rotation;
    addRequirements(this.drivetrain);
  }
```

Now we have member variables called ```throttle``` and ```rotation```, which represent a stream of double values which we will later populate with controller input. When our command is active, we would like to simply feed these values to our subsystem method ```Drivetrain.arcadeDrive```, which requires 2 doubles called ```throttle``` and ```rotation```. Therefore we need to add some code to our command's ```execute()``` method:

```java
  // Called every time the scheduler runs while the command is scheduled.
  @Override
  public void execute() {
    // Drive the robot using ArcadeDrive style control
    // NOTE: The +/- for the y axis on the joystick is inverted from
    // the right-hand-rule
    drivetrain.arcadeDrive(
        -throttle.getAsDouble(),
        rotation.getAsDouble());
  }
```

For our ```Drivetrain.arcadeDrive(double, double)``` method, we supply the doubles from our ```DoubleSupplier``` using the ```getAsDouble()```. This method simply grabs the most recent value from the stream as a ```double```. This way, the ```CommandScheduler``` will check every cycle for a new value from the controller stream, and send it to the subsystem to be applied to the motor controllers.

## Finishing It Up

When our command is scheduled to stop (whether it is over, or another command is starting), the ```CommandScheduler``` will run ```end()``` for the last loop iteration rather than ```execute()```. We can use this as an opportunity to stop our subsystem or clear its settings so that we don't cause issues with another part of the code. Add the following lines to ```TeleopDrive.end(boolean)```:

```java
  // Called once the command ends or is interrupted.
  @Override
  public void end(boolean interrupted) {
    // Set output to 0 for safety
    drivetrain.arcadeDrive(0.0, 0.0);
  }
```

Perfect! Now the subsystem will be instructed to stop moving once we are done, just in case something goes wrong or the next command is not expecting the motors to be running.

Here is a copy of the final ```TeleopDrive.java``` file for reference:

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.CommandBase;
import frc.robot.subsystems.Drivetrain;
import java.util.function.DoubleSupplier;

public class TeleopDrive extends CommandBase {

  private final Drivetrain drivetrain;
  private final DoubleSupplier throttle;
  private final DoubleSupplier rotation;

  /** Creates a new TeleopDrive. */
  public TeleopDrive(Drivetrain drivetrain, DoubleSupplier throttle, DoubleSupplier rotation) {
    // Use addRequirements() here to declare subsystem dependencies.
    this.drivetrain = drivetrain;
    this.throttle = throttle;
    this.rotation = rotation;
    addRequirements(this.drivetrain);
  }

  // Called when the command is initially scheduled.
  @Override
  public void initialize() {}

  // Called every time the scheduler runs while the command is scheduled.
  @Override
  public void execute() {
    // Drive the robot using ArcadeDrive style control
    // NOTE: The +/- for the y axis on the joystick is inverted from
    // the right-hand-rule
    drivetrain.arcadeDrive(
        -throttle.getAsDouble(),
        rotation.getAsDouble());
  }

  // Called once the command ends or is interrupted.
  @Override
  public void end(boolean interrupted) {
    // Set output to 0 for safety
    drivetrain.arcadeDrive(0.0, 0.0);
  }

  // Returns true when the command should end.
  @Override
  public boolean isFinished() {
    return false;
  }
}
```
