# FRC 2019

Team 1806's modified version of Team 254's 2019 FRC robot code prepared for use for the 2020 season. This codebase is written in Java and is based off of WPILib's Java control system. Huge thanks to team 254 for making their code public for reuse purposes as per FRC rules, as team 1806 has been using 254 code bases since 2018.

The code is divided into several packages, each responsible for a different aspect of the robot function. This README explains setup instructions, the function of each package, and some of the variable naming conventions used. Additional information about each specific class can be found in that class' Java file.

## Setup Instructions

### General
1. Clone this repo
1. Run `./gradlew` to download gradle and needed FRC/Vendor libraries
1. Run `./gradlew tasks` to see available options
1. Enjoy!

### Visual Studio Code (Official IDE)
1. Get the WPILib extension for easiest use from the VSCode Marketplace - Requires Java 11 or greater
1. In [`.vscode/settings.json`](.vscode/settings.json), set the User Setting, `java.home`, to the correct directory pointing to your JDK 11 directory

### IntelliJ
1. Run `./gradlew idea`
1. Open the `FRC-2019-Public.ipr` file with IntelliJ

### Eclipse
1. Run `./gradlew eclipse`
1. Open Eclipse and go to File > Open Projects from File System...
1. Set the import source to the `FRC-2019-Public` folder then click finish

### Basic Gradle Commands
* Run `./gradlew deploy` to deploy to the robot in Terminal (*nix) or Powershell (Windows)
* Run `./gradlew build` to build the code.  Use the `--info` flag for more details
* Run `./gradlew test` to run all of the JUnit tests

## Code Highlights
* Path following with an adaptive pure pursuit controller and motion profiling

    To control autonomous driving, the robot utilizes an [adaptive pure pursuit controller](src/main/java/com/team1806/lib/control/AdaptivePurePursuitController.java) to control steering and a custom trapezoidal motion profile [generator](src/main/java/com/team1806/lib/motion/MotionProfileGenerator.java) and [follower](src/main/java/com/team1806/lib/motion/ProfileFollower.java) to control velocity.

* Path generation for vision targets.
	Can generate a path on the fly to drive to a vision target automatically.

## Package Functions
- [`com.team1806.frc2019`](src/main/java/com/team1806/frc2019)

    Contains the robot's central functions and holds a class with all numerical constants used throughout the code (see [`Constants.java`](src/main/java/com/team1806/frc2019/Constants.java)). For example, the [`Robot`](src/main/java/com/team1806/frc2019/Robot.java) class controls all routines depending on the robot mode. In addition, the [`RobotState`](src/main/java/com/team1806/frc2019/RobotState.java) class keeps track of the current position of the robot's various frames of reference.

- [`com.team1806.frc2019.auto`](src/main/java/com/team1806/frc2019/auto)

    Handles the execution of autonomous routines and contains the [`actions`](src/main/java/com/team1806/frc2019/auto/actions) and [`modes`](src/main/java/com/team1806/frc2019/auto/modes) packages.

- [`com.team1806.frc2019.auto.actions`](src/main/java/com/team1806/frc2019/auto/actions)

    Contains all actions used during the autonomous period, which all share a common interface, [`Action`](src/main/java/com/team1806/frc2019/auto/actions/Action.java) (also in this package). Examples include driving paths, auto aiming the turret and scoring, and auto steering. Actions interact with the subsystems, which in turn interact with the hardware.

- [`com.team1806.frc2019.auto.modes`](src/main/java/com/team1806/frc2019/auto/modes)

    Contains all autonomous modes. Autonomous modes consist of a list of autonomous actions executed in a specific order.

- [`com.team1806.frc2019.controlboard`](src/main/java/com/team1806/frc2019/controlboard)

    Contains code for the driver to use either joysticks or gamepad and the operator to use a gamepad. Also contains a wrapper class specifically for Xbox controllers (see [XboxController.java](src/main/java/com/team1806/frc2019/controlboard/XboxController.java)).

