✅ 🔷 Project Title:
"GSM-Based Wireless Notice Board using Arduino UNO"

✅ 📄 Project Description:
This project is a wireless notice board system using an Arduino UNO, GSM module, and 16x2 LCD display. The system receives SMS messages via the GSM module and displays them on the LCD. It acts like a digital notice board which can be updated from anywhere just by sending an SMS.
This is highly useful in:
Schools or colleges for displaying updates
Offices or factories for alerts
Public info systems without Wi-Fi

✅ 🔧 Key Features:
Feature	Description
📲 GSM SMS Reading	Reads SMS sent to SIM card
📟 16x2 LCD Display	Shows messages to users
🧠 Arduino UNO	Main controller to process logic
💡 LED Indicator	Shows system activity
🔢 SMS Format Parsing	Only messages between # and * are displayed
🔌 No Internet Required	Works purely on GSM SMS

✅ 🧠 How it Works (Flow):
sql
Copy
Edit
[User Sends SMS] → GSM Module → Arduino UNO → Extracts Message → LCD Displays it

✅ 🧾 Components Used:
Component	Quantity	Purpose
Arduino UNO	1	Main brain
GSM Module (SIM800L/SIM900A)	1	Receives SMS
SIM Card	1	Receives the message
16x2 LCD Display	1	Shows messages
Potentiometer	1	LCD contrast control
Power Supply (5V 2A)	1	GSM Module Power
Jumper Wires + Breadboard	--	Connections
LED	1	Status Indication

✅ 🧾 Code Explanation (Line-by-Line)
cpp
Copy
Edit
#include <LiquidCrystal.h>
LiquidCrystal lcd(7, 6, 5, 4, 3, 2);
Includes LCD library

Defines which Arduino pins control the LCD

cpp
Copy
Edit
int led = 13;
LED connected to pin 13 for indication

cpp
Copy
Edit
int temp = 0, i = 0, x = 0, k = 0;
char str[100], msg[32];
str[]: stores raw SMS

msg[]: stores filtered message between # and *

🔁 setup() function:
cpp
Copy
Edit
lcd.begin(16, 2);
Serial.begin(9600);
pinMode(led, OUTPUT);
digitalWrite(led, HIGH);
Initializes LCD & Serial

Turns ON the LED

cpp
Copy
Edit
lcd.print("GSM Initilizing...");
gsm_init();
Shows "GSM Initializing"

Calls GSM setup function

cpp
Copy
Edit
lcd.setCursor(0, 0);
lcd.print("Wireless Notice");
lcd.setCursor(0, 1);
lcd.print("    Board      ");
delay(2000);
Prints welcome message on LCD

cpp
Copy
Edit
Serial.println("AT+CNMI=2,2,0,0,0");
delay(500);
Serial.println("AT+CMGF=1");
Sets GSM to Text Mode (CMGF=1)

Enables SMS live notifications (CNMI)

🔁 loop() function:
cpp
Copy
Edit
for (unsigned int t = 0; t < 60000; t++) {
  serialEvent();   // Check for incoming SMS
  if (temp == 1) {
    x = 0, k = 0, temp = 0;
    ...
    lcd.clear();
    lcd.print(msg); // Show message on LCD
    delay(1000);
  }
}
lcd.scrollDisplayLeft();  // Scroll message left
📥 serialEvent() – Handles incoming SMS:
cpp
Copy
Edit
while (Serial.available()) {
  char ch = (char)Serial.read();  // Read serial char
  str[i++] = ch;                  // Store in buffer
  if (ch == '*') {               // End of message
    temp = 1;
    lcd.clear();
    lcd.print("Message Received");
  }
}
📶 gsm_init() – Initializes GSM Module:
cpp
Copy
Edit
while (at_flag) {
  Serial.println("AT"); // Check if module replies
  ...
}

while (echo_flag) {
  Serial.println("ATE0"); // Turn off command echo
  ...
}

while (net_flag) {
  Serial.println("AT+CPIN?");
  ...
}
Checks for GSM connection

Turns off echo (ATE0)

Waits until SIM is "READY"

✅ ✅ Example SMS:
bash
Copy
Edit
#Hello Rohan*
This message will be shown on LCD as:

nginx
Copy
Edit
Hello Rohan

✅ 💡 Real-World Applications:
Wireless Notice Boards (schools, offices)
Emergency Alert Display System
Factory Info System (low signal areas)
Smart village information kiosk

✅ 📌 Summary:
Feature	Status
Works on Arduino UNO	✅ Yes
No Internet needed	✅ Yes
SMS based control	✅ Yes
Display via LCD	✅ Yes
Clean message parser	✅ Yes
