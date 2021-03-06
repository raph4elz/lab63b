# การทดลองที่ 6 การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

## วัตถุประสงค์ 
1. เพื่อเข้าใจการเขียนโปรแกรมที่จะสร้าง Wifi Access Point ขึ้นมาเอง 
2. เพื่อสามารถนำโปรแกรมไปใช้ในการเชื่อม wifi ของตัวเองกับอุปกรณ์อื่น

## อุปกรณ์ที่ใช้ 
1. CPU
2. microcontroller ESP-01
3. อุปกรณ์เชื่อมต่อ USB เข้าไปยัง serial

## แหล่งข้อมูลเพื่อการศึกษา
https://www.youtube.com/watch?v=T26DVHePlTs

## วิธีการทำการทดลอง 
1. ทำการเสียบ microcontroller เข้าทาง serial port ของ USB 
2. ดูที่ตัวอย่างโปรแกรม ที่โฟลเดอร์ pattani
- พิมพ์ cd pattani เพื่อไปยังโฟลเดอร์
- แสดงโฟลเดอร์ ซึ่งมีโปรแกรมตัวอย่าง 9 โปรแกรม
  - ไปที่ตัวอย่างที่ X
    - พิมพ์ cd 0X_Wifi-AP-Web-Server
3. ดู source code program 
- พิมพ์ vi src/main.cpp

```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "MY-ESP8266";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
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
  server.handleClient();
}
```
โดยโค้ดดังกล่าว ประกอบด้วยข้อมูลดังนี้

- กำหนด ชื่อ และ ตั้งpassword

![wee](https://user-images.githubusercontent.com/81258597/112364328-f8e5da00-8d08-11eb-92a3-ec566fde447e.png)


- กำหนด IPAdress, gateway, subnet

![eee](https://user-images.githubusercontent.com/81258597/112364388-09965000-8d09-11eb-9069-4c8761da4de0.png)


- เตรียม web server

![wed](https://user-images.githubusercontent.com/81258597/112364444-19159900-8d09-11eb-9585-ed6f7249e345.png)


- รันคำสั่ง softAP แล้วกำหนด ssiad กับ password

![rq](https://user-images.githubusercontent.com/81258597/112364613-4cf0be80-8d09-11eb-895e-26c7e80ac2b8.png)


4. อัปโหลดโปรแกรม 06_Wifi-AP-Web-Server เข้าไปยัง microcontroller โดยใช้คำสั่ง upload
  - พิมพ์ pio run -t upload

![pio ](https://user-images.githubusercontent.com/81258597/112364711-62fe7f00-8d09-11eb-9076-74b1eac9e4e9.png)


  - ในขณะที่ program กำลังรันข้อมูล เพื่อให้ microcontroller รับโปรแกรมใหม่เข้าไป
    - กดปุ่มสีแดง เพื่อให้เกิดการ reset
 
![pass red](https://user-images.githubusercontent.com/81258597/112364781-790c3f80-8d09-11eb-8138-c73b7a615c0e.png)


5. ตรวจสอบว่า wifi ที่สร้างขึ้นนั้นแสดงผลหรือยัง 
    - พิมพ์ pio device monitor


6. ใช้โทรศัพท์โทรศัพท์มือถือค้นหา wifi

## การบันทึกผลการทดลอง 
   คำสั่ง pio device monitor ใช้ในการดูผลลัพธ์ของการรันโปรแกรมใน microcontroller ซึ่งแดสงผลออกมาว่า ver started และ ตรวจสอบการแสดงผลของ wifi โดยทดลองค้นหาด้วยโทรศัพท์มือถือ ทำให้พบกับ wifi 

## อภิปรายผลการทดลอง (พร้อมตัวอย่าง)
   จากการทดลองดังกล่าว โปรแกรม 06_Wifi-AP-Web-Server ทำให้เราสามารถสร้าง Wifi Access Point ขึ้นมาเองได้ และตรวจสอบพบว่ามีอยู่จริง ดังภาพ
   
![1231](https://user-images.githubusercontent.com/81258597/112365065-ca1c3380-8d09-11eb-85c2-c30da5767f9a.png)


## คำถามหลังการทดลอง 
เราสามารถรหัส wifi ได้ที่ไหน
* ตอบ const char* password = '******';
