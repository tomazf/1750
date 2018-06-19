//#include <Wire.h>                   // we're using software lib
#include <SoftwareWire.h>
#include <SoftwareSerial.h>
#include <math.h>

int BH1750_Device_1 = 0x23;           // I2C address for light sensor
int BH1750_Device_2 = 0x5C;           // I2C address for light sensor

SoftwareWire myWire_1(8, 9);            // SDA, SCL  -  due to HW mods
SoftwareWire myWire_2(11, 10);          // SDA, SCL
SoftwareWire myWire_3(13, 12);          // SDA, SCL

SoftwareSerial lcd(3, 2);               // RX, TX

volatile unsigned int s1;
volatile unsigned int s2;
volatile unsigned int s3;
volatile unsigned int s4;
volatile unsigned int s5;

// for first run
bool firstRun = true;

void setup() {
  //Wire.begin();
  myWire_1.begin();
  myWire_2.begin();
  myWire_3.begin();

  Serial.begin(9600);
  lcd.begin(9600);

  //  Wire.beginTransmission (BH1750_Device_1);
  //  Wire.write(0x10);                             // Set resolution to 1 Lux
  //  Wire.endTransmission();
  //
  //  Wire.beginTransmission (BH1750_Device_2);
  //  Wire.write(0x10);                             // Set resolution to 1 Lux
  //  Wire.endTransmission();

  myWire_1.beginTransmission (BH1750_Device_1);
  myWire_1.write(0x10);                             // Set resolution to 1 Lux
  myWire_1.endTransmission();

  myWire_1.beginTransmission (BH1750_Device_2);
  myWire_1.write(0x10);                             // Set resolution to 1 Lux
  myWire_1.endTransmission();

  myWire_2.beginTransmission (BH1750_Device_1);
  myWire_2.write(0x10);                             // Set resolution to 1 Lux
  myWire_2.endTransmission();

  myWire_2.beginTransmission (BH1750_Device_2);
  myWire_2.write(0x10);                             // Set resolution to 1 Lux
  myWire_2.endTransmission();

  myWire_3.beginTransmission (BH1750_Device_1);
  myWire_3.write(0x10);                             // Set resolution to 1 Lux
  myWire_3.endTransmission();

  // setup end
  Serial.println("Setup end!");
  delay(5500);

  // LCD display from master
  // just for debug on reset
  clearDisplay();
  delay(200);
  setLCDCursor(2);
  lcd.print("* MASTER RESET *");
  setLCDCursor(42);
  lcd.print("initializing");
  delay(1000);
  setLCDCursor(54);
  lcd.print(".");
  delay(1000);
  setLCDCursor(55);
  lcd.print(".");
  delay(1000);
  setLCDCursor(56);
  lcd.print(".");
  delay(1000);
  setLCDCursor(78);
  lcd.print("OK");
  delay(2000);
  clearDisplay();

  Serial.println("Setup end - LCD sent!");

}

void loop() {

  // read all sensors
  readSensors(false);     // false = round function!

  // send to remote LCD   // SW uart
  //clearDisplay();       // disabled, instead we pad with whitespace (no blinking)

  if (firstRun) {
    setLCDCursor(0);
    lcd.print("+------ LUXi ------+");
    firstRun = false;
  }

  /*
    lcd.print(s1,DEC);
    setLCDCursor(5);
    lcd.print(s2, DEC);
    setLCDCursor(11);
    lcd.print(s3, DEC);
    setLCDCursor(16);
    lcd.print(s4, DEC);
    setLCDCursor(21);
    lcd.print(s5, DEC);
    setLCDCursor(27);
    lcd.print("lux");
  */

  printLine(2);
  printLine(3);
  printLine(4);

  // manual test
  //printLine(4,"+------ L.X -------+");

  // debug
  displayToSerial();        // local HW uart

  // for sensor reading
  delay (200);
}

