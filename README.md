## Author
Bader Erjaila  
Student ID: 230202906
Project Progress Update

# Smart Traffic Light System using Arduino and Ultrasonic SensorS

## Overview
This project implements a smart traffic light control system using Arduino Uno and ultrasonic sensors (HC-SR04). Unlike traditional traffic lights with fixed timing, this system dynamically adjusts the green light duration according to real-time traffic density. The system uses near and far sensors to detect normal traffic and congestion levels, allowing more efficient traffic management.
## Objective
The objective of this project is to improve traffic flow efficiency by adapting traffic light timing according to real-time traffic conditions. The system reduces unnecessary waiting time and gives priority to congested roads.

## Components
• Arduino Uno
• 4 HC-SR04 Ultrasonic Sensors
• 4 Traffic Light Modules
• Breadboard
• Jumper Wires
• USB Cable

## Working Principle
The system uses two ultrasonic sensors for each road:
Near sensor: Detects normal vehicle presence and gives a shorter green light duration (7 seconds).
Far sensor: Detects traffic congestion and gives a longer green light duration (12 seconds).
If no vehicles are detected, all traffic lights remain red. The system also uses a priority mechanism and fairness logic using the variable lastServed.

## Features
• Real-time vehicle detection using ultrasonic sensors
• Dynamic traffic signal timing based on traffic density
• Congestion priority system using far sensors
• Fair traffic control using lastServed logic
• Simple and cost-effective design
• Reduced unnecessary waiting time


## Simulation
The system is designed and tested using Proteus simulation software.
<img width="317" height="345" alt="image" src="https://github.com/user-attachments/assets/8860afa1-9ecf-4530-98ce-d8101f0c10b6" />


## Hardware Implementation
The hardware implementation was developed using Arduino Uno, HC-SR04 ultrasonic sensors, and traffic light modules. The system was connected using a breadboard and jumper wires. The same logic used in the simulation was successfully implemented on real hardware. 
<img width="465" height="541" alt="image" src="https://github.com/user-attachments/assets/834a1d3f-bfce-401f-af3f-b045843b5be5" />


 Code Logic
 ## Arduino Code Explanation

The Arduino program controls the entire Smart Traffic Light System by reading the ultrasonic sensor values and controlling the traffic light modules according to traffic density.

At the beginning of the program, all sensor pins and traffic light pins are defined using pinMode(). The ultrasonic sensors use TRIG pins as OUTPUT to send ultrasonic waves and ECHO pins as INPUT to receive the reflected signals.

The function readSensor() is used to measure the distance between the sensor and vehicles. The measured distance is compared with a threshold value to determine whether a vehicle is detected or not.

The system uses two sensors for each road:

• Near sensor: Detects normal vehicle presence and gives a shorter green light duration (7 seconds).

• Far sensor: Detects traffic congestion and gives a longer green light duration (12 seconds).

The functions getLevel13() and getLevel24() determine the traffic density level for each road:

• Level 0 → No vehicles detected

• Level 1 → Near sensor active

• Level 2 → Far sensor active (traffic congestion)

The loop() function continuously checks traffic conditions and dynamically changes the traffic signal timing. If both roads contain vehicles, the system gives priority to the more congested road.

The variable lastServed is used to prevent the same road from receiving green light repeatedly and to ensure fairness between roads.

The functions open13() and open24() control the sequence of green, yellow, and red traffic lights for each road.

### Pin Connections

The Smart Traffic Light System uses Arduino Uno pins for both ultrasonic sensors and traffic light modules.

Ultrasonic Sensor Connections:

• Far Sensor Road 1/3

  - TRIG → Digital Pin 2

  - ECHO → Digital Pin 3

• Near Sensor Road 1/3

  - TRIG → Digital Pin 4

  - ECHO → Digital Pin 5

• Far Sensor Road 2/4

  - TRIG → Digital Pin 6

  - ECHO → Digital Pin 7

• Near Sensor Road 2/4

  - TRIG → Digital Pin 8

  - ECHO → Digital Pin 9

Traffic Light Connections:

• Road 1/3 Traffic Light

  - Green LED → A0

  - Yellow LED → A1

  - Red LED → A2

• Road 2/4 Traffic Light

  - Green LED → A3

  - Yellow LED → A4

  - Red LED → A5

Power Connections:

• All VCC pins are connected to 5V.

• All GND pins are connected to the common ground line on the breadboard.


## Arduino Code

#define TRIG_FAR13 2

#define ECHO_FAR13 3

#define TRIG_NEAR13 4

#define ECHO_NEAR13 5

#define TRIG_FAR24 6

#define ECHO_FAR24 7

#define TRIG_NEAR24 8

#define ECHO_NEAR24 9

