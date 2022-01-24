---
title: "Step 5: Bringing It Together"
parent: "Tutorial: PID Control"
nav_order: 5
---
# Bringing It Together

Here is a copy of the full code for convenience.

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot.subsystems;

import java.util.Map;

import com.ctre.phoenix.motorcontrol.ControlMode;
import com.ctre.phoenix.motorcontrol.FeedbackDevice;
import com.ctre.phoenix.motorcontrol.can.WPI_TalonSRX;

import edu.wpi.first.networktables.NetworkTableEntry;
import edu.wpi.first.wpilibj.shuffleboard.BuiltInLayouts;
import edu.wpi.first.wpilibj.shuffleboard.BuiltInWidgets;
import edu.wpi.first.wpilibj.shuffleboard.Shuffleboard;
import edu.wpi.first.wpilibj.shuffleboard.ShuffleboardLayout;
import edu.wpi.first.wpilibj.shuffleboard.ShuffleboardTab;
import edu.wpi.first.wpilibj2.command.CommandScheduler;
import edu.wpi.first.wpilibj2.command.SubsystemBase;

public class Lift extends SubsystemBase {

  // Talon controller for Lift motor
  private final WPI_TalonSRX liftController;
  private NetworkTableEntry liftP; // kP for Lift Talon closed-loop PID
  private NetworkTableEntry liftI; // kI for Lift Talon closed-loop PID
  private NetworkTableEntry liftD; // kD for Lift Talon closed-loop PID
  private NetworkTableEntry liftF; // kF for Lift Talon closed-loop PID
  private NetworkTableEntry liftSetpoint;   // Desired Lift setpoint in Talon sensor ticks
  private NetworkTableEntry liftPosition;   // NetTables dashboard indicator for Lift position in Talon sensor ticks

  private int safetyScore;          // Current safety score of subsystem [0, 100]
  private boolean subsystemEnabled; // Is the subsystem currently enabled

  public enum Mode {
    /** Lift up position */
    UP(<upTicks>),
    /** Lift dispense position */
    DISPENSE(<dispense_ticks>),
    /** Lift down position */
    DOWN(<down_ticks>);

    private double output;

    private Mode(double output) {
      this.output = output;
    }
    public double getValue() {return this.output;}
  }

  /** Creates a new Lift. */
  public Lift() {
    // Make sure to clear any existing settings after initializing
    liftController = new WPI_TalonSRX(23);
    liftController.configFactoryDefault();

    // Tell the Talon to use the onboard Mag Encoder
    liftController.configSelectedFeedbackSensor(FeedbackDevice.QuadEncoder);
    liftController.setSelectedSensorPosition(0.0);

    // Set the sensor and Talon inversion separately
    liftController.setInverted(false);
    liftController.setSensorPhase(false);

    liftController.config_kP(0, <kP>);
    liftController.config_kI(0, <kI>);
    liftController.config_kD(0, <kD>);
    liftController.config_kF(0, <kF>);

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

    // Add the PIDF constants to a Layout
    liftP = shuffleLiftPIDLayout
        .add("P", 0.0)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftI = shuffleLiftPIDLayout
        .add("I", 0.0)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftD = shuffleLiftPIDLayout
        .add("D", 0.0)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();
    liftF = shuffleLiftPIDLayout
        .add("F", 0.0)
        .withWidget(BuiltInWidgets.kTextView)
        .getEntry();

        // Start the safety score at maximum, and enable the subsystem
    safetyScore = 100;
    subsystemEnabled = true;
  }

  public void set(double position) {
    liftController.set(ControlMode.Position, position);
  }

  /**
   * Sets the Lift to the specified operating mode.
   * 
   * <p> Will do nothing and print a warning to the console if the subsystem
   * is disabled
   * 
   * @param mode desired Lift operating mode
   */
  public void setLift(Lift mode) {
    if (!isEnabled()) {
      System.out.println("WARNING: Lift subsystem is disabled");
      return;
    }
    switch (mode) {
      case TESTING:
        liftController.set(ControlMode.Position, getLiftSetpoint());
        break;
      default:
        if (mode.getMode() == ControlMode.PercentOutput) {
          liftSetpoint.setDouble(mode.getValue());
        }
        liftController.set(mode.getMode(), mode.getValue());
        break;
    }
  }

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
}
```