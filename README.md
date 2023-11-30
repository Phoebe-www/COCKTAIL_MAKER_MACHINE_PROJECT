# COCKTAIL_MAKER_MACHINE_PROJECT
The purpose of our project is to build an automatic cocktail machine that can produce correct drinks according to the voice command from users. When the machine is available, the LCD screen displays “ready for drink”. At this stage, users can choose which drink to be made by talking to google assistant on the phone, then the machine will start making the drink with the LCD screen displaying the drink information. When the drink is done, the LCD screen goes back to “ready for drink”. The overview of working flow is shown as below:

![image](https://user-images.githubusercontent.com/113930091/227333192-e5442c3f-2ff4-4ed2-ae77-ab86264192bd.png)

## ESP32和LCD屏幕的I2C通信过程

硬件连接：

首先，确保您正确连接了ESP32与LCD屏幕，包括SDA（数据线）和SCL（时钟线）连接到相应的引脚。还需要连接电源和地线。
初始化I2C总线：

在ESP32上初始化I2C总线，以便与LCD屏幕进行通信。您需要指定I2C总线的速度（通常以Hz为单位）和SDA、SCL引脚的引脚号。
扫描I2C地址（可选）：

在开始通信之前，您可以扫描I2C总线上的设备地址，以确定LCD屏幕的I2C地址。大多数LCD屏幕将在数据手册中提供其默认地址。
建立通信：

使用ESP32的I2C库函数，通过I2C总线向LCD屏幕发送启动信号，并指定LCD屏幕的I2C地址。这将建立与LCD屏幕的通信。
发送数据和命令：

根据LCD屏幕的规格，您可以发送命令或数据以控制LCD的显示。命令通常用于初始化LCD，并设置显示模式、清除屏幕等。数据用于在LCD上显示文本、图像或其他内容。
接收响应：

根据需要，您可以接收LCD屏幕的响应或确认。一些LCD屏幕可能会返回状态或确认信息，以便您知道命令是否成功执行。
结束通信：

当您完成与LCD屏幕的通信时，请发送停止信号以结束I2C通信。这将释放总线，使其他设备可以进行通信。
关闭I2C总线：

最后，关闭ESP32上的I2C总线以释放资源。

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

![image](https://user-images.githubusercontent.com/113930091/227345971-10ded42e-fe74-4de6-a5f0-da4d8fe7abc5.png)


# Mechanical Part
For the mechanical part, we used three pumps. The schematic is shown as below:

![image](https://user-images.githubusercontent.com/113930091/227343495-1fabb600-c2ba-48e9-b447-c1b9846fd085.png)


We used 3D printing and laser cutting to build our packaging. We started from 3D modeling. The modeling image and final design are shown as below:


![image](https://user-images.githubusercontent.com/113930091/227345659-45705319-bb84-4421-9165-ef396a634a0c.png)


From the rare view and side view, we can see the three-pump design clearly.

![image](https://user-images.githubusercontent.com/113930091/227348886-e75d06f5-426a-4f87-b59b-9d1bff4aa595.png)








