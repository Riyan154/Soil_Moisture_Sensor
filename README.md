
#include <DHT.h>
#include <LiquidCrystal.h>

#define DHTPIN 7
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

const int soilPin = A0;
const int greenLED = 8;
const int yellowLED = 9;
const int redLED = 10;
const int relayPin = 13;

// Moisture thresholds
const int GREEN_THRESHOLD = 85;
const int YELLOW_THRESHOLD = 69;

// State: 2 = dry, 1 = getting dry, 0 = moist
int currentState = 2;
int pendingState = 2;
int stableCount = 0;

const int REQUIRED_READINGS = 2;

int readSoil() {
  long total = 0;

  for (int i = 0; i < 5; i++) {
    total += analogRead(soilPin);
    delay(10);
  }

  return total / 5;
}

void showLED(int state) {
  digitalWrite(redLED, state == 2 ? HIGH : LOW);
  digitalWrite(yellowLED, state == 1 ? HIGH : LOW);
  digitalWrite(greenLED, state == 0 ? HIGH : LOW);
}

void controlPump(int state) {
  // Active-low relay: LOW = ON, HIGH = OFF
  // Pump runs only when soil is dry
  if (state == 2) {
    digitalWrite(relayPin, HIGH);
  } else {
    digitalWrite(relayPin, LOW);
  }
}

void setup() {
  Serial.begin(9600);

  dht.begin();
  lcd.begin(16, 2);

  pinMode(greenLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(redLED, OUTPUT);

  pinMode(relayPin, OUTPUT);

  // Keep pump OFF at startup
  digitalWrite(relayPin, HIGH);

  lcd.setCursor(0, 0);
  lcd.print("AquaSaver");
  lcd.setCursor(0, 1);
  lcd.print("Starting...");
  delay(2000);
  lcd.clear();
}

void loop() {
  int soilValue = readSoil();

  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  int moisturePercent = map(soilValue, 1023, 250, 0, 100);
  moisturePercent = constrain(moisturePercent, 0, 100);

  int requestedState;

  if (moisturePercent >= GREEN_THRESHOLD) {
    requestedState = 0;
  } else if (moisturePercent >= YELLOW_THRESHOLD) {
    requestedState = 1;
  } else {
    requestedState = 2;
  }

  // Require consistent readings before changing state
  if (requestedState != currentState) {
    if (requestedState == pendingState) {
      stableCount++;
    } else {
      pendingState = requestedState;
      stableCount = 1;
    }

    if (stableCount >= REQUIRED_READINGS) {
      currentState = requestedState;
      stableCount = 0;
    }
  } else {
    pendingState = currentState;
    stableCount = 0;
  }

  // Update LEDs and pump
  showLED(currentState);
  controlPump(currentState);

  const char *soilStatus;

  if (currentState == 2) {
    soilStatus = "DRY";
  } else if (currentState == 1) {
    soilStatus = "GETTING DRY";
  } else {
    soilStatus = "MOIST";
  }

  // LCD row 1: soil status only
  lcd.setCursor(0, 0);
  lcd.print("                ");
  lcd.setCursor(0, 0);
  lcd.print("Soil:");
  lcd.print(soilStatus);

  // LCD row 2: temperature and humidity
  lcd.setCursor(0, 1);
  lcd.print("                ");
  lcd.setCursor(0, 1);

  if (isnan(humidity) || isnan(temperature)) {
    lcd.print("DHT11 read error");
  } else {
    lcd.print("T:");
    lcd.print(temperature, 0);
    lcd.print("C H:");
    lcd.print(humidity, 0);
    lcd.print("%");
  }

  // Serial Monitor
  Serial.print("Soil Status: ");
  Serial.print(soilStatus);

  if (isnan(humidity) || isnan(temperature)) {
    Serial.println(" | DHT11 read error");
  } else {
    Serial.print(" | Temperature: ");
    Serial.print(temperature, 1);
    Serial.print(" C | Humidity: ");
    Serial.print(humidity, 1);
    Serial.print("%");
    Serial.print(" | Pump: ");
    Serial.println(currentState == 2 ? "ON" : "OFF");
  }

  delay(1000);
}

