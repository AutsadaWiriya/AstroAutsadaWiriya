---
title: The simple harmonic model measures the period, acceleration due to gravity, and angular velocity.
published: 2024-03-14
tags: ["Project"]
category: Project
image: ./img.png
draft: false
---

## 🧑🏼‍🔧 Project นี้ อยู่ในรายวิชา Embeded Lab ⚡
### โดยจะใช้ความรู้ทางด้าน Micro Controller ในการออกแบบและสร้างตัว Project นี้ขึ้นมา เพื่อใช้งานและช่วยสร้างประโยชน์ให้กับผู้ใช้งานได้


**จุดประสงค์**\
เพื่ออธิบายและแสดงที่มาของสูตรสำหรับการหาคาบการแกว่งของลูกตุ้ม (Pendulum) โดยใช้เป็นสื่อการเรียนการสอนในหัวข้อการเคลื่อนที่แบบการแกว่งของลูกตุ้ม เพื่อหาค่าคาบการแกว่งและอัตราเร็วเชิงมุม นอกจากนี้ ยังมีการพิสูจน์ความถูกต้องของผลการทดลองโดยการแทนค่าความเร่งเนื่องจากแรงโน้มถ่วง (g) เพื่อยืนยันความแม่นยำของการทดลอง

**หลักการทำงาน**\
ระบบนี้เริ่มจากการที่ลูกตุ้มถูกยึดติดด้วยแรงแม่เหล็กไฟฟ้า เมื่อกดปุ่มเริ่มต้น ระบบจะทำการปิดการทำงานของแม่เหล็กไฟฟ้า ส่งผลให้ลูกตุ้มเริ่มแกว่ง โดยมีเซ็นเซอร์อินฟราเรด (IR sensor) คอยตรวจจับการเคลื่อนที่ของลูกตุ้มขณะที่แกว่งผ่านจุดตรวจจับ ข้อมูลที่ได้จากเซ็นเซอร์จะถูกนำไปใช้ในการคำนวณคาบการแกว่งของลูกตุ้มและอัตราเร็วเชิงมุม นอกจากนี้ ระบบจะควบคุมการเปิด-ปิดแม่เหล็กไฟฟ้าในช่วงเวลาที่เหมาะสมเพื่อให้ลูกตุ้มสามารถแกว่งได้อย่างต่อเนื่องเป็นเวลานาน การเปิด-ปิดแม่เหล็กไฟฟ้านี้ถูกกำหนดจากระยะการเคลื่อนที่ที่ได้รับจากเซ็นเซอร์อินฟราเรด ผลลัพธ์ที่คำนวณได้จะถูกแสดงผลผ่านระบบ MQTT และจอ LCD
ในขณะเดียวกัน ลูกตุ้มอีกตัวหนึ่งจะถูกใช้สำหรับการทดลองปล่อยด้วยมือ เพื่อหาจังหวะที่เหมาะสมในการควบคุมการแกว่งให้ลูกตุ้มทั้งสองไม่ชนกัน

**วัสดุและอุปกรณ์ประกอบด้วย:**
| วัสดุ/อุปกรณ์                                        | จำนวน |
|------------------------------------------------------|-------|
| เซ็นเซอร์อินฟราเรด (IR sensor)                      | 1 ตัว |
| บอร์ดไมโครคอนโทรลเลอร์ ESP32                      | 2 ตัว |
| ตัวรับสัญญาณ Wi-Fi                                  | 1 ตัว |
| ลูกตุ้ม                                              | 2 ลูก |
| เส้นเอ็นสำหรับยึดลูกตุ้ม                              | -     |
| กาว                                                  | -     |
| เครื่องพิมพ์ 3 มิติ (3D printer)                     | -     |
| ปุ่มกด (Push Button)                                 | 1 ชิ้น|
| สายไฟ                                               | -     |
| ตัวต้านทาน (Resistor) และตัวเก็บประจุ (Capacitor)    | -     |
| หลอด LED                                            | 1 หลอด|
| คอมพิวเตอร์สำหรับควบคุมและแสดงผล                    | -     |
| แม่เหล็ก                                            | 1 ตัว |

**Design (การออกแบบ)**\
ในขั้นต้นได้ออกแบบโครงสร้างเป็นรูปตัว T โดยวางแผนให้ปล่อยลูกตุ้มตัวแรกก่อน แล้วทำการวัดคาบการแกว่งของลูกตุ้มนั้นเพื่อใช้ในการคำนวณและปล่อยลูกตุ้มตัวที่สองในเวลาที่เหมาะสม เพื่อหลีกเลี่ยงไม่ให้ลูกตุ้มทั้งสองชนกัน อย่างไรก็ตาม เนื่องจากข้อจำกัดทางด้านเวลาและข้อจำกัดอื่น ๆ ซึ่งจะกล่าวถึงในหัวข้อ "ปัญหาที่พบระหว่างทำโปรเจค" จึงมีความจำเป็นต้องปรับเปลี่ยนโครงสร้างการออกแบบใหม่\

