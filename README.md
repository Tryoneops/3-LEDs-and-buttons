# 3-LEDs-and-buttons
![image](https://github.com/user-attachments/assets/0cf0a331-e84a-4804-b7f7-13cdea92f0dd)
https://www.tinkercad.com/things/1EaqaRSQxtT-3-leds-button?sharecode=fGUFMGr8r_J_AVjwvHwMPiqh5Gi18Qwh-6LxLtEU1X4

code: 

#define Pressed  LOW
#define Released HIGH

#define LEDon    LOW
#define LEDoff   HIGH

const byte switchOne    = 7;
const byte switchTwo    = 8;
const byte switchThree  = 9;

const byte LED_One      = 10;
const byte LED_Two      = 11;
const byte LED_Three    = 12;

const byte heartBeatLED = 13;

byte switchOneState;
byte lastswitchOneState;
byte lastswitchTwoState;
byte lastswitchThreeState;

unsigned long heartBeatMillis;
unsigned long switchMillis;

void setup() {
  pinMode(switchOne, INPUT_PULLUP);
  pinMode(switchTwo, INPUT_PULLUP);
  pinMode(switchThree, INPUT_PULLUP);

  pinMode(heartBeatLED, OUTPUT);

  pinMode(LED_One, OUTPUT);
  digitalWrite(LED_One, LEDon);

  pinMode(LED_Two, OUTPUT);
  digitalWrite(LED_Two, LEDon);

  pinMode(LED_Three, OUTPUT);
  digitalWrite(LED_Three, LEDon);
}

void loop() {
  heartBeat();

  if (millis() - switchMillis >= 1ul) {
    switchMillis = millis();

    if (checkSwitch(switchOne, lastswitchOneState)) {
      digitalWrite(LED_One, !digitalRead(LED_One));
    }

    if (checkSwitch(switchTwo, lastswitchTwoState)) {
      digitalWrite(LED_One, !digitalRead(LED_One));
      digitalWrite(LED_Two, !digitalRead(LED_Two));
    }

    if (checkSwitch(switchThree, lastswitchThreeState)) {
      digitalWrite(LED_Two, !digitalRead(LED_Two));
      digitalWrite(LED_Three, !digitalRead(LED_Three));
    }
  }
}

void heartBeat() {
  if (millis() - heartBeatMillis < 700ul) return;
  heartBeatMillis = millis();
  digitalWrite(heartBeatLED, !digitalRead(heartBeatLED));
}

bool checkSwitch(byte switchPin, byte &lastState) {
  byte thisSwitchState = digitalRead(switchPin);
  if (thisSwitchState != lastState) {
    lastState = thisSwitchState;
    if (thisSwitchState == Pressed) {
      return true;
    }
  }
  return false;
}
