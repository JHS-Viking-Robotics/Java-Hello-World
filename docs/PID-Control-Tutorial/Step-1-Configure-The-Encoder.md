---
title: "Step 1: Configure The Encoder"
parent: "Tutorial: PID Control"
nav_order: 1
---
# Configure The Encoder

We will start by creating a default subsystem called ```Lift```, and adding our hardware to it:

```java
  // Talon controller for Lift motor
  private final WPI_TalonSRX liftController;
```

Next in the constructor, we need to configure the Talon on the CAN bus and tell it to use the onboard PID controller and Mag Encoder

```java
    // Make sure to clear any existing settings after initializing
    liftController = new WPI_TalonSRX(23);
    liftController.configFactoryDefault();

    // Tell the Talon to use the onboard Mag Encoder
    liftController.configSelectedFeedbackSensor(FeedbackDevice.QuadEncoder);
    liftController.setSelectedSensorPosition(0.0);

    // Set the sensor and Talon inversion separately
    liftController.setInverted(false);
    liftController.setSensorPhase(false);
```

Finally we need to configure the PID controller. You can read more on how these work in the [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/index.html), as the process is the same. Make sure to replace ```<kPIDF>``` with your own PID values, or start at ```0.0``` if you don't know yet.

```java
    liftController.config_kP(0, <kP>);
    liftController.config_kI(0, <kI>);
    liftController.config_kD(0, <kD>);
    liftController.config_kF(0, <kF>);
```

With the controller configured, we can make a simple method in Lift for sending the Lift to a position.

```java
  /** Set the Lift to a position in TalonSRX sensor ticks */
  public void set(double position) {
    liftController.set(ControlMode.Position, position);
  }
```