- [`com.team1806.frc2019.loops`](src/main/java/com/team1806/frc2019/loops)

    Contains codes for loops, which are routines that run periodically on the robot, such as for calculating robot pose, processing vision feedback, or updating subsystems. All loops implement the [`Loop`](src/main/java/com/team1806/frc2019/loops/Loop.java) interface and are handled (started, stopped, added) by the [`Looper`](src/main/java/com/team1806/frc2019/loops/Looper.java) class, which runs at 100 Hz. The [`Robot`](src/main/java/com/team1806/frc2019/Robot.java) class has one main looper, `mEnabledLooper`, that runs all loops when the robot is enabled.

- [`com.team1806.frc2019.paths`](src/main/java/com/team1806/frc2019/paths)

    Contains all paths that the robot drives during autonomous mode. Each path is made up of a list of `Waypoint` objects. The [`PathBuilder`](src/main/java/com/team1806/frc2019/paths/PathBuilder.java) class, which is also included in this package, transforms these waypoints into a series of `Arc` and `Line` objects.

- [`com.team1806.frc2019.planners`](src/main/java/com/team1806/frc2019/planners)

    Contains various planners for superstructure motion to avoid superstructure collisions with itself or the drivebase.

- [`com.team1806.frc2019.statemachines`](src/main/java/com/team1806/frc2019/statemachines)

    Contains the state machines for the end effector, overall superstructure, and two climbing mechanisms.

- [`com.team1806.frc2019.states`](src/main/java/com/team1806/frc2019/states)

    Contains states and other classes used in various subsystem and state machine classes.

- [`com.team1806.frc2019.subsystems`](src/main/java/com/team1806/frc2019/subsystems)

    Contains code for subsystems, which are consolidated into one central class per subsystem, all of which extend the [`Subsystem`](src/main/java/com/team1806/frc2019/subsystems/Subsystem.java) abstract class. Each subsystem uses state machines for control and is a singleton, meaning that there is only one instance of each. Subsystems also contain an enabled loop, a read periodic inputs method, and a write periodic outputs method, which are controlled by the [`SubystemManager`](src/main/java/com/team1806/frc2019/SubsystemManager.java) class.

- [`com.team1806.lib.control`](src/main/java/com/team1806/lib/control)

    Contains all the classes used for the robot's path following. The robot's steering is controlled by the [`AdaptivePurePursuitController`](src/main/java/com/team1806/lib/control/AdaptivePurePursuitController.java) class.

- [`com.team1806.lib.drivers`](src/main/java/com/team1806/lib/drivers)

    Contains a set of custom classes for motor controllers (TalonSRX's and Spark MAX's) that include classes for creating motor controllers with factory default settings, handling errors, reducing CAN Bus usage, and checking motors.

- [`com.team1806.lib.geometry`](src/main/java/com/team1806/lib/geometry)

    Contains a set of classes that represent various geometric entities.

- [`com.team1806.lib.motion`](src/main/java/com/team1806/lib/motion)

    Contains all motion profiling code used for autonomous driving. Trapezoidal motion profiles are used for smooth acceleration and minimal slip.

- [`com.team1806.lib.physics`](src/main/java/com/team1806/lib/physics)

    Contains the [`DriveCharacterization`](src/main/java/com/team1806/lib/physics/DriveCharacterization.java) class which represents the characterization for specifically a differential drivetrain. While this class did not end up being used during this season, it differs from 2018, so is being released, nonetheless.

- [`com.team1806.lib.util`](src/main/java/com/team1806/lib/util)

    Contains a collection of assorted utilities classes used in the robot code. Check each file for more information.

- [`com.team1806.lib.vision`](src/main/java/com/team1806/lib/vision)

    Contains various classes that help with tracking and storing information about vision targets.

- [`com.team1806.lib.wpilib`](src/main/java/com/team1806/lib/wpilib)

    Contains parent classes of the main [`Robot`](src/main/java/com/team1806/frc2019/Robot.java) class that get rid of loop overrun and watchdog print messages that clutter the console.
	
## Variable Naming Conventions
- k*** (i.e. `kDriveWheelTrackWidthInches`): Final constants, especially those found in the [`Constants.java`](src/main/java/com/team1806/frc2019/Constants.java) file
- m*** (i.e. `mPathFollower`): Private instance variables
- Note: Code from team 1806 probably breaks these practices. Sorry.