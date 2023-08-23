# TagoreInt_eok
#include <Servo.h>
#include <Keypad.h>

Servo servoMotor;
const int buttonPin = 2; // Change to the pin number where you connected the push button
const int ledPin = 13;   // Change to the pin number where you connected the LED

const byte ROW_NUM = 4;    // Number of rows in the keypad
const byte COLUMN_NUM = 4; // Number of columns in the keypad

// Define the keypad layout
char keys[ROW_NUM][COLUMN_NUM] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROW_NUM] = {7, 10, 11, 12};     // Connect to the row pinouts of the keypad
byte colPins[COLUMN_NUM] = {3, 4, 5, 6};  // Connect to the column pinouts of the keypad

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROW_NUM, COLUMN_NUM);

int buttonState = 0;
int cleanCount = 0;
bool isCleaning = false;

void setup() {
  servoMotor.attach(9); // Change to the pin number where you connected the servo signal wire
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(ledPin, OUTPUT); // Set the LED pin as an output
  Serial.begin(9600);
  Serial.println("Press 'C' on the keypad or use the push button to start cleaning.");
}

void loop() {
  char key = keypad.getKey();

  // Check if the 'C' key of the keypad is pressed or if the push button is pressed
  if ((key == 'C' && !isCleaning) || (digitalRead(buttonPin) == LOW && !isCleaning)) {
    startCleaning();
  }
}

