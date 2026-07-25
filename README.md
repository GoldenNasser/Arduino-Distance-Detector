# Arduino Distance Detector

This is a simple Arduino project. It uses a distance sensor to find objects. If an object is close, a motor moves and an LED turns on.

## Parts List
- Arduino Uno
- Ultrasonic Sensor (HC-SR04)
- Servo Motor
- LED
- 220 Ohm Resistor
- Breadboard and jumper wires

## Problems We Faced and How We Solved Them

**Problem 1: The LED did not turn on.**
- **Solution:** We made sure the LED was put in the right way. The long leg goes to the power, and the short leg goes to the ground. We also made sure to use the correct 220 Ohm resistor.

**Problem 2: The system kept turning on and off non-stop.**
- **Solution:** The servo motor was taking too much power quickly. This made the sensor give wrong numbers (like 0). We solved this by changing the code. We told the code to ignore bad numbers and added a small pause (`delay`) to let the power become stable.

## Code

```cpp
#include <Servo.h>

const int trigPin = 8;
const int echoPin = 9;
const int servoPin = 10;
const int ledPin = 11;

Servo myServo;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  
  myServo.attach(servoPin);
  myServo.write(0);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  long duration = pulseIn(echoPin, HIGH);
  long distance = (duration * 0.034) / 2;
  
  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance > 2 && distance < 400) {
    if (distance <= 10) {
      myServo.write(90);          
      digitalWrite(ledPin, HIGH); 
      delay(600);
    } else {
      myServo.write(0);           
      digitalWrite(ledPin, LOW);    
      delay(100);
    }
  }
}
