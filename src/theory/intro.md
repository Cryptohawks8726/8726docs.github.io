(Needs proofreading! Written by: Keshav)

# Introduction to Control Theory

## Necessary Vocab

**System**: The thing you are trying to control. Examples: Climber, Shooter, Turret, Elevator, Arm, etc.

**States**: All systems have states. A state is simply the property of the system that you are trying to control. Examples: Position of a Turret, Velocity of a Flywheel, Height of an Elevator, etc.

**Inputs**: Inputs are features of a system that have an ability to change it's state. For FRC Control, voltage is the input that we are controlling and it can change the state of the system.

**Measurement**: Also known as outputs, these are the states of a system as understood by sensors. The actual state may be hard to get a reading of so measurements are usually estimates based on data and are usually good enough for FRC. We use a variety of sensors to get information but primarily [motor encoders](../parts/encoders.md).

**Setpoint**: Also known as references, this is the desired state of a system. Examples: Desired RPM of a Flywheel, Desired Height of an Elevator. 

**Error**: Error is the difference between the setpoint and the measurement in the system. For example, if a flywheel is at 2000RPM and its setpoint is 8726RPM then the error is \\( 8726 RPM - 2000 RPM = 6726 RPM \\).

**Control Algorithm**: A control algorithm will, using the setpoint and measurement, produce an input to feed into the system. The input will affect the state of the system. A good control algorithm minimizes the error to as low as possible in as little time as possible but the exact trade off between preventing overshooting and reaching the system in a low amount of time varies from system to system.

## States we Typically Control in FRC

### Position Control

This is a very common state that we control in FRC applications. Climbers, elevators, turrets, and arms are all clasified as position control. This is because the setpoint and measurements that we are dealing with are both positions. We have one position and want to go to another position so we are controlling the position and need position control. 

*Note on Position Control with Angles -> When a system is moving in circles, such as a turret, it sometimes be more effiecient to go in the opposite direction. (since it's a circle). This also varies with the mechanical constraints of the system but this is just something to note. 

### Velocity Control

This is also very common to control in FRC. Flywheels, rollers, and drivetrains all rely on velocity control. This is because the setpoint and measurements that we are dealing with are both velocities. Our goal is to reach a certain velocity, which can be seen most obviously in flywheels.