**Design แบบแรก**  
![Front View](./Frontview.webp)
<center>Front View</center>

![Top view](./Topview.webp)
<center>Top View</center>

**Design แบบที่ทำเสร็จแล้ว**
![CCurrent กesign](./Currentdesign.webp)
<center>Current Design</center>

![Schemetic](./Schemetic.webp)
<center>Schematic Diagram</center>

![PCB](./PCB.webp)
<center>PCB</center>

**Code:**\
ESP32 ตัวที่ 1
```bash
#define BUTTON 34
#define SENSOR 23
#define MAGNETIC 27

float previousTimer = 0.000f;
float timer[2] = {0.000f};
int count = 0;
bool statusSensor = false;
bool firstTime = true;
bool buttonPressed = false;
char messageT[75];
bool check = false;

const char ssid[] = "ssid";
const char pass[] = "pass";

const char mqtt_broker[]="test.mosquitto.org";
const char mqtt_topic[]="####";
const char mqtt_client_id[]="####";
int MQTT_PORT=1883;

WiFiClient net;
MQTTClient client;

void connect() {
  Serial.print("checking wifi...");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(1000);
  }

  Serial.print("\nconnecting...");
  while (!client.connect(mqtt_client_id)) {  
    Serial.print(".");
    delay(1000);
  }

  Serial.println("\nconnected!");

  client.subscribe(mqtt_topic);
  // client.unsubscribe("/hello");
}

void messageReceived(String &topic, String &payload) {
  Serial.println("incoming: " + topic + " - " + payload);
  if(payload == "pushButton"){
    digitalWrite(MAGNETIC, HIGH);
    delay(100);
    digitalWrite(MAGNETIC, LOW);
  }
}

void setup() {
  // ปุ่มปล่อยแม่เหล็ก
  pinMode(BUTTON, INPUT);
  // sensor ตรวจจับ
  pinMode(SENSOR, INPUT);
  // แม่เหล็ก
  pinMode(MAGNETIC, OUTPUT);
  digitalWrite(MAGNETIC, LOW);
  WiFi.begin(ssid, pass);
  Serial.begin(9600);
  client.begin(mqtt_broker, MQTT_PORT, net);
  client.onMessage(messageReceived);

  connect();
  lcd.begin();
  lcd.backlight();

  lcd.print("Group2.11");
}

void loop() {
  client.loop();
  if (!client.connected()) {
    connect();
  }

  // จับเวลาและหยุดจับเวลา
  if(digitalRead(SENSOR) == 0 && !statusSensor){
    if (firstTime) {
      firstTime = false;
    } else {
      timer[count] = millis() - timer[count]; // หยุดจับเวลา
      count++;
      if (count == 2) {
        count = 0;
        // ตรวจสอบค่า timer
        bool validData = true;
        for (int i = 0; i < 2; i++) {
          if (timer[i] < 200.000f) {
            validData = false;
            break;
          }
        }
        if (validData) {
          // คำนวณค่า timer เฉลี่ย
          float averageTimer = 0.000f;
          for (int i = 0; i < 2; i++) {
            averageTimer += timer[i];
          }
          averageTimer /= 2.000f;
          // คำนวณค่า g เฉลี่ย
          double g = 0.0;
          double T = 0.0;
          double W = 0.0;
          for (int i = 0; i < 2; i++) {
            g += 4 * PI * PI * (0.09 / ((timer[i] / 500.0) * (timer[i] / 500.0)));
          }
          g /= 2.0;
          W = sqrt(g/0.09);
          lcd.setCursor(0, 1);
          // แสดงผลลัพธ์
          if( g > 8.5){
            lcd.clear();
            lcd.print("T: ");
            lcd.print(averageTimer/500.000);
            lcd.print(" s");
            lcd.setCursor(0, 2);
            lcd.print("g: ");
            lcd.print(g);
            lcd.print(" m/s^2");

            Serial.println();
            Serial.print("T: ");
            Serial.print(averageTimer/500.000);
            Serial.println(" s");
            Serial.print("g: ");
            Serial.print(g);
            Serial.println(" m/s^2");
            Serial.print("w: ");
            Serial.print(W);
            Serial.println(" rad/s");
            sprintf(messageT, "T: %.3f s __ g: %.2f m/s^2 __ w: %.2f rad/s", averageTimer/500.0 , g, W);
            client.publish(mqtt_topic, messageT);
          }
        }
      }
    }
    timer[count] = millis(); // เริ่มจับเวลา
    statusSensor = true;
  }
  if(digitalRead(SENSOR) == 1 && statusSensor){
    statusSensor = false;
  }
}
```
ESP32 ตัวที่ 2
```bash
#define MAGNETIC 27
#define BUTTON 34
#define LED 21

float previousTimer = 0.000f;
float timer[2] = {0.000f};
int count = 0;
bool statusSensor = false;
bool firstTime = true;
bool buttonPressed = false;
char messageT[50];
bool check = false;

void setup() {
  pinMode(BUTTON, INPUT);
  pinMode(MAGNETIC, OUTPUT);
  digitalWrite(MAGNETIC, HIGH);
  pinMode(LED, OUTPUT);
  digitalWrite(LED,HIGH);
}

void loop() {
  if (digitalRead(BUTTON) == HIGH && !buttonPressed) {
    buttonPressed = true;
    digitalWrite(MAGNETIC, LOW);
    digitalWrite(LED,LOW);
    while(buttonPressed){
      
      delay(310);
      digitalWrite(MAGNETIC, HIGH);
      digitalWrite(LED,HIGH);
      delay(300);
      digitalWrite(MAGNETIC, LOW);
      digitalWrite(LED, LOW);
      
    }
  }
  if (digitalRead(BUTTON) == LOW && buttonPressed) {
    buttonPressed = false;
    digitalWrite(MAGNETIC, HIGH);
    digitalWrite(LED,LOW);
  }
}
```

