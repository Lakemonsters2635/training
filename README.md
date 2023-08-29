# Training
Software Training structure for Lakemonsters 2635

Basics
---

```mermaid
    flowchart BT
    subgraph BasicsPt.1
        direction LR
        subgraph FacilitationBasics
            direction BT
            id0[Development Environment Setup]-->id1
            id1[Git Basics]-->Downloads;
            Downloads-->id2[Robot Constants];
        end
        subgraph CodeBasics
            direction BT
            Subsystem-->id3[Debugging Basics];
            id3-->id4[Command Structure];
            id4-->Structure;
            id4-->id5[Button Bindings];
            id4-->Encoders;
            id4-->id6[Limit Switches];
            Structure-->B[<a href='https://github.com/Lakemonsters2635/pneumatics_base'>Pneumatics</a>];
            id5-->B;
            Structure-->A[<a href='https://github.com/Lakemonsters2635/ElevatorBase'>Limit Switch Elevator</a>];
            id5-->A;
            id6-->A;
            Structure-->id8[Tank Drive];
            Encoders-->id9[Brush Motor];
            id5-->id9;
            Structure-->id9;
        end
    end
    subgraph BasicsCapstone
            direction BT
            id10[Network Tables]~~~Shuffleboard;
            Parallel/Sequential~~~Shuffleboard;
            Debugging-->Shuffleboard;
            Ternaries~~~Shuffleboard;
            Lambdas~~~Shuffleboard;
        end
    FacilitationBasics-->CodeBasics;
    BasicsPt.1-->BasicsCapstone;
    
```
**note: click on nodes for links to base code 
# Robot
1. Feedback/feedforward
2. PID
3. Performing System ID on elevator
4. Elevators
5. [Arm](https://github.com/Lakemonsters2635/arm_motor_base)
6. Tank
7. Swerve

# Auto
1. Command scheduler/auto chooser
2. Auto creation
3. Path planner
    - Path planner basics
    - Path planner limitations
1. Vision integration

# Vision
- Limelight
    - Network tables
- Machine learning vision basic
    1. Roboflow
    1. Python
    1. Training the model
    4. Oak camera
    5. Deploying models on oak camera
- Machine learning vision advanced
    1. Network tables (ntcore)
    1. Camera server (cscore)
    6. Data handling (numpy)
    1. Monstervision
    1. AprilTags
    7. Raspberry Pi
        - Development environment
        - WPILibPi
        - Basic Linux commands
        - Deploy Monster vision




# Elementary Java
TBD

# Helpful Links
- Vendor Libraries:
    - [CTRE Library Download](https://store.ctr-electronics.com/software/)
        - The link is at the bottom of the page, get the v5 link.
    - [REV Library Download](https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information#java-api)
        - Located around the middle of the page, use the online installation method
    - [Kauai Labs Library Download](https://pdocs.kauailabs.com/navx-mxp/software/roborio-libraries/java/)
        - Located around the middle of the page, use the online installation method
- [WPILib Java API Docs](https://github.wpilib.org/allwpilib/docs/release/java/index.html)