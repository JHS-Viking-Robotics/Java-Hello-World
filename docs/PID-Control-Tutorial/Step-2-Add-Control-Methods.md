---
title: "Step 2: Add Control Methods"
parent: "Tutorial: PID Control"
nav_order: 1
---
# Add Control Methods

We created a very basic ```set(double)``` method previously, but this requires us to remember a lot of numbers that correspond to positions on our Lift. Let's create a nice [enum]("https://www.w3schools.com/java/java_enums.asp") with the positions that we might want to use. Make sure to fill in ```<position_ticks>``` with your own positions!

```java
...
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

  public Lift() {
...
```

Now we can make an overload of our ```set(double)``` method that takes in a ```Mode```, so that we can call it simply like ```subsystem.set(Mode)```. You can also use a ```switch``` statement if you want to have different defined operating modes that do other things.

```java
    /**
   * Sets the Lift to the specified operating mode.
   * 
   * <p> Will do nothing and print a warning to the console if the subsystem
   * is disabled
   * 
   * @param mode desired Lift operating mode
   */
  public void set(Mode mode) {
    liftController.set(ControlMode.Position, mode.getValue());
  }
```
