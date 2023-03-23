# COCKTAIL_MAKER_MACHINE_PROJECT
The purpose of our project is to build an automatic cocktail machine that can produce correct drinks according to the voice command from users. When the machine is available, the LCD screen displays “ready for drink”. At this stage, users can choose which drink to be made by talking to google assistant on the phone, then the machine will start making the drink with the LCD screen displaying the drink information. When the drink is done, the LCD screen goes back to “ready for drink”. The overview of working flow is shown as below:

![image](https://user-images.githubusercontent.com/113930091/227333192-e5442c3f-2ff4-4ed2-ae77-ab86264192bd.png)

# List of Main Components
[Adafruit ESP32 Feather Board](https://www.adafruit.com/product/3405)

[RP2040 MCU](https://www.adafruit.com/product/4864?gclid=Cj0KCQiA_P6dBhD1ARIsAAGI7HDB3bS81QrAcRroHiH9zoDsJJwLUQOmEJi_CEaD2tX1yZLWML5zlCQaAgJTEALw_wcB)

[Peristaltic Pumps](https://www.amazon.com/gp/product/B09MVPJXFJ/ref=ppx_yo_dt_b_asin_image_o02_s00?ie=UTF8&psc=1)

[16x2 LCD screen with driver board](https://www.temu.com/subject/n9/googleshopping-landingpage-a-psurl.html?goods_id=601099512683692&_bg_fs=1&_p_rfs=1&_x_ads_channel=google&_x_ads_sub_channel=shopping&_x_login_type=Google&_x_vst_scene=adg&sku_id=17592192006895&_x_ns_sku_id=17592192006895&_x_ads_account=8195172345&_x_ads_set=18597826067&_x_ads_id=150873099588&_x_ads_creative_id=628103286637&_x_ns_source=g&_x_ns_gclid=Cj0KCQiA_P6dBhD1ARIsAAGI7HC5biIXgm3DMiXlrKp_uj10SjIomxO_WPg4CDtjRdtwkYZscLI5kDUaAniWEALw_wcB&_x_ns_placement=&_x_ns_match_type=&_x_ns_ad_position=&_x_ns_product_id=17592192006895&_x_ns_wbraid=CjkKCQiA2fmdBhCBARIoALC0hcNKjNKtrJZ4rDA9r775iTywh6AaxBb-pN9ba99Or7dYH3PN1xoCx4M&_x_ns_gbraid=0AAAAAo4mICFKom0ZUUtsSi9_EVc3EqEB1&gclid=Cj0KCQiA_P6dBhD1ARIsAAGI7HC5biIXgm3DMiXlrKp_uj10SjIomxO_WPg4CDtjRdtwkYZscLI5kDUaAniWEALw_wcB)

# Project Instructions
Our project is composed of two microcontrollers: RP2040 and ESP32 Feather, three electric pumps, one LCD screen and a 3D printed model. The diagram of the overall operation is shown here. ESP32 receives the voice command from users and send their choice to RP2040 through GPIO. To realize this functionality, the ThingSpeak IoT platform and IFTTT service are used. IFTTT is for receiving the voice command and post the information on the IoT server, then ESP32 will read the drink selection from the server.

ESP32 is responsible for reading the data on ThingSpeak, sending the information to RP2040 and modifying the content displaying on the LCD screen. Micropython is used to realize the functionality. For the ESP32 to access IoT server, it needs to connect to WiFi. Once it’s connected to WiFi, it reads the data on ThingSpeak channel and send out the information by enabling and setting the corresponding GPIO pin. Besides, the content of LCD screen is also changed accordingly. 

RP2040 receives the information form ESP32 via GPIO. The operation of RP2040 is written in C. There is a state machine constructed in the code, and RP2040 will enter different states based on the information it receives and actions it has already taken. As shown in the code, there are 3 states: IDLE, DRAW, END. When the drink is decided, the three pumps will be activated via GPIO in order, for a specific time set by the code to make the correct drink. 

The assembly details are stated here. Firstly, the I2C pins of ESP32 are connected to the I2C pins of LCD screen. Three GPIO pins of ESP32 are directly connected three GPIO pins of RP2040 for sending the decision of drink. The power wires of pumps are directly connected to the 12V power supply, and the ground wires are connected to the emitter of BJT. The collector of BJT is connected to ground, and the base is connected to the corresponding GPIO pin of RP2040 that is set when a specific pump is expected to operate.

The illustration of the connection between main components is shown here. The component on the left side is the driver of LCD screen, the board on top is ESP32, and the board at the bottom is Raspberry Pi Pico. As we can see, LCD screen with drive attached is connected to the ESP32 board via I2C, and ESP32 communicates with RP2040 via 3 GPIO pins.

![image](https://user-images.githubusercontent.com/113930091/227342422-55a62ac8-0ede-49bf-937a-becf07d61d9d.png)

# Mechanical Part
For the mechanical part, we used three pumps. The schematic is shown as below:
![image](https://user-images.githubusercontent.com/113930091/227343495-1fabb600-c2ba-48e9-b447-c1b9846fd085.png)


