# การทดลองที่ 2 การเขียนโปรแกรมค้นหาไวไฟ

## วัตถุประสงค์ 
1. เพื่อเข้าใจวิธีการเขียนโปรแกรมค้นหา wifi
2. เพื่อรันโปรแกรมบน microcontroller ในการค้นหา wifi

## อุปกรณ์ที่ใช้ 
1. CPU
2. เสาอากาศสำหรับรับ wifi
3. อุปกรณ์เชื่อมต่อ USB เข้าไปยัง serial
4. microcontroller ESP-01 ที่มี wifi ในตัวเอง

## แหล่งข้อมูลเพื่อการศึกษา
https://www.youtube.com/watch?v=yBjab0UNuB8

## วิธีการทำการทดลอง 
1. ทำการเสียบ microcontroller เข้าทาง serial port ของ USB 
2. ดูที่ตัวอย่างโปรแกรม ที่โฟลเดอร์ pattani
- พิมพ์ cd pattani เพื่อไปยังโฟลเดอร์
- แสดงโฟลเดอร์ ซึ่งมีโปรแกรมตัวอย่าง 9 โปรแกรม
  - ไปที่ตัวอย่างที่ 2
    - พิมพ์ cd 02_Scan-Wifi
3. ดู source code program 
- พิมพ์ vi src/main.cpp

```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	WiFi.mode(WIFI_STA);
	WiFi.disconnect();
	delay(100);
	Serial.println("\n\n\n");
}

void loop()
{
	Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
	int n = WiFi.scanNetworks();
	if(n == 0) {
		Serial.println("NO NETWORK FOUND");
	} else {
		for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.println(")");
			delay(10);
		}
	}
	Serial.println("\n\n");
}
```

4. อัพโหลดโปรแกรม 02_Scan-Wifi เข้าไปยัง microcontroller โดยใช้คำสั่ง upload
   - พิมพ์ pio run -t upload

![AQ](https://user-images.githubusercontent.com/81258597/112329061-6f72df80-8ce9-11eb-93e0-b354495d4d56.png)


   - ในขณะที่ program กำลังรันข้อมูล เพื่อให้ microcontroller รับโปรแกรมใหม่เข้าไป
     - กดปุ่มสีดำ เพื่อทำให้เกิดการ load 
     - กดปุ่มสีแดง เพื่อให้เกิดการ reset

![พๆพๆพ](https://user-images.githubusercontent.com/81258597/112376999-c5f71280-8d17-11eb-8d68-1d09c9fa05a0.png)

   - สังเกตผลลัพธ์ที่แสดงผลผ่านคอมพิวเตอร์
     - พิมพ์ pio device monitor
  
![image](https://user-images.githubusercontent.com/80879678/112092640-8242c280-8bca-11eb-8907-0a1be8c000f2.jpg)
 
## การบันทึกผลการทดลอง 
   หลังจากหน้าจอแสดงผลลัพธ์ การวนลูปจะเริ่มต้นขึ้น และเริ่มการค้นหา wifi ภายในบริเวณดังกล่าว และแสดงผล ดังภาพ 
   
![image](https://user-images.githubusercontent.com/80879678/112092782-ca61e500-8bca-11eb-94f7-4198a18f1636.jpg)

## อภิปรายผลการทดลอง
   pio run -t upload นั้นใช้ในการอัพโหลดข้อมูลจาก 02_Scan-Wifi ไปยัง microcontroller โดยคำสั่ง pio device monitor ที่ใช้ดูผลลัพธ์ของโปรแกรมที่ถูกอัพโหลดเข้าไป 
   ส่งผลทำให้เราสามารถสังเกตถึงชื่อและจำนวนของ wifi ที่ถูกพบภายในบริเวณดังกล่าวได้

## คำถามหลังการทดลอง 
ปัจจัยในข้อใดที่ส่งผลต่อการค้นหาwifi
* ตอบ ความเร็ว สัญญาณ