void displayToSerial() {
  Serial.print(s1, DEC);
  Serial.print(",");
  Serial.print(s2, DEC);
  Serial.print(",");
  Serial.print(s3, DEC);
  Serial.print(",");
  Serial.print(s4, DEC);
  Serial.print(",");
  Serial.print(s5, DEC);
  Serial.println();
}

void readSensors(bool fine) {
  if (!fine)
  {
    s1 = round(BH1750_Read_b1(BH1750_Device_1) / 10.0) * 10;
    s2 = round(BH1750_Read_b1(BH1750_Device_2) / 10.0) * 10;
    s3 = round(BH1750_Read_b2(BH1750_Device_1) / 10.0) * 10;
    s4 = round(BH1750_Read_b2(BH1750_Device_2) / 10.0) * 10;
    s5 = round(BH1750_Read_b3(BH1750_Device_1) / 10.0) * 10;
  }
  else {
    s1 = BH1750_Read_b1(BH1750_Device_1);
    s2 = BH1750_Read_b1(BH1750_Device_2);
    s3 = BH1750_Read_b2(BH1750_Device_1);
    s4 = BH1750_Read_b2(BH1750_Device_2);
    s5 = BH1750_Read_b3(BH1750_Device_1);
  }
}

unsigned int BH1750_Read_b1(int addr) {
  unsigned int i = 0;
  myWire_1.beginTransmission (addr);
  myWire_1.requestFrom (addr, 2);
  while (myWire_1.available())
  {
    i <<= 8;
    i |= myWire_1.read();
  }
  myWire_1.endTransmission();
  return i / 1.2;                     // Convert to Lux
}

unsigned int BH1750_Read_b2(int addr) {
  unsigned int i = 0;
  myWire_2.beginTransmission (addr);
  myWire_2.requestFrom (addr, 2);
  while (myWire_2.available())
  {
    i <<= 8;
    i |= myWire_2.read();
  }
  myWire_2.endTransmission();
  return i / 1.2;                     // Convert to Lux
}

unsigned int BH1750_Read_b3(int addr) {
  unsigned int i = 0;
  myWire_3.beginTransmission (addr);
  myWire_3.requestFrom (addr, 2);
  while (myWire_3.available())
  {
    i <<= 8;
    i |= myWire_3.read();
  }
  myWire_3.endTransmission();
  return i / 1.2;                     // Convert to Lux
}

void clearDisplay()
{
  lcd.write((unsigned char)0xFE);               // send the special command
  lcd.write((unsigned char)0x01);               // send the clear screen command
}

void setLCDCursor(byte cursor_position)
{
  lcd.write((unsigned char)0xFE);               // send the special command
  lcd.write((unsigned char)0x80);               // send the set cursor command
  lcd.write((unsigned char)cursor_position);    // send the cursor position
}

void printLine(int line) {                      // padding with spaces

  String dataW = "";
  setLCDCursor((line - 1) * 20);                // set line

  switch (line) {
    case 2:
      for (int i = 0; i <= (5 - (int)(log10(s1 + 1) + 1)); i++) { // +1 zato, da ne premakne pri nicli
        dataW += " ";
      }
      dataW += s1;
      dataW += "       ";
      for (int i = 0; i <= (5 - (int)(log10(s2 + 1) + 1)); i++) {
        dataW += ' ';
      }
      dataW += s2;
      dataW += " ";
      break;

    case 3:
      for (int i = 0; i <= (11 - (int)(log10(s3 + 1) + 1)); i++) {
        dataW += " ";
      }
      dataW += s3;
      dataW += "        ";
      break;

    case 4:
      for (int i = 0; i <= (5 - (int)(log10(s4 + 1) + 1)); i++) {
        dataW += " ";
      }
      dataW += s4;
      dataW += "       ";
      for (int i = 0; i <= (5 - (int)(log10(s5 + 1) + 1)); i++) {
        dataW += ' ';
      }
      dataW += s5;
      dataW += " ";
      break;
  }

  // send to LCD
  lcd.print(dataW);
}

void printLine(int line, String data) {
  if (data.length() != 20) return;
  else {
    setLCDCursor((line - 1) * 20);
    lcd.print(data);
  }
}
