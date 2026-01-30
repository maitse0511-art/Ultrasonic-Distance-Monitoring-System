#include <LiquidCrystal.h>   // Library for controlling the LCD

// LCD pin connections
int RS = 12;
int E  = 10;
int D4 = 9;
int D5 = 8;
int D6 = 7;
int D7 = 6;

// LED pin definitions
int redLed    = 4;
int greenLed  = 5;
int orangeLed = 11;

// Control variables
int BuzzTimes = 10;     // Number of warning blinks
int redBlink  = 10;     // (Declared but not currently used)

// Create LCD object (parallel mode)
LiquidCrystal lcd(RS, E, D4, D5, D6, D7);

// Ultrasonic sensor pins
int trigPin = 2;
int echoPin = 3;

// Variables for distance measurement
long  TravelTime;        // Time taken for echo to return (µs)
float TravelDistance;    // Calculated distance (cm)
int   TravelTarget;      // (Declared but not currently used)

void setup() {
  Serial.begin(9600);        // Start serial communication
  lcd.begin(16, 4);          // Initialize 16x4 LCD

  // Set LED pins as outputs
  pinMode(redLed, OUTPUT);
  pinMode(greenLed, OUTPUT);
  pinMode(orangeLed, OUTPUT);

  // Set ultrasonic sensor pin modes
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // Display startup message
  lcd.setCursor(0, 0);
  lcd.print("System Ready");
  delay(1000);
  lcd.clear();
}

void loop() {

  // Display distance label on LCD (value updated later)
  lcd.setCursor(0, 0);
  lcd.print("Distance:");
  lcd.print(TravelDistance);
  lcd.print(" cm");

  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(10);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure echo pulse duration
  TravelTime = pulseIn(echoPin, HIGH);

  delay(500);  // Measurement interval delay

  // Convert time to distance in cm
  TravelDistance = (TravelTime * 0.034) / 2;

  // Clear previous warning/status messages
  lcd.clear();

  // CAUTION ZONE (10–20 cm)
  if (TravelDistance >= 10 && TravelDistance <= 20) {

    digitalWrite(redLed, LOW);
    digitalWrite(greenLed, LOW);
    digitalWrite(orangeLed, HIGH);

    lcd.setCursor(4, 2);
    lcd.print("BE CAUTIOUS");
    lcd.setCursor(4, 3);
    lcd.print("YOU ARE CLOSE");
  }

  // DANGER ZONE (< 10 cm)
  else if (TravelDistance < 10) {

    int B = 0;
    while (B < BuzzTimes) {

      // Blink red LED as warning
      digitalWrite(redLed, HIGH);
      delay(50);
      digitalWrite(redLed, LOW);
      digitalWrite(greenLed, LOW);
      delay(50);

      // Re-display distance and warning
      lcd.setCursor(0, 0);
      lcd.print("Distance:");
      lcd.print(TravelDistance);
      lcd.print(" cm");

      lcd.setCursor(4, 2);
      lcd.print("WARNING, TOO CLOSE");

      B = B + 1;   // Increment blink counter
    }
  }

  // SAFE ZONE (> 20 cm)
  else {
    digitalWrite(greenLed, HIGH);
    digitalWrite(redLed, LOW);
    digitalWrite(orangeLed, LOW);

    lcd.setCursor(4, 2);
    lcd.print("SAFE ZONE");
  }

  // Print distance to Serial Monitor (debugging)
  Serial.print("Distance: ");
  Serial.print(TravelTime);   // NOTE: This prints time, not distance
  Serial.println(" cm");
}
