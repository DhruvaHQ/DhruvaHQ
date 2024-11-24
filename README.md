// Pin definitions
const int ledPin = 13;      // Built-in LED pin
const int cardPin = 2;      // Pin for card detection

// Function prototypes
void checkCardStatus();
void handleBluetoothCommand(char command);
void blinkLED();

void setup() {
  // Set up the LED and card detection pins
  pinMode(ledPin, OUTPUT);
  pinMode(cardPin, INPUT_PULLUP);
  
  // Initialize Serial communication for Bluetooth (using pins 0 and 1)
  Serial.begin(9600);
  Serial.println("Smart Wallet System Initialized");
}

void loop() {
  // Continuously check card status
  checkCardStatus();

  // Check if data is received from Bluetooth
  if (Serial.available()) {
    char command = Serial.read();
    handleBluetoothCommand(command);
  }

  delay(200); // Short delay to stabilize readings
}

// Function to check the card status
void checkCardStatus() {
  int cardStatus = digitalRead(cardPin);
  
  // If the card is removed, blink LED and send alert
  if (cardStatus == HIGH) { 
    blinkLED();
    Serial.println("Alert: Smart Card Removed!");
  } else {
    digitalWrite(ledPin, LOW); // Turn off LED if the card is present
  }
}

// Function to handle incoming Bluetooth commands
void handleBluetoothCommand(char command) {
  if (command == 'A') { // If the command is 'A', blink the LED
    blinkLED();
    Serial.println("Action Triggered: Blinking LED");
  }
}

// Function to blink the LED
void blinkLED() {
  for (int i = 0; i < 3; i++) { // Blink the LED 3 times
    digitalWrite(ledPin, HIGH);
    delay(300);
    digitalWrite(ledPin, LOW);
    delay(300);
  }
}
