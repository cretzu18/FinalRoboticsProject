# Instant Camera

## General description

The Instant Camera project is a modern, DIY take on the classic [instant camera](https://en.wikipedia.org/wiki/Instant_camera). It a small device that lets you take photos, see them on a screen, save them digitally, and print them out instantly onto paper just like an old Polaroid. The project is built around the ESP32-CAM microcontroller.

## BOM (Bill Of Materials)

- ESP32-CAM
- OV2640 Camera Sensor
- TFT LCD Display
- TTL Serial Thermal Printer + Thermal Paper Rolls
- MicroSD Card
- 5V 3A DC Power Adapter
- Breadboard
- Capacitors
- Resistors
- Wires
- Buttons
- Micro Limit Switch
  

## Tutorial source

I will follow this [tutorial](https://www.youtube.com/watch?v=8lnDPz4QZjQ) to learn the basics of how to talk to the thermal printer. But my project will add more features:
  - A Selfie Mode: the code detects  the physical flip and automatically mirrors and rotates the image sensor data
  - Live Viewfinder: on the TFT Screen
  - Digital Gallery: the user can browse photos and choose which ones to reprint directly from the device
    

## Questions

### Q1 - What is the system boundary?

The system boundary is the line between the inside and the outside of the system.

The internal system consists of all components residing within the physical chassis that are directly managed by the ESP32 microcontroller: the ESP32-CAM module, the camera sensor and the mechanical flip switch, the TFT LCD viewfinder and the TTL Serial Thermal Printer and the MicroSD Card used for digital file archiving.

The external system includes independent entities that interact with the camera but are not controlled by its internal logic: the user, external smartphones or PCs that access the camera's data via the Web App and whatever its being captured by the camera lens. 

### Q2 - Where does intelligence live?

The intelligence lives in the ESP32 microcontroller because it acts as a decision-maker. It is capable of:
  - Processing Input: it reads the Selfie Mode and commands the camera to flip the image
  - Edge Computing: it converts complex color photos into black and white dot patterns for the printer
  - State Machine: it manages the state machine, ensuring the camera, screen and SD card don't crash while working togheter

### Q3 - What is the hardest technical problem?

The hardest technical problem is the fact that ESP32-CAM has a few pins and the camera and SD card already occupy almost all available connections. I must force the TFT Screen and SD Card to share the same communication lines without interfering with each other. Also the code must be perfectly non-blocking so the screen doesn't freeze while the camera is saving data or the printer is heating up.

### Q4 - What is the minimum demo?

The miminum demo for this project will have this four actions complete functionally:
  1. Mechanical Detection: flip the camera 180 degrees for selfie mode
  2. Capture and Storage: press a button to store a photo on the SD Card
  3. Physical Output: the thermal printer successfully prints a photo out
  4. Remote Access: open a phone browser to the web app and see the image gallery

## Q5 - Why is this not just a tutorial?

Most tutorials cover only one compoment while I have to design a shared bus architecture to prevent the screen, SD card and camera from colliding on the same pins. It has a custom logic like the mechanical flip of the camera or the conversion of the high resolution JPEFs into dithered printer patterns.

