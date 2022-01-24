---
title: "Step 3: Tune The PID Controller"
parent: "Tutorial: PID Control"
nav_order: 3
---
# Tune The PID Controller

The CTRE Phoenix Tuner lets you connect to the robot and test the Talon PID controller manually, but sometimes you need to test it with more than one system at a time. In the [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/index.html), they outline a way to put the PID controller on the Dashboard to make it easier to tune and test. This method doesn't quite work for the CTRE controllers, so we can use our own method.

> **_Note_** For all of these examples, you should make sure to fill in your experimentally derived PIDF constants, or else nothing will happen

First add some new NetworkTable entries. We will add one for each PIDF constant, and also a control and an indicator for manually moving the Lift.

```java
  private NetworkTableEntry liftP; // kP for Lift Talon closed-loop PID
  private NetworkTableEntry liftI; // kI for Lift Talon closed-loop PID
  private NetworkTableEntry liftD; // kD for Lift Talon closed-loop PID
  private NetworkTableEntry liftF; // kF for Lift Talon closed-loop PID
  private NetworkTableEntry liftSetpoint;   // Desired Lift setpoint in Talon sensor ticks
  private NetworkTableEntry liftPosition;   // NetTables dashboard indicator for Lift position in Talon sensor ticks
```

We configure these in the Lift constructor. You can omit these if you want to manually configure the Shuffleboard on the Driver Station computer. The first step is to set up a new tab on the Shuffleboard, and a Layout for the PIDF constants.

```java
    // We will save this as a reference to a Shuffleboard tab called "Lift"
    ShuffleboardTab shuffleLiftTab = Shuffleboard.getTab("Lift");

    // We can optionally add the PIDF displays to the same layout, which simply
    // keeps them together for readability
    ShuffleboardLayout shuffleLiftPIDLayout = shuffleLiftTab
        .getLayout("Lift PID", BuiltInLayouts.kList)
        .withProperties(
            Map.of(
                "Label position", "LEFT"))
        .withPosition(2, 0)
        .withSize(1, 2);
```

Now lets add the PIDF constants to the Layout so they are all together.

```java
    // Add the PIDF constants to a Layout
    liftP = shuffleLiftPIDLayout
        .add("P", <kP>)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftI = shuffleLiftPIDLayout
        .add("I", <kI>)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftD = shuffleLiftPIDLayout
        .add("D", <kD>)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftF = shuffleLiftPIDLayout
        .add("F", <kF>)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
```

Finally we will add our control slider and position indicator. Note that both will have a range of [-4096, 4096], or one rotation of the encoder, but you should change these to the physical min and max range of your system so that you don't tell the motor to drive the system into itself. This is important so you don't accidentally break anything or blow a motor.

```java
    // Configure Lift setpoint indicator and controller
    liftPosition = shuffleLiftTab
        .add("Lift Position", 0.0)
        .withWidget(BuiltInWidgets.kNumberBar)
        .withProperties(
            Map.of(
                "Min", -4096.0,
                "Max", 4096.0))
        .withSize(4, 1)
        .withPosition(0, 2)
        .getEntry();
    liftSetpoint = shuffleLiftTab
        .add("Lift Setpoint", 0.0)
        .withWidget(BuiltInWidgets.kNumberSlider)
        .withProperties(
            Map.of(
                "Min", -4096.0,
                "Max", 4096.0))
        .withSize(4, 1)
        .withPosition(0, 3)
        .getEntry();
```

With the NetworkTables controls all set up, we can use ```periodic()``` to copy the values from the Dashboard and send them to the Talon, and read the Talon's reported position and put it on the Dashboard.

```java
  @Override
  public void periodic() {
    // This method will be called once per scheduler run
    // Push the Lift PIDF constants from NetworkTable to the Talon
    liftController.config_kP(0, liftP.getDouble(kP));
    liftController.config_kI(0, liftI.getDouble(kI));
    liftController.config_kD(0, liftD.getDouble(kD));
    liftController.config_kF(0, liftF.getDouble(kF));

    // Push the current position from the Talon to the NetworkTable
    liftPosition.setDouble(liftController.getSelectedSensorPosition());

    // Send the new position to the motor. You should remove this on a competition bot
    this.set(liftSetpoint.getDouble(0.0));
  }
```

Now we have a slider on the Shuffleboard which will send the Lift to a position, an indicator showing the actual measured position of the Lift, and text boxes for updating the PID constants. You will want to first ensure the motor and sensor are in the correct phase such that positive is up or forward (or a sensible direction), then you can start increasing your PID values. Again, see the [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/index.html) for a tuning tutorial.
