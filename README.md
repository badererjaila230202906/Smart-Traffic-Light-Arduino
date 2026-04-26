# Smart Traffic Light System using Arduino and Ultrasonic Sensor

## Overview
This project implements a smart traffic light control system using Arduino Uno and an ultrasonic sensor (HC-SR04). Unlike traditional traffic lights with fixed timing, this system adjusts the signal duration dynamically based on vehicle presence.

## Objective
The objective of this project is to improve traffic flow efficiency by adapting the duration of the traffic lights according to real-time conditions using sensor input.

## Components
- Arduino Uno
- Ultrasonic Sensor (HC-SR04)
- 3 LEDs (Red, Yellow, Green)
- Resistors
- Breadboard
- Connecting wires

## Working Principle
The ultrasonic sensor continuously measures the distance between the sensor and approaching vehicles.

- If a vehicle is detected (short distance):
  - Green light duration increases to allow more cars to pass

- If no vehicle is detected (long distance):
  - Green light duration decreases

- The system cycles between red, yellow, and green lights based on this logic

## Features
- Real-time vehicle detection
- Dynamic traffic signal control
- Efficient traffic flow simulation
- Simple and cost-effective design

## Simulation
The system is designed and tested using Proteus simulation software.

## Hardware Implementation
The same circuit can be implemented on real hardware using Arduino Uno and the listed components.

## Author
Bader Erjaila  
Student ID: 230202906
Project Progress Update

In this stage of the project, I have designed the road layout for a four-way intersection to support the implementation of a smart traffic light system.

The intersection was carefully structured to represent a realistic crossroad scenario, allowing multiple traffic directions. I have also organized and visually enhanced the layout to clearly distinguish the roads and lanes, which will help in placing and controlling the traffic signals in the next stages.

This design will serve as the foundation for implementing the traffic lights, sensors, and smart control logic.
