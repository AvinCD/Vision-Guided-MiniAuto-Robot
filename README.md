MiniAuto Vision Dashboard
A showcase-ready vision-guided mobile robot using:
Hiwonder MiniAuto mecanum-wheeled robot
ESP32-S3 camera for onboard video streaming
Laptop Python/OpenCV for red-target detection and reactive decision-making
Hiwonder BLE module for wireless motor commands
Flask dashboard for live video, manual control, autonomy, and emergency stop
> The laptop is the vision and decision layer. The robot receives movement commands wirelessly through BLE.
---
Demo Features
Live ESP32-S3 camera feed
Manual robot control using dashboard buttons or keyboard
Wireless BLE motor control
Red object / marker tracking
Reactive behavior:
target left → rotate left
target right → rotate right
target centered → move forward
target close → stop
target lost → stop
Start / stop autonomy toggle
Emergency stop button
---
System Architecture
```text
ESP32-S3 Camera
      │ Wi-Fi MJPEG stream
      ▼
Laptop Python + OpenCV + Flask Dashboard
      │ Bluetooth Low Energy commands
      ▼
Hiwonder BLE Module
      │
      ▼
MiniAuto Arduino Controller → Motor Driver → Mecanum Wheels
```
---
Repository Structure
```text
miniauto-vision-dashboard/
├── arduino/
│   └── app_control_safe_start.ino
├── python/
│   ├── miniauto_vision_dashboard_ble.py
│   ├── test_hiwonder_ble.py
│   └── inspect_hiwonder_ble.py
├── docs/
│   └── esp32_camera_notes.md
├── requirements.txt
├── .gitignore
└── README.md
```
---
Hardware Requirements
Hiwonder MiniAuto / compatible Arduino motor platform
Hiwonder Bluetooth Low Energy module
ESP32-S3 camera board
Laptop with Bluetooth and Wi-Fi
Robot battery powered ON during motor testing
---
Software Requirements
Python 3.10 or later
Arduino IDE for the MiniAuto and ESP32-S3 camera sketches
Install dependencies:
```bash
pip install -r requirements.txt
```

Dashboard Setup
Open:
```text
python/miniauto_vision_dashboard_ble.py
```
At the top, update these values whenever needed:
```python
ESP32_IP = "192.168.1.220"
BLE_ADDRESS = "48:87:2D:82:8A:74"
BLE_CONTROL_HANDLE = 24
ROBOT_SPEED = 35
```
Run the dashboard:
```bash
python python/miniauto_vision_dashboard_ble.py
```
Open in a browser:
```text
http://127.0.0.1:5000
```
For phone dashboard access, keep the phone and laptop on the same Wi-Fi, find the laptop IPv4 address, and use:
```text
http://<LAPTOP_IP>:5000
```
Allow Python through the Windows Firewall on private networks if Windows asks.
---
Dashboard Controls
Control	Function
▲ / Arrow Up	Move forward
▼ / Arrow Down	Move backward
◀ / Arrow Left	Rotate left
▶ / Arrow Right	Rotate right
Space	Stop
A	Start autonomy
X	Stop autonomy
Emergency Stop	Immediately disables autonomy and sends stop commands
Manual movement automatically pauses autonomy.
---
Red Target Autonomy Tuning
In `python/miniauto_vision_dashboard_ble.py`:
```python
ROBOT_SPEED = 35
MIN_OBJECT_AREA = 1800
CENTER_DEADZONE = 65
STOP_AREA = 18000
SEND_INTERVAL = 0.08
```
Recommended speed values:
```text
25  slow and safe
35  recommended showcase speed
45  faster but may overshoot
```
If the robot oscillates left and right, increase `CENTER_DEADZONE` slightly.
If it stops too early, increase `STOP_AREA`.
If it ignores a small red card, reduce `MIN_OBJECT_AREA`.
---
Safety Checklist
Before every demonstration:
Place the robot on open floor space.
Make sure the robot battery is ON.
Ensure the phone app is disconnected.
Verify the ESP32 camera stream in the dashboard.
Verify BLE status shows `CONNECTED`.
Test the emergency stop button.
Start with manual control before enabling autonomy.
---
Project Description
This project demonstrates a practical vision-guided autonomous mobile robot. The ESP32-S3 camera provides an onboard visual stream, while a laptop-based Python/OpenCV pipeline detects a red marker and generates reactive navigation decisions. Commands are sent wirelessly to the MiniAuto through Bluetooth Low Energy. The Flask dashboard provides a usable operator interface for manual control, autonomy activation, live monitoring, and safety stop.
---
Known Limitations
The laptop performs vision processing; autonomy is not fully onboard.
The robot uses reactive target following, not SLAM, mapping, or global path planning.
ESP32 IP may change when Wi-Fi network changes.
BLE address or GATT handle can differ on another robot/module.
