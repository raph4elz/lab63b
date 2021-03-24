# การทดลองที่ 3 การเขียนโปรแกรมเอ้าพุทสัญญาณดิจิทัล

## วัตถุประสงค์ 
1. เพื่อศึกษาหน้าที่ของ Relay
2. เพื่อศึกษาและทำความเข้าใจ output ที่แสดงผลออกมา
3. เพื่อให้สามารถเขียนโปรแกรมและรัน microcontroller ซึ่งต่อกับ adapter

## อุปกรณ์ที่ใช้ 
1. Relay
2. adapter
3. หลอด LED เปล่งแสง
4. microcontroller ESP-01
5. อุปกรณ์เชื่อมต่อ USB เข้าไปยัง serial

## แหล่งข้อมูลเพื่อการศึกษา
- https://www.youtube.com/watch?v=CCnN1WJsXQY
- https://www.youtube.com/watch?v=6JnhaUILGuw

## วิธีการทำการทดลอง 
1. นำ adapter(ต่อกับLED) ต่อเข้ากับ USB 
2. ทำการเสียบ microcontroller เข้าทาง serial port ของ USB 
3. ดูที่ตัวอย่างโปรแกรม ที่โฟลเดอร์ pattani
- พิมพ์ cd pattani เพื่อไปยังโฟลเดอร์
- แสดงโฟลเดอร์ ซึ่งมีโปรแกรมตัวอย่าง 9 โปรแกรม
  - ไปที่ตัวอย่างที่ 3
    - พิมพ์ cd 03_Output-Port
    

4. ดู source code program 
- พิมพ์ vi src/main.cpp
- โปรแกรม นี้ set up serial port ที่ port 0 (output)
- ใน loop แสดงให้เห็นถึงการวนไปเรื่อยๆ ในบรรทัดต่างๆ ประกอบด้วย
  - cnt++ หมายถึง การนับเพิ่มเรื่อยๆ 
    - หาก cnt%2 แปลว่า ตัวเลขเป็นเลขคี่ ให้ดำเนินการ on
    - หากไม่ใช่ แปลว่า ตัวเลขเป็นเลขคู่ ให้ดำเนินการ off
  - delay(500) หมายถึง ช่วงเวลา 500 ms 
 
```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON ===========");
		digitalWrite(0, HIGH);
	} else {
		Serial.println("========== OFF ===========");
		digitalWrite(0, LOW);
	}
	delay(500);
}
```

5. อัพโหลดโปรแกรม 03_Output-Port เข้าไปยัง microcontroller โดยใช้คำสั่ง upload
   - พิมพ์ pio run -t upload
   - ![AQ](https://user-images.githubusercontent.com/81258597/112336080-54a36980-8cef-11eb-93fc-fdbe6619db98.png)
   - ในขณะที่ program กำลังรันข้อมูล เพื่อให้ microcontroller รับโปรแกรมใหม่เข้าไป
     - กดปุ่มสีแดง เพื่อให้เกิดการ reset
     

   - สังเกตผลลัพธ์ที่แสดงผลผ่านคอมพิวเตอร์
     - พิมพ์ pio device monitor

6. นำ microcontroller ต่อเข้ากับ Relay เพื่อให้ Relay ทำหน้าที่เปิด-ปิดสวิตซ์ 

![ww](https://user-images.githubusercontent.com/81258597/112336784-f6c35180-8cef-11eb-9806-ee6b3a5324b4.png)

7. นำแหล่งจ่ายไฟมาต่อเข้ากับตัว Relay เพื่อจ่ายไฟให้ Relay ทำงานได้
   - ผลลัพธ์ออกมาเป็น on off สลับกันไปเรื่อยๆ ดังภาพ
     - ไฟสีเขียวติดๆดับๆ สลับกัไป
     - ![Screenshot 2021-03-24 222345](https://user-images.githubusercontent.com/81258597/112336559-bfed3b80-8cef-11eb-8cc1-0b272572039e.png)

     - ไฟสีน้ำเงิน ไม่ติดเลย
     -   ![Screenshot 2021-03-24 222333](https://user-images.githubusercontent.com/81258597/112336489-b4017980-8cef-11eb-928f-b62594afeb6b.png)
     

 

## การบันทึกผลการทดลอง 

count | ลักษณะสีของหลอดไฟ | ความสว่าง
------------ | ------------- | -------------
เลขคี่ | ไฟสีเขียว | ติด
เลขคู่ | ไฟสีเขียว | ดับ

## อภิปรายผลการทดลอง 
- pio run -t upload นั้นใช้ในการอัพโหลดข้อมูลจาก 03_Output-Port ไปยัง microcontroller โดยคำสั่ง pio device monitor ที่ใช้ดูผลลัพธ์ของโปรแกรมที่ถูกอัพโหลดเข้าไป โดย output ที่ได้ออกมาจะสังเกตว่า ขึ้น on-off สลับกันไปในทุกๆ 500 ms 
- ในขณะเดียวกันหลอด LED เปล่งแสง ที่ port 0 ซึ่ง ติดและดับ ตามคำสั่ง on-off
- เมื่อนำ microcontroller ต่อเข้ากับ Relay จะสังเกตได้ว่า Relay มีลักษณะการเปิด-ปิดสวิตซ์สอดคล้องกับ code ของโปรแกรมที่เราใส่

## คำถามหลังการทดลอง
หากเปลี่ยน delay เป็น 1000 ms สำหรับโจทย์ข้อนี้ หมายถึงอะไร
- [ ] output จะแสดง on-off สลับกันไปในทุกๆ 500 ms 
- [x] output จะแสดง on-off สลับกันไปในทุกๆ 1000 ms 
