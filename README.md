void setup() {
  // put your setup code here, to run once:
#include <Keypad>
#include <LiquidCrystal>

const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1', '2', '3', '+'},
  {'4', '5', '6', '-'},
  {'7', '8', '9', '*'},
  {'C', '0', '=', '/'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(12, 11, 10, 16, 15, 14);

float num1, num2;
char operand;
boolean operandEntered = false;

void setup() {
  lcd.begin(16, 2);
  lcd.print("Arduino Calc");
  delay(1000);
  lcd.clear();
}

void loop() {
  char key = keypad.getKey();

  if (key != NO_KEY) {
    if (isdigit(key)) {
      if (!operandEntered) {
        num1 = num1 * 10 + (key - '0');
        lcd.print(key);
      } else {
        num2 = num2 * 10 + (key - '0');
        lcd.print(key);
      }
    } else if (key == '+' || key == '-' || key == '*' || key == '/') {
      if (!operandEntered) {
        operand = key;
        operandEntered = true;
        lcd.print(key);
      }
    } else if (key == '=') {
      float result;
      switch (operand) {
        case '+':
          result = num1 + num2;
          break;
        case '-':
          result = num1 - num2;
          break;
        case '*':
          result = num1 * num2;
          break;
        case '/':
          if (num2 == 0) {
            lcd.clear();
            lcd.print("Error: Divide by 0");
            delay(2000);
            lcd.clear();
            break;
          }
          result = num1 / num2;
          break;
      }
      lcd.clear();
      lcd.print("Result: ");
      lcd.print(result);
      num1 = result;
      num2 = 0;
      operandEntered = false;
    } else if (key == 'C') {
      num1 = num2 = 0;
      operand = '\0';
      operandEntered = false;
      lcd.clear();
    }
  }
}


}

void loop() {
  // put your main code here, to run repeatedly:

}
