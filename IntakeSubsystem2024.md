## Intake Subsystem Tutorial

### Overview
Steps for adding a simple intake system to the robot and connecting it to the joysticks:
* Define the motor CAN IDs 
* Make a subsystem for the motors of the intake
* Set up constants for motor CAN IDs 
* Create an IntakeCommand for picking up a ring
* Bind the IntakeCommand to a button
* Create an IntakeOutCommand for reversing the intake and releasing the ring
* Bind the IntakeOutCommand to a button

### Define Motor CAN IDs
Refer to the Motor Setup and Troubleshooting document to retrieve the CAN ID of the motor for the intake subsystem.
Note that the procedure for setting up different motors varies.

### Make a subsystem for the motors of the intake
Create a new ```CANSparkMax``` for the motor controller using the Motor ID from Constants. Add functions for setting the voltage of the motor.
```java
public CANSparkMax intakeMotor;
  public IntakeSubsystem() {
    intakeMotor = new CANSparkMax(Constants.INTAKE_MOTOR_ID, MotorType.kBrushless);
    intakeMotor.setSmartCurrentLimit(20,1);
    intakeMotor.setInverted(true);    
  }
```
Example of a function:
```java
public void inIntake() {
    intakeMotor.setVoltage(Constants.INTAKE_IN_SPEED * -10);
  }
```

### Set up constants for motor CAN IDs
In Constants, create variables to store the motor's CAN ID and speeds for the intake motor.
```java
// INTAKE CONSTANTS
    public static final int INTAKE_MOTOR_ID = 13;

    public static final int INTAKE_STOP_SPEED = 0;
    public static final double INTAKE_IN_SPEED = -0.9;
    public static final double INTAKE_OUT_SPEED = 0.2;
```

### Create an IntakeCommand for picking up a ring
Create an IntakeCommand and import IntakeSubsystem. In the ```initialize()``` and ```execute()``` functions, call functions from IntakeSubsystem to start and stop the motors.

### Bind the IntakeCommand to a button
In RobotContainer, create a button for the IntakeCommand.
```java
    Trigger intakeButton = new JoystickButton(rightJoystick, Constants.INTAKE_BUTTON);
```
Set the button to run the IntakeCommand when pressed.
```java
    intakeButton.whileTrue(m_intakeCommand);
```

### Create an IntakeOutCommand for reversing the intake and releasing the ring
This command is for outtaking on the intake side. Similar to IntakeCommand, but also create a timer.

### Bind the IntakeOutCommand to a button
In RobotContainer, create a button for the IntakeOutCommand.
```java
        Trigger intakeOutButton = new JoystickButton(rightJoystick, Constants.INTAKE_OUT_BUTTON);
```
Set the button to run the IntakeCommand when pressed.
```java
intakeOutButton.onTrue(m_intakeOutCommand);
```