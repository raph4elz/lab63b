# การทดลองที่ 7 ปิด เปิด ไฟ LED ผ่าน WiFi

## วัตถุประสงค์
1. เพื่อเข้าใจเกี่ยวกับโครงสร้างของ code program และปรับแต่ง
2. เพื่อคิดค้นการทำงานที่จะใช้ผ่าน microcontroller รูปแบบใหม่
3. เพื่อทำการประยุกต์ใช้ความรู้ที่ศึกษาจากข้อมูลที่ได้ศึกษาจากทุกแลป

## อุปกรณ์ที่ใช้
1. Arduino UNO R3 - Made in italy

2. ESP8266 ESP-01 Wireless WIFI Module

3. ไอซีเร็กกูเลเตอร์ LD1117

4. คาปาซิเตอร์ 100 nF

5. คาปาซิเตอร์ 10 uF

6. สาย Jumper Female to Male ยาว 20cm.

7. สาย Jumper Male to Male ยาว 20cm.

8. Prototype PCB Board 4x6 cm Double Sides 2 ชิ้น

9. สกรูหัวกลม+น็อตตัวเมีย ขนาด 3มม ยาว 12มม

10. Relay 1 Channel DC 5V Module

11. SMD LED Lighting G4 AC DC 12V

12. รางถ่าน AA 8 ก้อน

