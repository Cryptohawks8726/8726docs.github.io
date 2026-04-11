(Needs proofreading! Written by: Dhruva)

# MotorTester

MotorTester is a WPILib project with source code that can be found [here](https://github.com/Cryptohawks8726/MotorTesting).
It is used primarily for testing motor functionality and running prototypes.

In the future, it will likely support SystemCore, but it has currently only been tested
with roboRIOs. This code currently works with motors controlled by SparkMax motor
controllers or TalonFX controllers, but support for other motor types may be added.

This code can also play music in the `.chrp` format on
[Kraken X60 motors](../parts/kraken.md#playing-sounds).

## Why was this made?

This was primarily made for testing prototypes. The team often wants to make prototypes of subsystems
using basic materials, and to test them a couple of motors may need to be
run at the same time with specific speeds. While this can be done using
[REV Hardware Client](../software/rhc.md), only one motor can be run at
a time so multiple laptops must be used, which makes things get messy.
By deploying the MotorTester code to a roboRIO (or SystemCore in the future),
multiple motors can be configured and run at once. You can also easily
see the speed and position of the motor's built-in encoder.

## How to use MotorTester

MotorTester exposes some values in NetworkTables which can be used to
register connected motors and then control the connected motors.
This is most easily done using [Aluminum](../software/aluminum.md),
which has an interface to add or run motors. However, it can
also be used manually through a tool like [Glass](../software/wpilib.md#glass).

First, deploy the MotorTester code onto the roboRIO (or SystemCore)
and ensure all motors are connected to the CAN bus and are receiving power.
You will need to know the CAN IDs of each motor, so make sure to check
or set them if needed. Additionally, make sure you have the
[driver station](../software/ds.md) open, as you will need it to enable
and disable the robot.

Then, add each motor to the list of connected motors. In Aluminum,
this is done by entering the CAN ID of the motor, selecting the type of
motor from the dropdown menu, and then clicking "Add Motor". To do this
manually from Glass, go to SmartDashboard and set the "CAN ID" value to the CAN ID
of the motor, set the "Motor Type" value to one of the strings listed in "Motor Types",
and then set "publish" to true.

Once this is done, you should see the motor appear on the list in Aluminum
or see new entries appear under "Motors" in SmartDashboard. You can then enter
a voltage to run the motor at (-12 to 12) and once the robot is enabled, the motor
will begin running at that speed. Disabling the robot will cause all motors to stop
as you would expect.

Music can also now be played on any connected Kraken X60 motors by uploading files
directly through Aluminum and then clicking the play button.

## Project structure

This is a normal WPILib robot project and so it should be similar to typical season code,
although it is missing most things you would have on an actual robot.

`RobotContainer.java` contains the main logic for adding and running motors. The class
implements `Sendable`, and in its `initSendable` method the interface for adding
motors is defined.

Each supported motor type has a matching class which extends from the abstract class `MotorWrapper`,
which also implements `Sendable`.
When new motors are added, a new `MotorWrapper` object is created corresponding to the motor type
the user selected and placed in NetworkTables under Motors/{CAN ID}.

To add support for a new type of motor controller, create a new class extending from `MotorWrapper`
and implement the required abstract methods. Then, go to `RobotContainer.java` and add the motor type
to the enum `MotorModels` and add a new case in `createNewMotor()` which creates a new instance of
the new class and puts it on SmartDashboard, similar to the existing cases for other motor controllers.

Note that care must be taken here to avoid a ConcurrentModificationException by calling
`SmartDashboard.postListenerTask()` (see the [documentation of this method](<https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/smartdashboard/SmartDashboard.html#postListenerTask(java.lang.Runnable)>))

Playing music uses the [Orchestra class from CTRE's PhoenixLib](https://api.ctr-electronics.com/phoenix6/stable/java/com/ctre/phoenix6/Orchestra.html).
The raw data from a .chrp file is sent through NetworkTables by setting the "music" value. When this value is set,
MotorTester will attempt to save it to a temporary file which can then later be used for playback.
