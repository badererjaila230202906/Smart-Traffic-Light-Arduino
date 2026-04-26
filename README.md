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
<img width="4284" height="5712" alt="image" src="https://github.com/user-attachments/assets/74c70e44-b6ce-4e53-b44f-cc686e1b391e" />
Implementation Update

After setting up the Arduino Uno and connecting all four traffic light modules, the system was tested successfully using Proteus simulation.

Since the HC-SR04 ultrasonic sensor was not available in the Proteus library, a potentiometer (POT-HG) was used as an alternative to simulate vehicle detection.

The potentiometer is connected as follows:

* One terminal → VCC
* One terminal → GND
* Middle terminal → A0 (analog input of Arduino)

⸻

🧠 Smart Behavior

The potentiometer simulates traffic density by providing a variable analog value (0–1023):

* When the potentiometer is rotated down (low value)
    → Represents no vehicles
    → Green light duration = 3 seconds
* When the potentiometer is in the middle
    → Represents moderate traffic
    → Green light duration = 5 seconds
* When the potentiometer is rotated up (high value)
    → Represents heavy traffic
    → Green light duration = 8 seconds

This allows the system to dynamically adjust the green light duration based on traffic conditions, making the system smart and adaptive.
<img width="3024" height="4032" alt="image" src="https://github.com/user-attachments/assets/45ba2e10-af1e-4a99-bbbd-90e1bdcc8a57" />


 Code Logic
 ## Arduino Code Explanation

This code controls a 4-lane smart traffic light system using Arduino Uno.  
Each lane has three lights: Green, Yellow, and Red.

The system also uses a potentiometer connected to analog pin A0. The potentiometer is used to simulate vehicle density instead of the ultrasonic sensor in Proteus.

### Pin Connections

Lane 1:
- Green → Pin 2
- Yellow → Pin 3
- Red → Pin 4

Lane 2:
- Green → Pin 5
- Yellow → Pin 6
- Red → Pin 7

Lane 3:
- Green → Pin 8
- Yellow → Pin 9
- Red → Pin 10

Lane 4:
- Green → Pin 11
- Yellow → Pin 12
- Red → Pin 13

Potentiometer:
- One side → VCC
- Other side → GND
- Middle pin → A0

---

## Smart Logic

The Arduino reads the potentiometer value from A0.

The value range is from 0 to 1023:

- Low value: no vehicles → Lanes green time = 3 seconds
- Medium value: normal traffic → Lanes green time = 5 seconds
- High value: heavy traffic → Lanes green time = 8 seconds

This makes the traffic light system adaptive because the green time changes based on the simulated traffic condition.

---

## Arduino Code

// Smart Traffic Light System - All Lanes Smart

// Lane 1

int G1 = 2;

int Y1 = 3;

int R1 = 4;

// Lane 2

int G2 = 5;

int Y2 = 6;

int R2 = 7;

// Lane 3

int G3 = 8;

int Y3 = 9;

int R3 = 10;

// Lane 4

int G4 = 11;

int Y4 = 12;

int R4 = 13;

// Potentiometer

int sensorPin = A0;

// Timing

int greenTime;

int yellowTime = 4000;

void setup() {

  pinMode(G1, OUTPUT); pinMode(Y1, OUTPUT); pinMode(R1, OUTPUT);

  pinMode(G2, OUTPUT); pinMode(Y2, OUTPUT); pinMode(R2, OUTPUT);

  pinMode(G3, OUTPUT); pinMode(Y3, OUTPUT); pinMode(R3, OUTPUT);

  pinMode(G4, OUTPUT); pinMode(Y4, OUTPUT); pinMode(R4, OUTPUT);

}

void allOff() {

  digitalWrite(G1, LOW); digitalWrite(Y1, LOW); digitalWrite(R1, LOW);

  digitalWrite(G2, LOW); digitalWrite(Y2, LOW); digitalWrite(R2, LOW);

  digitalWrite(G3, LOW); digitalWrite(Y3, LOW); digitalWrite(R3, LOW);

  digitalWrite(G4, LOW); digitalWrite(Y4, LOW); digitalWrite(R4, LOW);

}

void loop() {

  int sensorValue = analogRead(sensorPin);

  // Smart timing for ALL lanes

  if (sensorValue < 400) {

    greenTime = 3000;

  }

  else if (sensorValue < 700) {

    greenTime = 5000;

  }

  else {

    greenTime = 8000;

  }

  // Lane 1

  allOff();

  digitalWrite(G1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(R4, HIGH);

  delay(greenTime);

  allOff();

  digitalWrite(Y1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(R4, HIGH);

  delay(yellowTime);

  // Lane 2

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(G2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(R4, HIGH);

  delay(greenTime);

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(Y2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(R4, HIGH);

  delay(yellowTime);

  // Lane 3

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(G3, HIGH);

  digitalWrite(R4, HIGH);

  delay(greenTime);

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(Y3, HIGH);

  digitalWrite(R4, HIGH);

  delay(yellowTime);

  // Lane 4

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(G4, HIGH);

  delay(greenTime);

  allOff();

  digitalWrite(R1, HIGH);

  digitalWrite(R2, HIGH);

  digitalWrite(R3, HIGH);

  digitalWrite(Y4, HIGH);

  delay(yellowTime);

}
---

## Code Summary

The code works by turning on one lane at a time while keeping the other lanes red. Before switching to the next lane, the yellow light turns on for 4 seconds as a transition signal.

The smart part of the system is applied to Lanes. The potentiometer value controls how long Lanes stays green:

- Low potentiometer value → 3 seconds
- Medium potentiometer value → 5 seconds
- High potentiometer value → 8 seconds

This simulates different traffic conditions and allows the system to respond dynamically.
 