## แหล่งข้อมูลเพื่อการศึกษา
- หาที่อยู่ IP adress 
  1. กดค้นหาตรง Windows ว่า Control Pane
   ![Screenshot 2021-04-01 001820](https://user-images.githubusercontent.com/81258597/113185894-34405580-9281-11eb-9ba6-ca1f98c352af.png)
  2. ในหน้าต่างของ Control Panel, คลิกที่ Network and Internet
   ![Screenshot 2021-04-01 002010](https://user-images.githubusercontent.com/81258597/113185925-3b676380-9281-11eb-92bb-d3428ea38327.png)

  3. ในหน้าต่างของ Network and Sharing Center, ในส่วนของ View your active networks, ที่อยู่กับ Connectionsให้คลิกที่ลิงค์ของการเชื่อมต่อเครือข่าย.
   
   ![Screenshot 2021-04-01 002135](https://user-images.githubusercontent.com/81258597/113185943-402c1780-9281-11eb-9280-d69f48757080.png)

  4. ในหน้าต่างของ Network Connection Status,คลิกที่ปุ่ม Details
   
   ![Screenshot 2021-04-01 002225](https://user-images.githubusercontent.com/81258597/113185960-44583500-9281-11eb-9b62-2197d36e1da7.png)

  5.ในหน้าต่างของ Network Connection Details,ต่อจาก IPv4 IP Address จะมี IP address แสดงขึ้นมา.
   
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
1. ต้องแก้ไข ไอพี ของ ESP8266 ESP-01 ให้ตรงกับที่เราได้รับมาจาก Router

![Screenshot 2021-04-01 005030](https://user-images.githubusercontent.com/81258597/113188464-48d21d00-9284-11eb-9fb2-27b302c2556d.png)


2. การต่อวงจร ระหว่าง ESP8266 ESP-01 กับ Arduino UNO
![Screenshot 2021-04-01 012649](https://user-images.githubusercontent.com/81258597/113192742-5a69f380-9289-11eb-9a12-e666bbdb0750.png)


3. ต่อวงจร ระหว่าง Relay กับ Arduino UNO
  ![Screenshot 2021-04-01 012736](https://user-images.githubusercontent.com/81258597/113192829-779ec200-9289-11eb-8d9c-8c7f43096044.png)

4. ต่อวงจร ระหว่าง UNO+ Relay + LED + รางถ่าน
   ![Screenshot 2021-04-01 015722](https://user-images.githubusercontent.com/81258597/113196394-a9198c80-928d-11eb-9ada-12edb4aa98f6.png)



5. อัพโหลดโค้ด ไปยัง  Arduino UNO   
```javascript
#include <SoftwareSerial.h>

#define TIMEOUT 5000 // mS
#define LED 5
SoftwareSerial mySerial(7, 6); // RX, TX


void setup()
{
 pinMode(LED,OUTPUT);
 Serial.begin(115200);
 mySerial.begin(115200);
 SendCommand("AT+RST", "Ready");
 delay(5000);
 SendCommand("AT+CWMODE=1","OK");
 SendCommand("AT+CIFSR", "OK");
 SendCommand("AT+CIPMUX=1","OK");
 SendCommand("AT+CIPSERVER=1,80","OK");
}

void loop(){


  
 String IncomingString="";
 boolean StringReady = false;

 while (mySerial.available()){
   IncomingString=mySerial.readString();
   StringReady= true;
  }

  if (StringReady){
    Serial.println("Received String: " + IncomingString);
  
  if (IncomingString.indexOf("LED=ON") != -1) {
    digitalWrite(LED,HIGH);
   }

  if (IncomingString.indexOf("LED=OFF") != -1) {
    digitalWrite(LED,LOW);
   }
  }
 }

boolean SendCommand(String cmd, String ack){
  mySerial.println(cmd); // Send "AT+" command to module
  if (!echoFind(ack)) // timed out waiting for ack string
    return true; // ack blank or ack found
}

boolean echoFind(String keyword){
 byte current_char = 0;
 byte keyword_length = keyword.length();
 long deadline = millis() + TIMEOUT;
 while(millis() < deadline){
  if (mySerial.available()){
    char ch = mySerial.read();
    Serial.write(ch);
    if (ch == keyword[current_char])
      if (++current_char == keyword_length){
       Serial.println();
       return true;
    }
   }
  }
 return false; // Timed out
}

```

6. หลังอัพโหลดเสร็จแล้ว ให้ไปที่ Tools -> Serial Monitor
  ![Screenshot 2021-04-01 015827](https://user-images.githubusercontent.com/81258597/113196507-c8b0b500-928d-11eb-8b9b-00d4fe179e27.png)

7. เปิด Serial Monitor ของ Arduino ตั้งค่า baud rate 115200 และปรับช่องในรูปให้เป็น Both NL&CR
รอจนกระทั่งขึ้นคำว่า Received String:
แสดงว่าโปเจคเรา พร้อมทํางาน แล้ว

![Screenshot 2021-04-01 015914](https://user-images.githubusercontent.com/81258597/113196588-e120cf80-928d-11eb-8cd7-77c19ea24b94.png)

*  ทดสอบโดย เว็บเบราเซอร์ (web browser)
    เริ่มด้วย เปิด เว็บเบราเซอร์ (web browser) ขึ้นมา
    คำสั่ง LED=ON และ LED=OFF ต้องพิมพ์เป็นภาษาอังกฤษตัวพิมพ์ใหญ่ เท่านั้น


    พิมพ์ http://192.168.1.51/LED=ON
    หรือ 192.168.1.51/LED=ON
    Enter
    ไฟ LED จะติด

    พิมพ์ http://192.168.1.51/LED=OFF
    หรือ 192.168.1.51/LED=OFF
    Enter
    ไฟ LED จะดับ


## การบันทึกผลการทดลอง 
  เราจะสามารถเปิดปิดไฟได้ผ่านทางการพิมพ์เข้าไปได้ผ่านเว็บได้เเต่ต้องเป็น IP ของwifi ที่เราทำการเชื่อมต่ออยู่ ไฟจะติดหรือดับเราสามารถกำหนดได้เองผ่านตัวเว็บที่เราทำการเชื่อเข้ากับตัวโปรเเกรม

## อภิปรายผลการทดลอง 
  เนื่องจากการทดลองนี้อาศัยการประยุกต์และรวบรวมข้อมูลผ่านความรู้ที่ได้ทำการศึกษาจากทุกแลป และด้วยสถานการณ์การแพร่ระบาดของโรค COVID-19 ทำให้ไม่สามารถทำการทดสอบโค้ดของโปรแกรมที่เขียนขึ้นมาใหม่ได้ว่ามีความถูกต้องหรือไม่ แต่ด้วยความรู้ที่มีจึงนำมาประยุกต์ออกมาได้เป็นการทดลองที่ 7 ดังที่ได้เขียนและกล่าวไปในข้างต้น 

## คำถามหลังการทดลอง 
ในการจะเขียนให้ไฟติดหรือดับต้องเป็นตัวใหญ่ หรือ ตัวเล็ก
* ตอบ ตัวใหญ่เสมอ

