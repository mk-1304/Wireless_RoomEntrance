# Advanced Wireless Room Entrance System

This project implements a secure and automated room access system that integrates individual access cards with cloud-based room management. By leveraging microcontrollers, NFC technology, and IoT connectivity, the system enables seamless, keyless entry and improves administrative efficiency.

## Components

- **Arduino** – Manages door operations based on access card data.  
- **ESP32** – Handles cloud communication and stores access logs remotely.  
- **NFC Module** – Reads card information and communicates with the Arduino for identity verification.  
- **FSR Sensor** – Installed inside rooms to detect card placement and trigger smart lock mechanisms.  

## Features

- **Keyless Entry**  
  Access cards are used to unlock doors via NFC scanning. Manual overrides are provided for emergency use cases.

- **In-Room Smart Lock with Card Detection**  
  An FSR sensor inside each room detects the presence of the access card to activate a local smart lock. This system also serves as a designated card holder to minimize loss or misplacement.

- **Annual Data Synchronization**  
  Scheduled data updates ensure that room assignments and access privileges remain accurate, supporting effective administrative control.

## Summary

This system merges convenience and security by linking individual room access to personal cards, streamlining both access control and room management in shared or institutional environments.
