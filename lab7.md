# การทดลองที่ 7 การใช้LEDควบคุมการค้นหาไวไฟแอคเซสพอยต์

## วัตถุประสงค์
1. เพื่อเข้าใจเกี่ยวกับโครงสร้างของ code program และปรับแต่ง
3. เพื่อคิดค้นการทำงานที่จะใช้ผ่าน microcontroller รูปแบบใหม่
4. เพื่อทำการประยุกต์ใช้ความรู้ที่ศึกษาจากข้อมูลที่ได้ศึกษาจากทุกแลป

## อุปกรณ์ที่ใช้
1. CPU
2. adapter
3. หลอด LED เปล่งแสง
4. microcontroller ESP-01
5. อุปกรณ์เชื่อมต่อ USB เข้าไปยัง serial

6. wifi ที่ต้องการเชื่อมกับ microcontroller

## แหล่งข้อมูลเพื่อการศึกษา
- หาที่อยู่ IP adress 
  1. กดค้นหาตรง Windows ว่า Control Pane
   ![Screenshot 2021-04-01 001820](https://user-images.githubusercontent.com/81258597/113185894-34405580-9281-11eb-9ba6-ca1f98c352af.png)
  2. ในหน้าต่างของ Control Panel, คลิกที่ Network and Internet
   ![Screenshot 2021-04-01 002010](https://user-images.githubusercontent.com/81258597/113185925-3b676380-9281-11eb-92bb-d3428ea38327.png)

  3. ในหน้าต่างของ Network and Sharing Center, ในส่วนของ View your active networks, ที่อยู่กับ Connections, ให้คลิกที่ลิงค์ของการเชื่อมต่อเครือข่าย
    ![Screenshot 2021-04-01 002135](https://user-images.githubusercontent.com/81258597/113185943-402c1780-9281-11eb-9280-d69f48757080.png)

  4. ในหน้าต่างของ Network Connection Status, คลิกที่ปุ่ม Details
    ![Screenshot 2021-04-01 002225](https://user-images.githubusercontent.com/81258597/113185960-44583500-9281-11eb-9b62-2197d36e1da7.png)

  5.ในหน้าต่างของ Network Connection Details, ต่อจาก IPv4 IP Address จะมี IP address แสดงขึ้นมา.
    ![Screenshot 2021-04-01 002341](https://user-images.githubusercontent.com/81258597/113185976-491ce900-9281-11eb-9bea-5c4a84da1651.png)




- คลิปวิดิโอจากแลปต่างๆ
  - แลป 1 https://www.youtube.com/watch?v=NLIUsWLEpmg
  - แลป 2 https://www.youtube.com/watch?v=yBjab0UNuB8
  - แลป 3 https://www.youtube.com/watch?v=CCnN1WJsXQY และ https://www.youtube.com/watch?v=6JnhaUILGuw
  - แลป 4 https://www.youtube.com/watch?v=nFqoZT26U5k
  - แลป 5 https://www.youtube.com/watch?v=VX-QNQcO-b4
  - แลป 6 https://www.youtube.com/watch?v=T26DVHePlTs
- source code จากอาจารย์
  - https://github.com/choompol-boonmee/lab63b/tree/master/examples

## วิธีการทำการทดลอง
1. เขียนโปรแกรมบน microcontroller โดยทำการเสียบ microcontroller เข้าทาง serial port ของ USB 

(จากซ้ายไปขวา microcontroller ESP-01, อุปกรณ์เชื่อมต่อ USB และ การเสียบเข้าทาง serial port ตามลำดับ)

![image](https://user-images.githubusercontent.com/80879966/112019858-6dcadf80-8b62-11eb-8370-cc9b002280f5.jpg)

2. ดูที่ตัวอย่างโปรแกรม ที่โฟลเดอร์ pattani
- พิมพ์ cd pattani เพื่อไปยังโฟลเดอร์
- แสดงโฟลเดอร์ ซึ่งมีโปรแกรมตัวอย่าง 9 โปรแกรม
  - ไปที่ตัวอย่างที่ 7
    - พิมพ์ cd 07_Sensor-wifi
    - พิมพ์ vi src/main.cpp

3. ดู source code program 
- พิมพ์ vi src/main.cpp
- โดยข้อมูลเบื้องต้นของ code มีความหมายดังนี้
  - const char* ssid หมายถึง ชื่อ wifi
  - const char* password หมายถึง รหัสผ่าน wifi
  - IPAddress local_ip หมายถึง IP address หรือ Network address
  - IPAddress gateway หมายถึง Default gateway
  - IPAddress subnet หมายถึง Subnet Mask
 - ส่วนของ set up
   - Serial.beginset up หมายถึง การ set up serial port ที่ความเร็ว 115200
   - set up ให้ io port มี 2 อัน
      - pinMode บรรทัดแรก หมายถึง port 0 (สายสีขาว) เป็น input
      - pinMode บรรทัดถัดมา หมายถึง port 2 (สายสีเหลือง) เป็น output
   - WiFi.softAP หมายถึง การรันคำสั่ง softAP และกำหนด ssiad, password
   - delay(1000) หมายถึง ช่วงเวลา 1000 ms หรือ 1 วินาที
 - ส่วนของ loop
   - digitalRead() หมายถึง การอ่านข้อมูลจาก port ซึ่งในตัวอย่างหมายถึง port 0 (ข้อมูลที่อ่านได้เป็นข้อมูล digital ซึ่งมีแค่ค่า 0 หรือ 1)
      - หากค่าที่อ่านได้ เป็น 1 ให้ HIGH ไปที่ port 2 (หากเป็นหลอดไฟหมายถึงไฟติด)
      - หากค่าที่อ่านได้ เป็น 0 ให้ LOW ไปที่ port 2 (หากเป็นหลอดไฟหมายถึงไฟดับ)
    โดยในที่นี้หมายความว่า port 0 เป็นตัวควบคุมสัญญาณ input ให้ไฟเปิด หรือ ปิด
   - delay(2000) หมายถึง ช่วงเวลา 2000 ms หรือ 2 วินาที
    
```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "EELAB-0058";
const char* password = "vichamaporn";

IPAddress local_ip(172, 20, 10, 14);
IPAddress gateway(172, 20, 10, 1);
IPAddress subnet(255, 255, 255, 244);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);
	
	pinMode(0, INPUT);
 	pinMode(2, OUTPUT);
 	Serial.println("\n\n\n");
	
		WiFi.softAP(ssid, password);
		WiFi.softAPConfig(local_ip, gateway, subnet);
		delay(1000);

		server.onNotFound([]() {
			server.send(404, "text/plain", "ERROR! Path Not Found");
	});

		server.on("/", []() {
			cnt++;
			String msg = "Hello cnt: ";
			msg += cnt;
			server.send(200, "text/plain", msg);
	
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
int val = digitalRead(1);
 Serial.printf("======= read %d\n", val);
 if(val==1) {
  digitalWrite(2, HIGH);
 } else {
  digitalWrite(2, LOW);
 }
 delay(2000);
 server.handleClient();
}
```

4.เข้าไปที่ configuration file ใน program
- พิมพ์ vi platformio.ini เพื่อแสดงข้อมูล
  - platform แสดงถึง เทคโนโลยีของบริษัทผู้ผลิต
  - board แสดงถึง ชื่อบอร์ด
  - framwork แสดงถึง วิธีการเขียนโปรแกรม
  - upload_port แสดงถึง portที่ใช้ติดต่อ 
    - ในกรณีนี้เป็น windows จึงแสดงว่า COM3 (หรือCOM4)

```javascript
; IOT for KIDS
;
; By Dr.Choompol Boonmee
; 

[env:exercise01]
platform = espressif8266
board = esp01_1m
framework = arduino
board_build.flash_mode = dout

upload_port = /dev/cu.usbserial-1420
;upload_port = COM3

monitor_port = /dev/cu.usbserial-1420
;monitor_port = COM3
monitor_speed = 115200
```

5.อัพโหลดโปรแกรม 07_Sensor-wifi เข้าไปยัง microcontroller โดยใช้คำสั่ง upload
- พิมพ์ pio run -t upload
- ในขณะที่ program กำลังรันข้อมูล เพื่อให้ microcontroller รับโปรแกรมใหม่เข้าไป
  - กดปุ่มสีดำ เพื่อทำให้เกิดการ load 
  - กดปุ่มสีแดง เพื่อให้เกิดการ reset
 - ทำการกดปุ่มสีดำ เพื่อให้ทำการอัพโหลดได้
 - เมื่อโปรแกรมถูกอัพโหลดเสร็จสิ้น โปรแกรมจะทำงานโดยการตรวจสอบที่ port 0 ว่ามี input มาหรือไม่
    - ถ้า input เป็น 1 ไฟจะติดที่ port 2
    - ถ้า input เป็น 0 ไฟจะไม่ติด
- โดยเราจะสามารถสังเกตผลลัพธ์ที่แสดงผลผ่านคอมพิวเตอร์
  - พิมพ์ pio device monitor
   - กดปุ่มสีแดง เพื่อทำการ reset โปรแกรม  โดยโปรแกรมจะหยุดทำงานและเริ่มนับ 1 ใหม่
 
6. ใช้โทรศัพท์มือถือค้นหา wifi ในกรณีที่ input เป็น 1
   - สังเกต wifi ที่เกิดขึ้นจากการสร้าง

## การบันทึกผลการทดลอง 
  จากการทดลองได้ลองทำการปรับเปลี่ยนcode programจากเดิม โดยข้อมูลที่เปลี่ยนโดยสำคัญคือชื่อและรหัสผ่านของwifi ที่อยู่ไอพี,gateway รวมไปถึง subnet ให้เป็นที่อยู่ปัจจุบันที่อยู่ โดยทำการค้นหาในcontrol panel ซึ่งจริงๆแล้วสามารถทำได้อีกวิธีผ่านการเข้า comman prompt และพิมพ์ว่า IPCONFIG ข้อมูลที่ต้องการก็จะขึ้นมาเช่นกัน ซึ่งโครงสร้างเกี่ยวกับโค้ดในการสร้างwifi จากแลป5,6 นอกจากนี้ยังได้ทำการแทรกหลอด LED เปล่งแสงผ่านการเชื่อมต่อ port ต่างๆที่ได้นำความรู้จากแลปที 4 จากนั้นจึงทำการเปลี่ยนinputจากเดิมในแลปที่อาจารย์ยกตัวอย่างให้เป็น 1 ไฟจึงติด และเมื่อไฟติด เราจึงทำการค้นหาสัญญาณwifiได้ จึงเป็นสาเหตุที่ทำให้เกิดการประยุกต์การทดลองนี้ขึ้นมาและทดสอบผ่านการพิมพ์ pio run -t upload เพื่ออัพโหลดโปรแกรม 07_Sensor-wifi เข้าไปยัง microcontroller จากนั้นอาศัยคำสั่ง pio device monitor ที่ใช้ดูผลลัพธ์ของโปรแกรมที่ถูกอัพโหลดเข้าไป 
ซึ่งในที่นี้จะแสดงผลออกมาว่า ver started เพื่อตรวจสอบการแสดงผลของ wifi โดยทดลองค้นหาด้วยโทรศัพท์มือถือ ทำให้พบกับ wifi ที่เราทำการสร้างขึ้นมา

## อภิปรายผลการทดลอง 
  เนื่องจากการทดลองนี้อาศัยการประยุกต์และรวบรวมข้อมูลผ่านความรู้ที่ได้ทำการศึกษาจากทุกแลป และด้วยสถานการณ์การแพร่ระบาดของโรค COVID-19 ทำให้ไม่สามารถทำการทดสอบโค้ดของโปรแกรมที่เขียนขึ้นมาใหม่ได้ว่ามีความถูกต้องหรือไม่ แต่ด้วยความรู้ที่มีจึงนำมาประยุกต์ออกมาได้เป็นการทดลองที่ 7 ดังที่ได้เขียนและกล่าวไปในข้างต้น 

## คำถามหลังการทดลอง 


