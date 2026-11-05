# 6-axis-robotic-arm-
3D printed 6-axis robotic arm with arduino inverse kinematics. 
# 6-Axis Robot Arm

A 3D-printed, stepper-driven 6-axis robot arm controlled by an Arduino, with inverse kinematics so you can command the tool tip by position instead of setting each joint by hand.

> Status: in design (SolidWorks). Code is written but not yet tested on hardware.

## Specs

| Item | Value |
| --- | --- |
| Axes | 6 (base, shoulder, elbow, forearm roll, wrist pitch, tool roll) |
| Reach | ~365 mm |
| Link lengths | 110 / 160 / 150 / 55 mm |
| Drives | Spur gear (J1, 5:1), GT2 timing belts (J2–J5), direct (J6) |
| Motors | NEMA 17 (J1–J4), NEMA 11 (J5–J6) |
| Controller | Arduino Mega 2560 + 6 × TMC2209 drivers |
| Material | PETG / PLA+ |

## Joints

| Joint | Motion | Ratio | Range |
| --- | --- | --- | --- |
| J1 | Base yaw | 5:1 gear | ±170° |
| J2 | Shoulder | 5:1 belt | ±90° |
| J3 | Elbow | 4:1 belt | ±135° |
| J4 | Forearm roll | 3:1 belt | ±180° |
| J5 | Wrist pitch | 3:1 belt | ±110° |
| J6 | Tool roll | 1:1 | ±180° |

## Code

`robot_arm_ik.ino` uses the [AccelStepper](https://www.airspayce.com/mikem/arduino/AccelStepper/) library. Serial commands at 115200 baud:

| Command | Example | Does |
| --- | --- | --- |
| `H` | `H` | Home all joints on their limit switches |
| `G x y z roll` | `G 220 0 20 0` | Move the tool tip to (x, y, z) mm, pointing down |
| `J a1 … a6` | `J 0 30 60 0 90 0` | Move joints directly, in degrees |

### How the inverse kinematics works

1. Move up from the tool tip by the tool length to get the **wrist centre**.
2. **J1** = `atan2(y, x)` points the arm at the target.
3. **J2 and J3** solve a 2-link triangle (law of cosines) to reach the wrist centre, using the elbow-up solution.
4. **J5** is set so the tool points straight down; **J6** sets the tool's roll.

## Roadmap

- [ ] Finish SolidWorks model and motion study
- [ ] Print and test J1–J3
- [ ] Wire electronics and homing switches
- [ ] Test IK on hardware
- [ ] Add a gripper and a pen-drawing demo

## Author

Kopano Matlhadisa — Mechanical Engineering, University of Waterloo

