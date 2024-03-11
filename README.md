/ Include the necessary library
#include <Arduino.h>

// Define the LM35 analog pin
const int lm35Pin = A0;

// Define the onboard LED pin
const int ledPin = 13;

// Variables to store temperature readings
int temperature = 0;
int previousTemperature = 0;

// Variables to store last LED toggle time
unsigned long previousMillis = 0;

// Define blink intervals
const long intervalBelow30 = 250;
const long intervalAbove30 = 500;

// Setup function runs once at the beginning
void setup() {
  // Set the LED pin as an output
  pinMode(ledPin, OUTPUT);
  // Set the LM35 pin as an input
  pinMode(lm35Pin, INPUT);
}

// Main loop
void loop() {
  // Read the temperature from LM35 sensor
  temperature = analogRead(lm35Pin);
  // Convert the analog value to Celsius temperature
  temperature = (temperature * 500) / 1023;

  // Check if temperature falls below 30 degrees Celsius
  if (temperature < 30) {
    // Check if it's time to toggle LED
    if (millis() - previousMillis >= intervalBelow30) {
      // Toggle LED
      digitalWrite(ledPin, !digitalRead(ledPin));
      // Update previousMillis
      previousMillis = millis();
    }
  }
  // If temperature rises above 30 degrees Celsius
  else {
    // Check if it's time to toggle LED
    if (millis() - previousMillis >= intervalAbove30) {
      // Toggle LED
      digitalWrite(ledPin, !digitalRead(ledPin));
      // Update previousMillis
      previousMillis = millis();
    }
  }
}
