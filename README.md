#include <Keypad.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

const byte ROWS = 4;
const byte COLS = 3;

char keys[ROWS][COLS] = {
  {'1','2','3'},
  {'4','5','6'},
  {'7','8','9'},
  {'*','0','#'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String password = "7234";
String input = "";

void setup() {
  pinMode(13, OUTPUT);
  digitalWrite(13, LOW);

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("Enter Password");
}

void loop() {
  char key = keypad.getKey();

  if (key) {

    if (key == '#') {

      lcd.clear();

      if (input == password) {
        lcd.print("Correct");
        digitalWrite(13, HIGH); // Turn ON pin 13
      }
      else {
        lcd.print("Wrong Password");
        digitalWrite(13, LOW);
      }

      delay(2000);

      input = "";
      lcd.clear();
      lcd.print("Enter Password");
    }

    else if (key == '*') {
      input = "";
      lcd.clear();
      lcd.print("Cleared");
      delay(1000);

      lcd.clear();
      lcd.print("Enter Password");
    }

    else {
      input += key;

      lcd.setCursor(0, 1);
      lcd.print("                "); // clear line
      lcd.setCursor(0, 1);

      for (int i = 0; i < input.length(); i++) {
        lcd.print("*");
      }
    }
  }
}




For this project, you'll need:
Hardware
Arduino Uno (or Nano)
4×3 Keypad (the one in your photo)
16×2 LCD with I2C module
Jumper wires
USB cable for Arduino
Breadboard (optional)
Software
Arduino IDE
Download from: arduino.cc⁠�
Libraries (install from Arduino IDE)
Keypad by Mark Stanley & Alexander Brevig
LiquidCrystal_I2C
How to install libraries
In Arduino IDE:
Sketch → Include Library → Manage Libraries
Search for:
Keypad
LiquidCrystal I2C
Click Install
Before uploading
You may need to check the LCD I2C address:
Common addresses are 0x27 and 0x3F
If the LCD shows nothing, try changing:
C++
LiquidCrystal_I2C lcd(0x27, 16, 2);
to:
C++
LiquidCrystal_I2C lcd(0x3F, 16, 2);
What Arduino board are you using: Uno, Nano, or something else? I can give the exact wiring diagram.





Type 7234
Press #
LCD → Correct
Pin 13 → ON (built-in LED on most Arduino Uno boards lights up)





LCD I2C
Arduino
GND
GND
VCC
5V
SDA
A4
SCL
A5
Keypad → Arduino Uno
The keypad has 7 pins.
Keypad Pin
Arduino
R1
D9
R2
D8
R3
D7
R4
D6
C1
D5
C2
D4
C3
D3
Pin 13 Output
Arduino Pin
Device
D13
LED (+) or Relay IN
GND
LED (-) through 220Ω resistor
Or use the built-in LED on the Uno (connected to pin 13 already).
Simple Diagram
Plain text
LCD I2C
VCC ---- 5V
GND ---- GND
SDA ---- A4
SCL ---- A5

KEYPAD
R1 ---- D9
R2 ---- D8
R3 ---- D7
R4 ---- D6
C1 ---- D5
C2 ---- D4
C3 ---- D3

LED
D13 ---- LED ---- 220Ω ---- GND