</br>

**วิดีโอแสดงตัวอย่างการใช้งาน:**
<iframe width="560" height="315" src="https://www.youtube.com/embed/qnWkYZnzDow?si=WXlDx1Wvk1H5ZI_X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

</br>

**ปัญหาที่พบระหว่างทำโครงการ**\
**ปัญหาแรก**เกิดขึ้นจากแนวคิดเริ่มต้นที่วางแผนให้มีลูกตุ้ม 2 ตัว จัดวางในรูปตัว T แต่เนื่องจากข้อจำกัดด้านเวลา ทำให้ต้องปรับลดขนาดของงานลง จึงได้ตกลงกับอาจารย์และพี่ TA ให้เปลี่ยนเป็นการใช้เพียงแกนเดียวแทน\
**ปัญหาที่สอง**คือแม่เหล็กที่ใช้นั้นต้องการไฟขนาด 12V ในการทำงาน แต่ไฟที่จ่ายจากบอร์ด ESP32 มีเพียง 5V ทำให้ต้องตัดสินใจซื้ออะแดปเตอร์ขนาด 12V 1A มาใช้เป็นแหล่งจ่ายไฟให้กับแม่เหล็ก พร้อมทั้งใช้ตัวแปลงแรงดันไฟฟ้า (Step Down) เพื่อลดแรงดันไฟให้เหลือ 5V สำหรับเลี้ยงบอร์ด ESP32 อย่างไรก็ตาม เมื่อใช้ตัวแปลง Step Down แล้ว ไฟฟ้าที่ได้ไม่เพียงพอที่จะทำให้ระบบทำงานได้ตามปกติ ทำให้ไม่สามารถกดปุ่มเพื่อปล่อยลูกตุ้มได้ จึงจำเป็นต้องกลับไปใช้ไฟ 5V เหมือนเดิม\
**ปัญหาสุดท้าย**คือในตอนแรกตั้งใจให้ลูกตุ้มแกว่งอย่างต่อเนื่อง แต่เนื่องจากแรงแม่เหล็กไม่แรงพอ รวมทั้งแรงต้านอากาศ ทำให้ไม่สามารถทำให้ลูกตุ้มแกว่งตลอดเวลาได้ตามที่คาดหวังไว้





**ประสบการณ์และคำแนะนำ**
พบว่าการออกแบบ PCB ที่ไม่เหมาะสมส่งผลให้ไม่สามารถนำมาใช้งานกับชิ้นงานได้อย่างมีประสิทธิภาพ นอกจากนี้ยังพบปัญหาในการทำให้ลูกตุ้มแกว่งตลอดเวลาไม่ได้ตามที่คาดหวัง เนื่องจากการออกแบบการทดลองที่ไม่สอดคล้องกับข้อจำกัดด้านเวลา รวมทั้งการจัดการเวลาไม่ดีพอ ในอนาคตควรมีการวางแผนการทำงานที่ละเอียดขึ้น และศึกษาค้นคว้าหาข้อมูลเพิ่มเติม เพื่อให้การทดลองและการดำเนินงานเป็นไปอย่างราบรื่น

**จัดทำโดย**\
อภิวิชญ์ บุญฤทธิ์\
อัฎ​ษฎา วิริยา\
อาทิตยา เที่ยงอารมย์\
เอื้ออาทร เอื้อวงศ์ตระกูล