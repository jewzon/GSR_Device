# GSR_Device
Galvanic Skin Response Device, a tool used for finding the conduciveness of skin due to sweat gland activity. it's main purpose is "providing insights into emotional and physiological states".

I have designed a simple circuit which can detect and display sweat gland activity efficiently using voltage divider circuit.

I'll be providing step by step instructions to create the device. 
Here’s the final result:
![Image](https://github.com/user-attachments/assets/0de31eb3-38a2-461a-be4c-d317bc265132)

 you can check the components used for the project in the [BoM.csv](BoM.csv) (mentioned under "Value")
 -



Step 1: Designing Circuit
=
the fundamental circuit is Based on the voltage divider method of aquiring the eletrical Conductivity of the skin surface( do mind the conductivity is based on sweat on  skin).. Voltage divider produces a weak signal, to amplify it I used an OP-AMP LM358the output is given to  ATTiny 85 microcontroller , OLED display to show the results which works on Two-Wire Interface aka I2C protocol communicated by ATTiny 85.

For power source i used a 9v battery with is regulated by LM7805 which gives a constant 5 volt with DPDT switch to turn it on/off.

Step 2: PCB Design 
=
I used KiCad to design as you can see it below
Schematic:
-
<img width="1636" height="1152" alt="image" src="https://github.com/user-attachments/assets/cdc331e7-5d19-4f16-8bef-26de124fd656" />

PCB:
-
<img width="1062" height="898" alt="image" src="https://github.com/user-attachments/assets/80ff7c74-0903-4054-b8ea-bcb3e3e8dfea" />

<img width="1540" height="1038" alt="image" src="https://github.com/user-attachments/assets/f3329fdc-a975-4fd8-ba37-da45af60eb18" />

You might wonder why I made the vias so big and distancing the components with each other, it's because I used a CNC based Engraving Machine to Fabricated it, which can only produce a effective route/tracks at .7 mm.

Step 3: Fabrication
=
I used a CNC engraveer & driller to Fabricate the PCB. (if anyone needs to know how in-depth, let me know )

Step 4: Programming 
=
 ATTiny 85 can be programmed using ISP (In-System Programmer) via Arduino UNO as a programming interface.
 here is a youtube video tutorial for how to program ATTiny 85:
 
 [here](https://youtu.be/KbRW1ERetPU?si=yeuM4bQQ7WmSiEv9)

 this is the code I uploaded in ATTiny 85
```C++
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_ADDR 0x3C   // Most common I2C OLED address

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

#define GSR A0       // GSR sensor pin

void setup() {
  Serial.begin(9600);
  Wire.begin();      // SDA/SCL default pins

  // Initialize OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, OLED_ADDR)) {
    Serial.println("OLED not found");
    while (1);
  }

  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println("GSR Ready");
  display.display();
  delay(1000);
}

void loop() {
  long sum = 0;
  int readings = 0;
  unsigned long startTime = millis();

  // Collect readings for 0.5 seconds
  while (millis() - startTime < 500) {
    sum += analogRead(GSR);
    readings++;
    delay(5);
  }

  int gsr_avg = sum / readings;

  // Serial output
  Serial.print("Avg GSR: ");
  Serial.println(gsr_avg);

  // OLED output
  display.clearDisplay();
  display.setTextSize(2);
  display.setCursor(0, 0);
  display.print("GSR:");
  display.setCursor(0, 30);
  display.print(gsr_avg);
  display.display();
}

```
Step 5  : Soldering 
=
I believe this part is easy.

 




