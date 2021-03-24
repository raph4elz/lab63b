
# การทดลองที่ 1 เรื่อง การเขียนโปรแกรมเพื่อรันบนไมโครคอนโทรเลอร์

## วัตถุประสงค์
 1. เพื่อเข้าใจคำสั่งต่างๆของโค้ดโปรแกรมที่ใช้รัน
 1. เพื่อศึกษาองค์ประกอบ ข้อมูลพื้นฐานของ microcontroller
 1. เพื่อการเขียนและรันโปรแกรมจาก microcontroller

## อุปกรณ์ที่ใช้ 
  1. microcontroller ESP-01 ที่ประกอบไปด้วย CPU และมีเสาอากาศสำหรับ WIFI 
  1. อุปกรณ์เชื่อมต่อ USB เพื่อไปยัง serial 
## แหล่งข้อมูลเพื่อการศึกษา
  https://www.youtube.com/watch?v=NLIUsWLEpmg
  
##วิธีการทำการทดลอง 
1. เสียบ microcontroller เข้าทาง serial port
 * ![Screenshot 2021-03-24 172303](https://user-images.githubusercontent.com/81258597/112295479-86eca100-8cc6-11eb-8868-8c1773c470cb.png)
1. ดูที่ตัวอย่างโปรแกรม ที่โฟลเดอร์ pattani 
 * พิมพ์ cd 01_Serial Monitor
 * ![Screenshot 2021-03-24 174015](https://user-images.githubusercontent.com/81258597/112297151-1d6d9200-8cc8-11eb-8bf7-ebca00c4b1b5.png)
 * พิมพ์ vi src/main.cpp
 * ![Screenshot 2021-03-24 174143](https://user-images.githubusercontent.com/81258597/112297268-468e2280-8cc8-11eb-902f-1083b1610e8f.png)
 จะโชว์ code ตัวอย่างให้เราเห็น
 ```Javascript
 #include <Arduino.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
}

void loop()
{
	cnt++;
	Serial.printf("PATTANI :%d\n",cnt);
	delay(1000);
}
```

1. คำสั่งต่อไปนี้ ![Screenshot 2021-03-24 174143](https://user-images.githubusercontent.com/81258597/112302624-dda9a900-8ccd-11eb-8f1a-6552084ee02f.png) จะเป็นการบอกข้อมูลของplatform
 
```Javascript
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
## การบันทึกผลการทดลอง

## อภิปรายผลการทดลอง
 1. platformio นั้น สามารถใช้เขียนโปรแกรมจาก microcontroller หลายชนิดที่มีบริษัทต่างกันได้ โดย คำสั่ง platformio.ini เป็นเหมือนตัวแสดงผลว่าการเขียนโปรแกรมครั้งนี้เราจะเขียนให้กับ microcontroller ตัวไหน
 2. pio run -t upload นั้นใช้ในการอัพโหลดข้อมูลไปยัง microcontroller โดยสามารถกดปุ่มซึ่งอยู่ภายนอก microcontroller เพื่อทำการโหลดและรีเซ็ตการรันโปรแกรมได้
## คำถามหลังการทดลอง
ถ้าลงโปรเเกรมเข้าต้องกดปุ่มใด 
