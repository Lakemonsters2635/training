# Arm Position Tutorial

### Overview
Steps for making an arm that can go to a any given position using PID
* Start the Arm Constants
* Make a subsystem for the arm motors
* Create a MoveArmToPoseCommand
* Bind the MoveArmToPoseCommand to a button

### Setting up the Arm Constants

In Constants, create variables to store the motor's CAN ID and speeds for the intake motor.
```java
// ARM CONSTANTS
    public static final int LEFT_ARM_MOTOR_ID = 21;
    public static final int RIGHT_ARM_MOTOR_ID = 15;

    public static final int ARM_STOP_SPEED = 0;
```

### Make a subsystem for the arm motors
Create new ```TalonFX``` for the motor controllers using the Motor IDs from Constants. Also, create a PIDController to be used to move the arm later.
```java 
    //PID used for moving arm
    //Note that the P value may need a little bit changing through experimentation
    private PIDController pid = new PIDController(0.009, 0.0, 0.0); 

    public TalonFX m_armMotorLeft;
    public TalonFX m_armMotorRight;
    public ArmSubsystem(){
        m_armMotorLeft = new TalonFX(Constants.LEFT_ARM_MOTOR_ID);
        m_armMotorRight = new TalonFX(Constants.RIGHT_ARM_MOTOR_ID);

        //Brake Mode so the arm doesn't ever fall with full force (Safety)
        m_armMotor1.setNeutralMode(NeutralModeValue.Brake);
        m_armMotor2.setNeutralMode(NeutralModeValue.Brake);
    }
```
Positions of the arm are measured in encoder counts by an encoder, however it needs to be converted into a degree system for convience. To do this there are three steps.

1.  Create an encoder in the field
```java
public static final DutyCycleEncoder m_encoder = new DutyCycleEncoder(Constants.ARM_ENCODER_ID);
```
2.  Find the Encoder ID and make it a constant
```java
 public static final int ARM_ENCODER_ID = 0;
```
3.  Create a method to turn encoder counts into degrees
```java
 public double getTheta(){
    theta = 360.0 * (m_encoder.getAbsolutePosition() - Constants.ARM_ENCODER_OFFSET);
    theta %= 360.0;
    if (theta < 0){
      theta += 360.0;
    }
    if (theta > 180){
      theta -= 360;
    }
    return theta;
  }
```
The ```Constants.ARM_ENCODER_OFFSET``` is used to set a zero position where the arm is balanced. You can find this by finding the balance position then getting the encoder counts from smart dashbard.

Afterward, you can now use your PID loop. With just the P (Proportion) in the pid loop you can correct error but there usually is an overshoot and oscillation. You are still able to get close to the setpoint though.

To find the feedback motor power
```java
//In Peroidic
//A common mistake is not updating theta in periodic
fbMotorPower = MathUtil.clamp(pid.calculate(theta, m_poseTarget), lowerLimitFB, upperLimitFB) * 10;

motorPower = fbMotorpower;
```
The clamp is so the pid does not give the motors too much power. In the constructor set the starting ```m_poseTarget``` to a starting position/angle you want so the arm does not go crazy when enabled. Also set positions for where you can give zero motor power to make sure motors will not overheat. 

The last thing you need to do in the subsystem is create some methods. Such as a setter for poseTarget and an ```areWeThereYet()``` method to see if the arm has reached the position or is close enough to call it has reached the position.

### Create a ```MoveArmToPoseCommand```

After instantiating subsystems in the command add an angle variable that gets passed in the paremeters of the constructor to be used in ```initialzie()``` with the setter method for ```poseTarget``` that you created in your subsystem. 

Lastly, in ```isFinished()``` call the ```areWeThereYet()``` method that was created to end the command when the arm reaches the current position

### Bind ```MoveArmToPoseCommand``` to a button
In RobotContainer, create a button for the IntakeCommand.
```java
    Trigger armButton = new JoystickButton(rightJoystick, Constants.ARM_BUTTON);
```
Set the button to run the IntakeCommand when pressed.
```java
    armButton.onTrue(m_moveArmToPoseCommand);
```

### Common Mistakes
* Forgetting Imports
* Forgetting to import vendor libraries (phoenix)
* Forgetting ```addRequirements()``` in the command constructor
* Forgetting to instantiate subsystems or commands
* Forgetting to update variables in perioidic
* Forgetting Constants
* Having wrong scope of variables
  

### Criteria
Ask someone who is not a rookie to come look at your code before you run it and ask them to watch while you run your code. If they give you the green flag then you are done


### Where To Go From Here   
This code only consists of proportion control out of proportion, integral, and derivative (PID). Tuning the others in would help with oscilation and effecient movement.

The code is only feedback motor power. Learning the process of finding the feedforward motor power is also very beneficial, however it is already given to you in this tutorial.