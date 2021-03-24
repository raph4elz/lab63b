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
  - ไปที่ตัวอย่างที่ 6
    - พิมพ์ cd 06_Wifi-AP-Web-Server
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

![image](https://user-images.githubusercontent.com/80879678/112093962-ea92a380-8bcc-11eb-8b5d-9c367a3b9449.jpg)

- กำหนด IPAdress, gateway, subnet

![image](https://user-images.githubusercontent.com/80879678/112094552-13676880-8bce-11eb-840f-558fcd636b35.jpg)

- เตรียม web server

![image](https://user-images.githubusercontent.com/80879678/112094082-32192f80-8bcd-11eb-9905-b37362433e7a.jpg)

- รันคำสั่ง softAP แล้วกำหนด ssiad กับ password

![image](https://user-images.githubusercontent.com/80879678/112094155-537a1b80-8bcd-11eb-9f3f-7429cc1003ca.jpg)

4. อัปโหลดโปรแกรม 06_Wifi-AP-Web-Server เข้าไปยัง microcontroller โดยใช้คำสั่ง upload
  - พิมพ์ pio run -t upload

![image](https://user-images.githubusercontent.com/80879678/112094212-6ee52680-8bcd-11eb-963f-b78ec3414b97.jpg)

  - ในขณะที่ program กำลังรันข้อมูล เพื่อให้ microcontroller รับโปรแกรมใหม่เข้าไป
    - กดปุ่มสีแดง เพื่อให้เกิดการ reset
 
![image](https://user-images.githubusercontent.com/80879678/112094256-88866e00-8bcd-11eb-8bab-dc64b6cff485.jpg)

5. ตรวจสอบว่า wifi ที่สร้างขึ้นนั้นแสดงผลหรือยัง 
    - พิมพ์ pio device monitor

![image](https://user-images.githubusercontent.com/80879678/112094298-9cca6b00-8bcd-11eb-84ab-2dc865285aca.jpg)

6. ใช้โทรศัพท์โทรศัพท์มือถือค้นหา wifi

## การบันทึกผลการทดลอง 
   คำสั่ง pio device monitor ใช้ในการดูผลลัพธ์ของการรันโปรแกรมใน microcontroller ซึ่งแดสงผลออกมาว่า ver started และ ตรวจสอบการแสดงผลของ wifi โดยทดลองค้นหาด้วยโทรศัพท์มือถือ ทำให้พบกับ wifi 

## อภิปรายผลการทดลอง (พร้อมตัวอย่าง)
   จากการทดลองดังกล่าว โปรแกรม 06_Wifi-AP-Web-Server ทำให้เราสามารถสร้าง Wifi Access Point ขึ้นมาเองได้ และตรวจสอบพบว่ามีอยู่จริง ดังภาพ
   
![image](https://user-images.githubusercontent.com/80879678/112094339-ad7ae100-8bcd-11eb-98da-a0eb09f63ef6.jpg)

## คำถามหลังการทดลอง 
หากทำตามขั้นตอนดังกล่าว จะสามารถสร้าง Wifi Access Point ได้เอง
- [x] ใช่
- [ ] ไม่ใช่