#define G13 A0

#define Y13 A1

#define R13 A2

#define G24 A3

#define Y24 A4

#define R24 A5

int threshold = 15;

int lastServed = 0;   // 0 none, 13, 24

long readSensor(int trig, int echo) {

  digitalWrite(trig, LOW);

  delayMicroseconds(2);

  digitalWrite(trig, HIGH);

  delayMicroseconds(10);

  digitalWrite(trig, LOW);

  long duration = pulseIn(echo, HIGH, 30000);

  if (duration == 0) return 999;

  return duration * 0.034 / 2;

}

void setup() {

  pinMode(TRIG_FAR13, OUTPUT);

  pinMode(ECHO_FAR13, INPUT);

  pinMode(TRIG_NEAR13, OUTPUT);

  pinMode(ECHO_NEAR13, INPUT);

  pinMode(TRIG_FAR24, OUTPUT);

  pinMode(ECHO_FAR24, INPUT);

  pinMode(TRIG_NEAR24, OUTPUT);

  pinMode(ECHO_NEAR24, INPUT);

  pinMode(G13, OUTPUT);

  pinMode(Y13, OUTPUT);

  pinMode(R13, OUTPUT);

  pinMode(G24, OUTPUT);

  pinMode(Y24, OUTPUT);

  pinMode(R24, OUTPUT);

  allRed();

}

void loop() {

  int L13 = getLevel13();

  delay(100);

  int L24 = getLevel24();

  delay(100);

  if (L13 == 0 && L24 == 0) {

    allRed();

    lastServed = 0;

    delay(500);

    return;

  }

  if (L13 > 0 && L24 > 0) {

    if (lastServed == 13) {

      open24(timeFor(L24));

      lastServed = 24;

    }

    else if (lastServed == 24) {

      open13(timeFor(L13));

      lastServed = 13;

    }

    else {

      if (L13 > L24) {

        open13(timeFor(L13));

        lastServed = 13;

      }

      else if (L24 > L13) {

        open24(timeFor(L24));

        lastServed = 24;

      }

      else {

        open13(timeFor(L13));

        lastServed = 13;

      }

    }

  }

  else if (L13 > 0) {

    open13(timeFor(L13));

    lastServed = 13;

  }

  else if (L24 > 0) {

    open24(timeFor(L24));

    lastServed = 24;

  }

  allRed();

  delay(300);

}

int getLevel13() {

  long far13 = readSensor(TRIG_FAR13, ECHO_FAR13);

  delay(80);

  long near13 = readSensor(TRIG_NEAR13, ECHO_NEAR13);

  if (far13 < threshold) return 2;

  if (near13 < threshold) return 1;

  return 0;

}

int getLevel24() {

  long far24 = readSensor(TRIG_FAR24, ECHO_FAR24);

  delay(80);

  long near24 = readSensor(TRIG_NEAR24, ECHO_NEAR24);

  if (far24 < threshold) return 2;

  if (near24 < threshold) return 1;

  return 0;

}

int timeFor(int L) {

  if (L == 1) return 7000;

  if (L == 2) return 12000;

  return 0;

}

void allRed() {

  digitalWrite(G13, LOW);

  digitalWrite(Y13, LOW);

  digitalWrite(R13, HIGH);

  digitalWrite(G24, LOW);

  digitalWrite(Y24, LOW);

  digitalWrite(R24, HIGH);

}

void open13(int t) {

  allRed();

  digitalWrite(R13, LOW);

  digitalWrite(G13, HIGH);

  delay(t);

  digitalWrite(G13, LOW);

  digitalWrite(Y13, HIGH);

  delay(2000);

  digitalWrite(Y13, LOW);

  digitalWrite(R13, HIGH);

}

void open24(int t) {

  allRed();

  digitalWrite(R24, LOW);

  digitalWrite(G24, HIGH);

  delay(t);

  digitalWrite(G24, LOW);

  digitalWrite(Y24, HIGH);

  delay(2000);

  digitalWrite(Y24, LOW);

  digitalWrite(R24, HIGH);

}

## CONCLUSION
This project successfully demonstrates a Smart Traffic Light System using Arduino Uno and ultrasonic sensors. Unlike traditional traffic light systems with fixed timing, the developed system dynamically adjusts the traffic signal duration according to real-time traffic density.
The use of near and far ultrasonic sensors allowed the system to distinguish between normal traffic and traffic congestion, providing longer green light duration for congested roads and shorter duration for normal traffic conditions.
The project also implemented a fairness mechanism using the variable lastServed to prevent one road from repeatedly receiving priority. Overall, the system improved traffic flow efficiency, reduced unnecessary waiting time, and provided a simple, low-cost, and effective smart traffic management solution.


 
