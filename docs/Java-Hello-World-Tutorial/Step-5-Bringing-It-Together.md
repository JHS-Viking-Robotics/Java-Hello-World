# Bringing It Together

Now that we have a working subsystem and command, we need to write a main program which combines our robot components. This program will also be responsible for scheduling commands to run, and collecting input from the controllers in the Driver Station.

## Structuring The Main Program

FRC robotics projects operate like this:

1. The RoboRIO runs ```Main```
2. ```Main``` creates a ```Robot``` object to interface with the FRC framework (like the driver station, etc.)
3. ```Robot``` creates a ```RobotContainer``` object, creates 1 copy of the autonomous command, and tells the ```CommandScheduler``` to run once every periodic task loop
4. ```RobotContainer``` creates 1 of each subsystem, and registers commands and button configurations with the ```CommandScheduler```

```Main.java``` and ```Robot.java``` are already written for us, so all we need to do is write ```RobotContainer.java```. Let's first declare our ```Drivetrain```, ```TeleopDrive```, and an Xbox controller as member variables:

```java
import edu.wpi.first.wpilibj.GenericHID;
import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj.XboxController.Button;
import edu.wpi.first.wpilibj.GenericHID.Hand;
import frc.robot.commands.ExampleCommand;
import frc.robot.commands.TeleopDrive;
import frc.robot.subsystems.Drivetrain;
import frc.robot.subsystems.ExampleSubsystem;
import edu.wpi.first.wpilibj2.command.Command;
import edu.wpi.first.wpilibj2.command.button.JoystickButton;

/**
 * This class is where the bulk of the robot should be declared. Since Command-based is a
 * "declarative" paradigm, very little robot logic should actually be handled in the {@link Robot}
 * periodic methods (other than the scheduler calls). Instead, the structure of the robot (including
 * subsystems, commands, and button mappings) should be declared here.
 */
public class RobotContainer {
  // The robot's subsystems and commands are defined here...
  private final ExampleSubsystem m_exampleSubsystem;
  private final Drivetrain m_drivetrainSubsystem;

  private final Command m_autoCommand;
  private final Command m_teleopDriveCommand;

  private final XboxController m_driveController;
```

Awesome! We will instantiate these objects in the next steps. You may have noticed that all of our objects have weird names starting with ```m_```. This is part of [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation) that is present in all WPILib code by default, and it means that the object in question is a member variable. We will not be using this notation throughout our project, but here we will keep it to indicate which objects should hang around ```RobotContainer``` forever until the robot shuts off, rather than being created/destroyed frequently like many Java classes.

## Adding Subsystems

Now in the ```RobotContainer()``` constructor we can instantiate our subsystem. Add the following lines of code to the constructor:

```java
    // Instantiate robot subsystems
    m_exampleSubsystem = new ExampleSubsystem();
    m_drivetrainSubsystem = new Drivetrain();
```

In the future if we need to operate on the ```Drivetrain``` subsystem, we should do it using ```m_drivetrainSubsystem``` rather than creating a new one each time.

## Adding Commands

Now we should add our commands. Again in the ```RobotContainer()``` constructor:

```java
    // Instantiate robot commands
    m_autoCommand = new ExampleCommand(m_exampleSubsystem);
    m_teleopDriveCommand = new TeleopDrive()
```

You will see lots of warning squiggles because we did not pass any arguements to the ```TeleopDrive()``` constructor, but this is ok for now. We are fixing this in the next section.

## Adding Controllers And Input Streams

We also need to instantiate our Xbox controller. Add the following lines to the beginning of the constructor, before any of the commands are instantiated:

```java
    // Instantiate controllers and input devices
    m_driveController = new XboxController(0);
```

In our ```TeleopDrive``` command, we said that we need a stream of ```double``` values. We would like to take the input from the left hand X and Y joystick on the Xbox controller, and pass the values into our command as a stream. To do this, we will need to use an advanced concept called lambdas and supplier streams. You can learn more about these later, but for now just change the line that we added earlier for our ```TeleopDrive``` command:

