#include <Servo.h>

// Define pin assignments
const int soilMoisturePin = A0;
const int potentiometerPin = A1;
const int pumpPin = 9;
const int ledPin = 11;
const int buzzerPin = 12;
const int buttonPin = 2;

Servo servoMotor;

void setup() {
  pinMode(soilMoisturePin, INPUT);
  pinMode(potentiometerPin, INPUT);
  pinMode(pumpPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  servoMotor.attach(10);
}

void loop() {
  // Check if the button is pressed to wake up the system
  if (digitalRead(buttonPin) == LOW) {
    int soilMoisture = analogRead(soilMoisturePin);
    int thresholdValue = map(analogRead(potentiometerPin), 0, 1023, 0, 1023); // Read potentiometer value and map it to the range [0, 1023]

    if (soilMoisture < thresholdValue) {
      // Water the plants
      digitalWrite(pumpPin, HIGH);
      servoMotor.write(90);
      delay(5000); // Watering duration

      // Provide feedback
      digitalWrite(ledPin, HIGH);
      tone(buzzerPin, 2000, 100); // Emit a short beep

      delay(5000); // Watering duration
      
      // Stop watering
      digitalWrite(pumpPin, LOW);
      servoMotor.write(0);

      // Provide feedback
      digitalWrite(ledPin, LOW);
      tone(buzzerPin, 2000, 100); // Emit a short beep
    }
  } else {
    // Enter sleep mode
    // Add code to put the system in sleep mode here
  }

  delay(1000); // Check soil moisture every second
}

