# ESP32Cam Color Detection and HSV Thresholding
## Overview

This project demonstrates how to create a video streamer using the ESP32Cam board with Real-Time Streaming Protocol (RTSP) support. The video stream can be accessed through a web browser, and a Python script is provided for color detection using HSV thresholding.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Setting Up Hardware](#1-setting-up-hardware)
  - [2. Software Installation](#2-software-installation)
  - [3. WiFi Configuration](#3-wifi-configuration)
  - [4. Accessing the Video Stream](#4-accessing-the-video-stream)
  - [5. Color Detection Script](#5-color-detection-script)
- [Python Color Detection Script](#python-color-detection-script)
- [Video](#video)
- [Usage](#usage)
- [Clone and Implementation](#clone-and-implementation)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

- ESP32Cam board
- PlatformIO installed in Visual Studio Code (VSCode)
- Arduino IDE
- Python with OpenCV library

## Getting Started

### 1. Setting Up Hardware

Connect the ESP32Cam board to your computer and ensure the OV2640 camera module is properly connected.

### 2. Software Installation

Install PlatformIO in VSCode for ESP32 development. Open the project in VSCode and upload the code to the ESP32 board.

1. Clone this repository.
2. Upload the code to the ESP32-CAM using Arduino IDE or PlatformIO in VS Code.
3. Ensure Python is installed on your system.
4. Install required Python libraries using:
    ```bash
    pip install numpy 
    pip install opencv-python 
    ```
### 3. WiFi Configuration

Update the `wifikeys.h` file with your WiFi SSID and password.

### 4. Accessing the Video Stream

Connect to the ESP32Cam through an existing WiFi network or the created access point.

### 5. Color Detection Script

Run the provided Python script for color detection. Ensure OpenCV is installed (`pip install opencv-python`).

## Python Color Detection Script

```python

# Change the IP address below according to the IP shown in the Serial monitor of Arduino code
url = 'http://192.168.4.1/cam-lo.jpg'


# Create trackbars for adjusting HSV values
cv2.createTrackbar("LH", "Tracking", 0, 255, nothing)
cv2.createTrackbar("LS", "Tracking", 0, 255, nothing)
cv2.createTrackbar("LV", "Tracking", 0, 255, nothing)
cv2.createTrackbar("UH", "Tracking", 255, 255, nothing)
cv2.createTrackbar("US", "Tracking", 255, 255, nothing)
cv2.createTrackbar("UV", "Tracking", 255, 255, nothing)

while True:
    # Fetch the image from the ESP32Cam
    img_resp = urllib.request.urlopen(url)
    imgnp = np.array(bytearray(img_resp.read()), dtype=np.uint8)
    frame = cv2.imdecode(imgnp, -1)

    # Convert frame to HSV
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # Get trackbar positions
    l_h = cv2.getTrackbarPos("LH", "Tracking")
    l_s = cv2.getTrackbarPos("LS", "Tracking")
    l_v = cv2.getTrackbarPos("LV", "Tracking")
    u_h = cv2.getTrackbarPos("UH", "Tracking")
    u_s = cv2.getTrackbarPos("US", "Tracking")
    u_v = cv2.getTrackbarPos("UV", "Tracking")

    # Define lower and upper HSV thresholds
    l_b = np.array([l_h, l_s, l_v])
    u_b = np.array([u_h, u_s, u_v])

    # Create mask and apply it to the frame
    mask = cv2.inRange(hsv, l_b, u_b)
    res = cv2.bitwise_and(frame, frame, mask=mask)
```

## Video

[video](https://github.com/ESP32-Work/Color-Detection-using-HSV/assets/81290322/be567beb-b690-441c-a9c8-6e4be1a2a321)

## Usage
1. Power up the ESP32-CAM and connect to its access point (AP) with the provided SSID and password.
2. Run the Python script on your local machine.
3. The live stream with extracted text will be displayed on your screen.


## Clone and Implementation
```bash
git clone https://github.com/ESP32-Work/Color-Detection-using-HSV.git
```

Open the project in VS Code with PlatformIO extension installed. Upload the Arduino code to the ESP32-CAM and run the Python script.

Move to the directory containing the python script. Ensure that it is executable.
```bash
chmod +x color_detection.py
``` 
Run the script.
```bash
python3 color_detection.py  or ./color_detection.py
```
## Contributing
Contributions are welcome! Open an issue or create a pull request to contribute.

## License
This project is licensed under the [MIT](LICENSE) License.
