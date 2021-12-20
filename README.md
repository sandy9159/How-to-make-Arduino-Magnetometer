# How-to-make-Arduino-Magnetometer

![image](https://user-images.githubusercontent.com/19898602/146794559-df185843-556a-4d3f-8c68-36820672406a.png)

Humans can't detect magnetic fields, but we use devices that rely on magnets all the time. Motors, compasses, rotation sensors, and wind turbines, for example, all require magnets for operation. This tutorial describes how to build an Arduino based magnetometer that senses magnetic field using three Hall effect sensors. The magnetic field vector at a location is displayed on a small screen using isometric projection.


An Arduino is a small open-source user-friendly microcontroller. It has digital input and output pins. It also has analog input pins, which are useful for reading input from sensors. Different Arduino models are available. This tutorial describes how to use either the Arduino Uno or the Arduino MKR1010. However other models can be used too.

Before you begin this tutorial, download the Arduino development environment as well as any libraries needed for your particular model. The development environment is available at https://www.arduino.cc/en/main/software, and installation instructions are available at https://www.arduino.cc/en/main/software.


# What is a magnetic field?

Permanent magnets exert forces on other permanent magnets. Current carrying wires exert forces on other current carrying wires. Permanent magnets and current carrying wires exert forces on each other too. This force per unit test current is a magnetic field.

If we measure the volume of an object, we get a single scalar number. However, magnetism is described by a vector field, a more complicated quantity. First, it varies with position throughout all space. For example, the magnetic field one centimeter from a permanent magnet is likely to be larger than the magnetic field ten centimeters away.

Next, the magnetic field at each point in space is represented by a vector. The magnitude of the vector represents the strength of the magnetic field. The direction is perpendicular to both the direction of the force and the direction of the test current.

We can picture the magnetic field at a single location as an arrow. We can picture the magnetic field throughout space by an array of arrow at different locations, possibly of different sizes and pointing in different directions. A nice visualization is available at https://www.falstad.com/vector3dm/. The magnetometer we are building displays the magnetic field at the location of the sensors as an arrow on the display.


# What is a Hall effect sensor, and how does it work?

A Hall effect sensor is a small, inexpensive device that measures the strength of the magnetic field along a particular direction. It is made from a piece of semiconductor doped with excess charges. The output of some Hall effect sensors is an analog voltage. Other Hall effect sensors have an integrated comparator and produce a digital output. Other Hall effect sensors are integrated into larger instruments which measure flow rate, rotation speed, or other quantities.

The physics behind the Hall effect is summarized by the Lorentz force equation. This equation describes the force on a moving charge due to an external electric and magnetic field.


![image](https://user-images.githubusercontent.com/19898602/146794726-8cca38b6-e758-44aa-8566-7c8c6d4fc816.png)

The figure below illustrates the Hall effect. Suppose we want to measure the strength of the magnetic field in the direction of the blue arrow. As shown in the left part of the figure, we apply a current through a piece of semiconductor perpendicular to the direction of the field to be measured. Current is flow of charges, so a charge in the semiconductor moves with some velocity. This charge will feel a force due to the external field, as shown in the middle part of the figure. Charges will move due to the force and accumulate on the edges of the semiconductor. Charges build up until the force due to the accumulated charges balances the force due to the external magnetic field. We can measure the voltage across the semiconductor, as shown in the right part of the figure. The voltage measured is proportional to the strength of the magnetic field, and it is in the direction perpendicular to the current and the direction of the magnetic field.


![image](https://user-images.githubusercontent.com/19898602/146794766-4018f900-957c-4010-98e3-4601f0f83e6d.png)


# What is isometric projection?

At each point in space, the magnetic field is described by a three dimensional vector. However, our display screen is two dimensional. We can project the three dimensional vector into a two dimensional plane so that we can draw it on the screen. There are multiple ways to accomplish this such as isometric projection, orthographic projection, or oblique projection.

In isometric projection, the x, y, and z axes are 120 degrees apart, and they appear equally foreshortened. Additional information about isometric projection, as well as the formulas needed, can be found on Wikipedia's page on the topic.

Before moving fuurther I would like to tell you something about PCB

Yes PCB are the heart of the electronics based project usually we hesitate to try custom PCB and opt to homemade solutions

like breadboard or Zero PCB earlier I also was in the same boat, I hesitate to try custom PCB my belief was they are much expensive.

but then I came to know about [JLCPCB.com](https://jlcpcb.com/IAT) and I was totally surprised how low price PCB's are they offering 

there PCB quality is best in market, now I always go with PCB for my project and [JLCPCB.com](https://jlcpcb.com/IAT) is my trusted 

PCB manufacturer, you can also try there PCB service for more details you can visit their website [JLCPCB.com](https://jlcpcb.com/IAT)
![image](https://user-images.githubusercontent.com/19898602/134224512-bea8d1c8-9ebe-448d-bbba-0cbecb42d528.png)


![image](https://user-images.githubusercontent.com/19898602/130722577-c30b7b43-ea89-4847-9c6b-058f9fabeda3.png)![image](https://user-images.githubusercontent.com/19898602/130722585-b5268db1-5f17-428f-ba60-b823140f2a70.png)

# Wiring

![image](https://user-images.githubusercontent.com/19898602/146794865-9b1666d3-99e3-4f2d-9839-15777298230d.png)


Place the sensors at one end of the breadboard, and place the display and Arduino on the opposite end. Current in the wires in the Arduino and display generate magnetic fields, which we do not want the sensors to read. Additionally, we may want to put the sensors near permanent magnets, which could adversely impact the current in the wires of the display and sensor. For these reasons, we want the sensors far from the display and Arduino. Also for these reasons, this magnetometer should be kept away from very strong magnetic fields.

Place the sensors perpendicular to each other but as close to each other as possible. Gently bend the sensors to get them perpendicular. Each pin of each sensor must be in a separate row of the breadboard so it can be separately connected.

![image](https://user-images.githubusercontent.com/19898602/146794899-9435c1b9-ac0c-4d7c-b489-e4569e90b4ed.png)

The wiring is slightly different between the MKR1010 and the Uno for two reasons. First, the Arduino and display communicate by SPI. Different Arduino models have different dedicated pins for certain SPI lines. Second, analog inputs of the Uno can accept up to 5 V while analog inputs of the MKR1010 can accept only up to 3.3 V. The recommended supply voltage for the Hall effect sensors is 5 V. The sensor outputs are connected to Arduino analog inputs, and these can be as large as the supply voltages. For the Uno, use the recommended 5 V supply for the sensors. For the MKR1010, use 3.3 V so that the analog input of the Arduino never sees a voltage larger than it can handle.

Follow the diagrams and instructions below for the Arduino you are using.


# Wiring with the Arduino Uno

![image](https://user-images.githubusercontent.com/19898602/146794963-eb8f285c-76ec-430c-ad9f-95b4e8974d0f.png)


The display has 11 pins. Connect them to the Arduino Uno as follows. (NC means not connected.)

Vin →5V

3.3 →NC

Gnd →GND

SCK →13

SO → NC

SI →11

TCS →10

RST →9

D/C →8

CCS → NC

Lite → NC


Connect Vin of the sensors to 5V of the Arduino. Connect ground of the sensor to ground of the Arduino. Connect the output of the sensors to analog inputs A1, A2, and A3 of the Arduino.


Let's get the TFT display working. Fortunately, Adafruit has some user friendly libraries and an excellent tutorial to go along with them. These instructions closely follow the tutorial, https://learn.adafruit.com/adafruit-1-44-color-tft-with-micro-sd-socket/overview.

Open the Arduino development environment. Go to Tools → Manage Libraries. Install the Adafruit_GFX, Adafruit_ZeroDMA, and Adafruit_ST7735 libraries. Restart the Android development environment.

The graphicstest example is included with the libraries. Open it. File → Examples → Adafruit ST7735 and ST7789 Library → graphicstest. To select the 1.44" display comment out line 95 and uncomment line 98.

Original version:


````
94 // Use this initializer if using a 1.8" TFT screen:
95 tft.initR(INITR_BLACKTAB);     //Init ST7735S chip, black tab
96
97 //OR use this initializer (uncomment) if using a 1.44" TFT:
98 //tft.initR(INITR_144GREENTAB); // Init ST7735R chip, green tab<br>

````
````
94 // Use this initializer if using a 1.8" TFT screen:
95 //tft.initR(INIT_BLACKTAB);    //Init ST7735S chip, black tab
96
97 //OR use this initializer (uncomment) if using a 1.44" TFT:
98 tft.initR(INITR_144GREENTAB); //Init SST35R chip, green tab<br>


````

![image](https://user-images.githubusercontent.com/19898602/146795193-18c5241d-0cfa-4d86-b3fa-38c01bc8ed38.png)


The next step would be to calibrate the device.The sensor data sheet provides information on how to convert raw sensor voltage values to magnetic field strength. Calibration could be verified by comparing to a more accurate magnetometer.

Permanent magnets interact with current carrying wires. Wires near the display and in the Arduino generate magnetic fields which could affect sensor readings. Additionally, if this device is used to measure near a strong permanent magnet, the magnetic field from the device under test will interact with, introduce noise into, and possibly damage the Arduino and display. Shielding could make this magnetometer more robust. The Arduino can withstand larger magnetic fields if it is shielded in a metal box, and less noise will be introduced if shielded cables connect the sensors instead of bare wires.

Magnetic field is a function of position, so it is different at every point in space. This device uses three sensors, one to measure the x, the y, and the z component of the magnetic field at a point. The sensors are near to each other but not at a single point, and this limits the magnetometer resolution. It would be cool to save magnetic field readings at different points then display them as an array of arrows at the corresponding locations. However, that is a project for another day.


