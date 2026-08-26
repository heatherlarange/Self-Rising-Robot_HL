# robo03

This is an Arduino sketch that controls a 2-DOF (degrees of freedom) robot using an M5Atom and executes a "stand-up" motion based on a pre-trained policy.

## Files

- `robo03/robo03.ino` - Control sketch for M5Atom
- `robo03/policy_network.h` - Pre-trained policy converted into a C header file

Since `policy_network.h` is included by `robo03.ino`, ensure it is placed in the same `robo03` folder.

## Required Libraries

Install the following libraries using the Arduino IDE Library Manager:

- M5Atom
- Kalman
- ESP32Servo

You also need the ESP32 board environment installed.

## Upload

Open `robo03/robo03.ino` in the Arduino IDE, build it for the M5Atom, and upload the sketch.

## Wi-Fi Control

Upon startup, the M5Atom creates a Wi-Fi access point.

- SSID: `robo1`
- Password: `password`
- URL: `http://192.168.42.1`

Accessing this URL via a web browser allows you to manually adjust the servos, start/stop the stand-up motion, and toggle automatic stand-up mode.

## Button

You can also initiate the stand-up action using the button on the M5Atom unit itself.

Pressing the button while the stand-up action is in progress stops the movement and returns the device to manual mode.

## Notes

- Servo 1 is intended to be connected to GPIO 26, and Servo 2 to GPIO 32.
- Servo offsets and pulse widths can be adjusted via the web interface and are saved to Preferences.
- `policy_network.h` contains a pre-generated policy network. Please replace this file if you wish to use a different set of training results.
