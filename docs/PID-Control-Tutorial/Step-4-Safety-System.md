---
title: "Step 4: Safety System"
parent: "Tutorial: PID Control"
nav_order: 4
---
# Safety System

A well constructed and tuned PID controlled system should work very well, and look awesome in the process! However, if the sensor is dislodged or the system is damaged somehow, we need a way to tell the system to stop activating so that it doesn't break itself, or worse hurt somebody.

To accomplish this, we will give the subsystem a safety score member, and every control loop we will run a series of checks on the motor controller to make sure we are operating correctly. Then we will have our ```set()``` methods check that the subsystem is enabled before moving, so that we don't have to do anything special in our commands to enable the safety configuration.

We start out by adding some members to our Lift.

```java
  private int safetyScore;          // Current safety score of subsystem [0, 100]
  private boolean subsystemEnabled; // Is the subsystem currently enabled

  public Lift() {
      ...
    // Start the safety score at maximum, and enable the subsystem
    safetyScore = 100;
    subsystemEnabled = true;
...
```

We will also define a helper method for updating the safety score. There are much better ways to do this using WPILib MathUtils, but for now we will keep it simple.

```java
  /**
   * Change the safety score for the subsystem by some amount
   * 
   * @param change amount +/- to change safety score by
   */
  private void changeSafetyScore(int change) {
    safetyScore += change;
    if (safetyScore < 0) {safetyScore = 0;}
    if (safetyScore > 100) {safetyScore = 1000;}
  }
```

Now we will add to our periodic() function.

```java
  @Override
  public void periodic() {
    ...
    // Ensure the safety score is able to return to 100 by slowly increasing it
    changeSafetyScore(1);

    // Calculate the maximum range of motion for the Lift, plus some wiggle room
    double maxRangeMotion = 200 + Math.abs(
        Lift.UP.getValue() - Lift.DOWN.getValue());

    // Run checks on the motors to ensure they are running safely. Note that
    // the score should be decreased by 1 extra unit to compensate for this
    // method trying to return the score to maximum
    if (liftController.getStatorCurrent() > 5.0) {
      changeSafetyScore(-6);
    } if (getLiftPositionError() > maxRangeMotion) {
      changeSafetyScore(-101);
    } if (getLiftPositionTicks() > Lift.UP.getValue() + 50) {
      changeSafetyScore(-21);
    } if (getLiftPositionTicks() < Lift.DOWN.getValue() - 50) {
      changeSafetyScore(-21);
    }

    // Disable the subsystem if we are not operating safely, otherwise check
    // the dashboard button to see if we are disabled there
    setSubsystemEnabled(safetyScore > 25);
  }
```

We need to also define ```setSubsystemEnabled(bool)``` for when we decide to disable, and possibly re-enable, the Lift. Notice that we are calling the CommandScheduler from inside of the subsystem. Normally this is bad practice, but since we are only using it for cancelling commands for the current subsystem we are okay.

```java
  /**
   * Disable or enable the Lift subsystem, and set the motors to neutral
   */
  public void setSubsystemEnabled(boolean enabled) {
    // We are already in desired state, so skip everything
    if (subsystemEnabled == enabled) {
      return;
    }
    System.out.println("Setting Lift subsystem status to " + enabled
                       + ", safety score at " + safetyScore);
  
    // Update subsystemEnabled and get a reference to the CommandScheduler
    CommandScheduler cmd = CommandScheduler.getInstance();
    subsystemEnabled = enabled;
    if (!enabled) {
      // Disable the subsystem by cancelling all commands and reverting back
      // to neutral
      cmd.cancel(cmd.requiring(this));
      // Note that if Command.end() gets called, it could update the motors
      // again, so we set it to neutral just in case
      liftController.set(ControlMode.PercentOutput, 0.0);
    }
  }
```

Finally, we should update our ```set(double)``` method so that it will not do anything when we are in disabled mode. This way we don't have to check if the subsystem is enabled during our commands, as the subsystem itself will take care of it.

```java
  public void set(double setpoint) {
    if (!subsystemEnabled) {
      System.out.println("WARNING: Lift subsystem is disabled");
      return;
    }
    liftController.set(ControlMode.Position, position);
  }
```

Now it is important to test the safety mode so that we know that it works effectively. Please be very careful not to hurt anybody or break anything when testing the robot, and also remember not to stall a motor or else it can catch fire.