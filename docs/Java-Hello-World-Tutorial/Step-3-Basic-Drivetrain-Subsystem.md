# Basic Drivetrain Subsystem

Now that we have a blank copy of this project on our computer, we can start writing some code. The first thing we will do is make our robot drive around using a joystick for input from a driver.

## Create The Subsystem

Whenever we have hardware on our robot that we would like to control with our robot program, we need to add this hardware to a ```subsystem```. A subsystem is a Java class which gives the rest of our program an interface for controlling hardware components.

Our ```Drivetrain``` subsystem will consist of 4 motor controllers. 2 Talon SRX controllers act as primary controllers, and 2 Victor SPX controllers follow the Talon SRX controllers. We will have some private member variables which represent the physical motor controllers. We will also have public methods which will tell the controllers to move a certain direction

First, let's create the boilerplate code. In VS Code, right click on the folder ```/src/main/java/frc/robot/subsystems```, and choose ```Create a new class/command```. Select ```Subsystem (New)```, and name the subsystem ```Drivetrain```. You should now have a new file in the ```subsystems``` folder called ```Drivetrain.java```, and it should look like this:

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Drivetrain extends SubsystemBase {
  /** Creates a new Drivetrain. */
  public Drivetrain() {}

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}
```

Great, now we have the basic code necessary for our subsystem to exist.

## Add The Hardware

Let's go ahead and declare our private member variables, which we said we will use for representing the hardware in our code. Insert the following lines:

```java
import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import com.ctre.phoenix.motorcontrol.can.WPI_TalonSRX;
import com.ctre.phoenix.motorcontrol.can.WPI_VictorSPX;


public class Drivetrain extends SubsystemBase {

  private final WPI_TalonSRX leftMain;
  private final WPI_TalonSRX rightMain;
  private final WPI_VictorSPX leftFollower;
  private final WPI_VictorSPX rightFollower;
  private final DifferentialDrive diffDrive;

  /** Creates a new Drivetrain. */
  public Drivetrain() {}

...
```

We should add a constructor field to this class, so that the subsystem will be created with references to its corresponding hardware. Let's add the following section to the ```Drivetrain``` constructor:

```java
  /** Creates a new Drivetrain. */
  public Drivetrain() {
    leftMain = new WPI_TalonSRX(1);
    rightMain = new WPI_TalonSRX(2);
    leftFollower = new WPI_VictorSPX(3);
    rightFollower = new WPI_VictorSPX(4);
  }
```

Now we have 4 motor controller objects available to our subsystem, which correspond to the physical controllers on the CAN bus with ID's 1-4. The last step for setting up these controllers is to configure their onboard settings. We first want to clear any old settings, then apply our new ones. Add the following lines directly under the last ones, inside the constructor:

```java
    // Clear any existing configurations
    leftMain.configFactoryDefault();
    rightMain.configFactoryDefault();
    leftFollower.configFactoryDefault();
    rightFollower.configFactoryDefault();

    // Configure motor inversion
    leftMain.setInverted(false);
    rightMain.setInverted(true);
    leftFollower.setInverted(false);
    rightMain.setInverted(true);

    // Set follower mode
    leftFollower.follow(leftMain);
    rightFollower.follow(rightMain);

// Create a DifferentialDrive object for computing motor output from
    // joystick directional inputs
    diffDrive = new DifferentialDrive(leftMain, rightMain);

```

Awesome job! We now have a very basic subsystem. It has 4 motor controllers which correspond to physical controllers on the robot, and these controllers are configured to operate together as a differential system (more on this later).

## Add Methods To Control The Subsystem

Hopefully you noticed earlier that we declared our motor controllers as ```private final```. In Java, this means that the variable is not accessible by anybody except for the class itself, and that the variable will always point to the same object (although the values inside can and do change). We are doing this primarily for security reasons. We don't want to accidentally allow some other part of our program to directly control a single motor without us knowing it, rather, we want the rest of our program to simply call methods like ```driveForward()``` and let the subsystem handle the rest. This is generally considered good practice, and is a good layer of protection so that we don't damage our robot (or ourselves).

We told Java that we don't want anyone else touching our motor controllers, so we need to have some ```public``` methods which let our program use the motors. Add the following lines below our finished ```Drivetrain()``` constructor:

```java
  /** Drives the robot using ArcadeDrive style control */
  public void arcadeDrive(double throttle, double rotation) {
    driveDifferential.arcadeDrive(throttle, rotation);
  }
```

We just did quite a lot in those 4 lines. We created a ```public void``` method for our subsystem, which means that the rest of our robot program can call this method, and it will not return any values. The method accepts 2 inputs, which are ```double``` values (they should be between -1.0 and 1.0). The method itself calls the ```arcadeDrive()``` method on our ```diffDrive``` object, which will move the robot depending on the input from a joystick. The ```diffDrive``` object is of the ```DifferentialDrive``` class, which is a helpful class from WPILib which translates joystick inputs to actual motor outputs.

Congratulations! You now have a working ```Drivetrain``` subsystem, which is capable of taking inputs from a joystick and applying power to the physical motors on the robot. The full ```Drivetrain.java``` file is included below:

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import com.ctre.phoenix.motorcontrol.can.WPI_TalonSRX;
import com.ctre.phoenix.motorcontrol.can.WPI_VictorSPX;

public class Drivetrain extends SubsystemBase {

  private final WPI_TalonSRX leftMain;
  private final WPI_TalonSRX rightMain;
  private final WPI_VictorSPX leftFollower;
  private final WPI_VictorSPX rightFollower;
  private final DifferentialDrive diffDrive;

  /** Creates a new Drivetrain. */
  public Drivetrain() {
    // Instantiate our motor controllers
    leftMain = new WPI_TalonSRX(1);
    rightMain = new WPI_TalonSRX(2);
    leftFollower = new WPI_VictorSPX(3);
    rightFollower = new WPI_VictorSPX(4);

    // Clear any existing configurations
    leftMain.configFactoryDefault();
    rightMain.configFactoryDefault();
    leftFollower.configFactoryDefault();
    rightFollower.configFactoryDefault();

    // Configure motor inversion
    leftMain.setInverted(false);
    rightMain.setInverted(true);
    leftFollower.setInverted(false);
    rightMain.setInverted(true);

    // Set follower mode
    leftFollower.follow(leftMain);
    rightFollower.follow(rightMain);

    // Create a DifferentialDrive object for computing motor output from
    // joystick directional inputs
    diffDrive = new DifferentialDrive(leftMain, rightMain);
  }

  /** Drives the robot using ArcadeDrive style control */
  public void arcadeDrive(double throttle, double rotation) {
    diffDrive.arcadeDrive(throttle, rotation);
  }

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}
```
