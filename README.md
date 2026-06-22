# Vision-Guided-MiniAuto-Robot
The robot uses its onboard camera to detect a coloured target. It autonomously turns toward the target, follows it, and stops when the target is close. The ESP32S3-CAM provides vision, Python/OpenCV makes the decision, and the MiniAuto controller drives the robot

Showcase concept
ESP32S3-CAM onboard camera
        ↓
Laptop OpenCV perception
        ↓
MiniAuto Arduino motor control
        ↓
Robot searches, tracks, follows, and stops