```java
    // Instantiate robot commands
    m_autoCommand = new ExampleCommand(m_exampleSubsystem);
    m_teleopDriveCommand = new TeleopDrive(
        m_drivetrainSubsystem,
        () -> m_driveController.getY(Hand.kLeft),
        () -> m_driveController.getX(Hand.kLeft));
```

If you get angry squiggles, make sure to save the file and check for any typos. If you are still getting an error about ```The constructor TeleopDrive(Drivetrain, () -> {}, () -> {}) is undefined```, then you may need to restart your Visual Studio Code to force Intellisense to update.

## Scheduling Commands

At this point we have everything instantiated, and we just need to schedule some commands to run. Add the following code to 

```java
  private void configureButtonBindings() {
    new JoystickButton(m_driveController, Button.kA.value)
        .whenHeld(m_teleopDriveCommand);
  }
```

Sweet! This tells the ```CommandScheduler``` that we want to run the ```m_teleopDriveCommand``` from earlier whenever the driver presses and holds the ```A``` button.

We used this configuration for this demo so that you know how to schedule commands later, but normally you probably don't want to always hold down a button in order to drive the robot. You can call ```Subsystem.setDefaultCommand(Command)``` during the ```RobotContainer()``` constructor to set a command to run anytime a subsystem is not in use. Here is an example:

```java
  public RobotContainer() {
    ...
    // Set a subsystem default command
    m_drivetrainSubsystem.setDefaultCommand(m_teleopDriveCommand);
```

You should now have working code which will let you drive your robot! There are a lot of steps to get to this point, so don't worry if you are not confident on everything just yet. You can always refer back to this tutorial, check out the resources listed at the beginning, or talk to other team members for help.

Here is a copy of the completed ```RobotContainer.java``` file:

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot;

import edu.wpi.first.wpilibj.GenericHID;
import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj.XboxController.Button;
import edu.wpi.first.wpilibj.GenericHID.Hand;
import frc.robot.commands.ExampleCommand;
import frc.robot.commands.TeleopDrive;
import frc.robot.subsystems.Drivetrain;
import frc.robot.subsystems.ExampleSubsystem;
import edu.wpi.first.wpilibj2.command.Command;
import edu.wpi.first.wpilibj2.command.button.JoystickButton;

/**
 * This class is where the bulk of the robot should be declared. Since Command-based is a
 * "declarative" paradigm, very little robot logic should actually be handled in the {@link Robot}
 * periodic methods (other than the scheduler calls). Instead, the structure of the robot (including
 * subsystems, commands, and button mappings) should be declared here.
 */
public class RobotContainer {
  // The robot's subsystems and commands are defined here...
  private final ExampleSubsystem m_exampleSubsystem;
  private final Drivetrain m_drivetrainSubsystem;

  private final Command m_autoCommand;
  private final Command m_teleopDriveCommand;

  private final XboxController m_driveController;

  /** The container for the robot. Contains subsystems, OI devices, and commands. */
  public RobotContainer() {
    // Instantiate controllers and input devices
    m_driveController = new XboxController(0);

    // Instantiate robot subsystems
    m_exampleSubsystem = new ExampleSubsystem();
    m_drivetrainSubsystem = new Drivetrain();

    // Instantiate robot commands
    m_autoCommand = new ExampleCommand(m_exampleSubsystem);
    m_teleopDriveCommand = new TeleopDrive(
        m_drivetrainSubsystem,
        () -> m_driveController.getY(Hand.kLeft),
        () -> m_driveController.getX(Hand.kLeft));

    // Configure the button bindings
    configureButtonBindings();
  }

  /**
   * Use this method to define your button->command mappings. Buttons can be created by
   * instantiating a {@link GenericHID} or one of its subclasses ({@link
   * edu.wpi.first.wpilibj.Joystick} or {@link XboxController}), and then passing it to a {@link
   * edu.wpi.first.wpilibj2.command.button.JoystickButton}.
   */
  private void configureButtonBindings() {
    new JoystickButton(m_driveController, Button.kA.value)
        .whenHeld(m_teleopDriveCommand);
  }

  /**
   * Use this to pass the autonomous command to the main {@link Robot} class.
   *
   * @return the command to run in autonomous
   */
  public Command getAutonomousCommand() {
    // An ExampleCommand will run in autonomous
    return m_autoCommand;
  }
}
```
