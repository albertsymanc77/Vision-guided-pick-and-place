# Vision-Guided Pick-and-Place System with Dobot Magician

## Project Overview

Developed a vision-guided robotic pick-and-place system that uses a USB camera and OpenCV to detect coloured blocks, estimate their positions and orientations, and identify suitable pallet slots. The system integrates a Dobot Magician for robotic movement and a stepper motor for precise block rotation before placement.

Since the Dobot's end effector could not provide the required rotation, an external stepper motor was integrated to align each block with its target pallet slot.

## How It Works

The system captures images using a 1920 × 1080 USB camera and processes them using Python and OpenCV. HSV colour segmentation, morphological operations, and contour detection are used to identify the blocks and pallet. The `minAreaRect()` function estimates each object's centre and orientation.

To resolve the 180° ambiguity in orientation estimation, red markers are attached to the blocks. Their positions relative to the block centres are used with vector calculations and `atan2()` to determine the correct orientation.

An ArUco marker (ID 1) serves as the reference for converting image coordinates into robot coordinates, using a calibrated scale of 0.511635 mm per pixel and the required axis transformation.

Pallet slot positions are calculated from the pallet's centre, orientation, and predefined slot spacing. Image analysis checks slot occupancy by measuring the yellow region around each slot centre, with a threshold of 40% used to classify a slot as occupied.

The required block rotation is calculated from the difference between the slot angle and block angle. An NI-DAQmx-controlled stepper motor rotates the block using directional half-step sequences, with a nominal resolution of 0.9° per step.

## Technologies and Components

- Python and OpenCV
- USB camera (1920 × 1080)
- Dobot Magician robotic arm
- PyDobot for robot communication
- Stepper motor for block orientation
- NI-DAQ hardware and NI-DAQmx
- ArUco-based coordinate referencing
- HSV colour segmentation and contour analysis

## Key Challenges

- **Robot coordinate accuracy:** The Dobot exhibited position errors that varied across its workspace. A single global offset correction was insufficient to achieve accurate pick-and-place operations.
- **Lighting variations:** Changes in illumination affected colour segmentation and required HSV threshold tuning.
- **Orientation ambiguity:** Red markers were introduced to resolve the 180° ambiguity in rectangle-based angle estimation.
- **Coordinate calibration:** Camera-to-robot coordinate conversion and axis mapping required careful calibration.
- **Rotation synchronisation:** Consistent angle conventions were needed across the vision system, Dobot, and stepper motor.
- **Stepper control:** The calculated angular difference had to be converted into the appropriate number and direction of steps.

## Project Status

The vision system successfully detects blocks and the pallet, estimates object positions and orientations, calculates pallet slot coordinates, identifies potentially empty slots, and determines the required block rotation. Stepper motor control was also implemented.

However, complete end-to-end pick-and-place operation was not achieved because of the Dobot's varying positional offset, which could not be corrected using a single global adjustment.

## Future Improvements

- Develop workspace-dependent positional compensation for the Dobot.
- Improve camera-to-robot calibration using multiple reference points.
- Refine lighting robustness and object detection.
- Integrate and validate the complete vision, rotation, and pick-and-place workflow.
