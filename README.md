# ros2_exam1 หุ่นยนต์เต่าหรรษา+<br>
ในโปรแกรมนี้มีการควบคุมหุ่นยนต์ผ่านการป้อนคำสั่งจากคีย์บอร์ด (Teleoperation) เพื่อสั่งการให้ turtlesim_plus เคลื่อนที่ไปตามที่เราควบคุม <br>
โดยเริ่มจากการทำงานทั้งหมดอยู่ภายใน workspace ที่มีชื่อว่า exam1_ws จากนั้นได้ทำการสร้าง package 2 ส่วนได้แก่<br>
<br>![teleop_key (6)](https://github.com/user-attachments/assets/bbe3560c-e75a-4ddc-89dd-29f3c641d3f4)

# **ส่วนแรก** : ส่วน teleop เต่า โดยจะมี node ต่าง ๆ ดังนี้
  teleop_schedule.py - ทำหน้าที่จัดการการเปลี่ยนสถานะของหุ่นยนต์ จัดการ state 
  teleop_key.py - จัดการรับคำสั่งจากคีย์บอร์ดเพื่อควบคุมหุ่นยนต์
  controller.py - ตัวควบคุมหลักที่รวมการทำงานของหุ่นยนต์และการส่งคำสั่งการเคลื่อนไหว เป็นส่วนที่รวมระบบเข้าด้วยกัน

  
**การติดตั้ง**
1.โหลไฟล์ที่แนบมา
2.เข้าไปที่ workspace และ pkg
```
cd exam1_ws
```
3.colcon build ที่ workspace
```
colcon build
```
<br>
**การใช้งาน**
1. การรัน Teleop Schedule

ไฟล์ teleop_schedule.py ทำหน้าที่จัดการการตั้งค่าการทำงานหลักของหุ่นยนต์ และการเปลี่ยนสถานะของหุ่นยนต์ได้ตามต้องการ
```
ros2 run mission teleop_schedule
```

ฟังก์ชันหลัก:
    timer_callback() - ทำงานตามความถี่ที่ตั้งค่าไว้ เพื่อตรวจสอบสถานะของหุ่นยนต์และส่งข้อมูลออก
    switch_state_server_callback() - ฟังก์ชันที่รอรับคำสั่งเปลี่ยนสถานะจากผู้ใช้งานและส่งกลับไปยังหุ่นยนต์

2. การควบคุมหุ่นยนต์ด้วยคีย์บอร์ด

ไฟล์ teleop_key.py รับอินพุตจากคีย์บอร์ดเพื่อควบคุมการเคลื่อนไหวของหุ่นยนต์ เช่น เดินหน้า ถอยหลัง เลี้ยวซ้าย และเลี้ยวขวา
```
ros2 run your_package_name teleop_key
```

**ฟังก์ชันที่สำคัญ:**
  getKey(): ใช้ในการอ่านข้อมูลจากคีย์บอร์ดที่ผู้ใช้กด<br>
  timer_callback(): ฟังก์ชันนี้ทำการตรวจสอบและส่งคำสั่งไปยังหุ่นยนต์ตามปุ่มที่กด<br>
<br>
<br>**ปุ่มควบคุม:** <br>
    w - เดินหน้า<br>
    a - เลี้ยวซ้าย<br>
    s - ถอยหลัง<br>
    d - เลี้ยวขวา<br>
    i - spawn pizza<br>
    o - save value to yaml file<br>
    p - clear pizza <br>
    Ctrl+C - หยุดโปรแกรม<br><br>
ตัวอย่างการใช้งาน การควบคุมให้เต่าเคลื่อนที่ <br>
![image](https://github.com/user-attachments/assets/7a163466-f985-4b6f-8ca8-200b590d1ea5)

3. การควบคุมหุ่นยนต์ผ่าน Controller

ไฟล์ controller.py จัดการการเคลื่อนไหวและสถานะของหุ่นยนต์ ควบคุมการหมุนและความเร็ว ใช้ในการควบคุมเพื่อให้หุ่นยนต์เคลื่อนไหวอย่างราบรื่น<br>
<br>ฟังก์ชันหลัก:
    timer_callback() - ตรวจสอบสถานะและเคลื่อนที่ของหุ่นยนต์<br>
    pose_callback() - รับตำแหน่งของหุ่นยนต์จากเซ็นเซอร์และคำนวณความเร็วที่ต้องการ<br>
    walk_until_end() - ให้หุ่นยนต์เคลื่อนที่ไปยังตำแหน่งเป้าหมายตามคิวที่กำหนดไว้<br>
    cmdvel() - ส่งคำสั่งการเคลื่อนที่ไปยังหุ่นยนต์ โดยจะมีค่าความเร็วและตำแหน่ง<br><br>
<br>ทำการรันดังนี้
```
ros2 run turtlesim_plus turtlesim_plus_node.py
```
```
ros2 run mission controller.py
```
```
ros2 run mission teleop_key.py
```
```
ros2 run mission teleop_schedule.py
```
<br>

![image](https://github.com/user-attachments/assets/fb1a20a7-dcf0-46b5-a9c0-13352bd9898b)

<br>จากโจทย์ที่ได้รับในส่วนที่ 1  เราสามารถควบคุมการเคลื่อนที่ของเต่าผ่านคีย์บอร์ดได้ และทำการทิ้งพิซซ่า บันทึกค่าตำแหน่งที่ทิ้งพิซซ่าได้ <br> โดยค่าที่ได้ถูกเก็บไว้ใน yaml file ซึ่งแสดงผลได้ดังนี้<br>
<br>![image](https://github.com/user-attachments/assets/4ba71c06-2962-45ab-8f95-47b3f4546903)
แต่ในส่วนของการเคลียร์หรือให้เต่ากินพิซซ่าที่ทิ้งไว้แล้ว เกิดข้อผิดพลาดเล็กน้อย ซึ่งทำให้เต่าไม่สามารถกินพิซซ่าได้ และเมื่อกดปุ่มเคลียร์ ทำให้การควบคุมเต่าเปลี่ยนไป ทำให้เต่าเดินเป็นวงกลม ล้อมรอบตรงที่ตอนกินพิซซ่า<br>
<br>ตัวอย่างงาน ![image](https://github.com/user-attachments/assets/f2fc1413-b0c7-4733-9f01-b06b051ef7d3)




# **ส่วนที่สอง** : ส่วน teleop เต่า โดยจะมี node ต่าง ๆ ดังนี้
ในส่วนนี้ ต้องทำการ spawn turtle 4 ตัว โดยได้ทำการ spawn ผ่าน lauch file ต้องตั้งชื่อ<br>หุ่นยนต์เต่าตัวที่ 1 ว่า Foxy โดยจะมีหน้าที่วิ่งไปทิ้ง Pizza ตามชุดตำแหน่งที่ได้จากการ save ครั้งที่ 1
<br>หุ่นยนต์เต่าตัวที่ 2 ชื่อ Noetic มีหน้าที่วิ่งไปทิ้ง Pizza ตามชุดตำแหน่งที่ได้จากการ save ครั้งที่ 2<br>
<br>หุ่นยนต์เต่าตัวที่ 3 ชื่อ Humble มีหน้าที่วิ่งไปทิ้ง Pizza ตามชุดตำแหน่งที่ได้จากการ save ครั้งที่ 3<br>
<br>หุ่นยนต์เต่าตัวที่ 4 ชื่อ Iron มีหน้าที่วิ่งไปทิ้ง Pizza ตามชุดตำแหน่งที่ได้จากการ save ครั้งที่ 4<br>
<br>ซึ่งมีข้อจำกัดในการเขียนโค้ดคือ ต้องทำการนำชื่อเข้า FUNCTION for loop เพื่อทำการเติมชื่อเข้าไปที่เต่าแต่ละตัว<br>

ในที่นี้ได้ทำการ spawn turtle ที่จุดเริ่มต้นทางด้านล่างซ้าย แล้วทำการรับค่าจากไฟล์ yaml ทำให้ได้ค่าตำแหน่งที่ต้องการให้เต่าเคลื่อนที่ไปแล้วทำการทิ้งพิซซ่า<br> โดยเงื่อนไขต่าง ๆ จะทำอยู่ใน node copy_controller.py<br>
<br>
<br>คำสั่งในการ run เต่า 4 ตัว
```
ros2 lauch mission copy_turtle.py
```
![image](https://github.com/user-attachments/assets/dafd4992-23c2-4f79-945f-ff5990dd0d9e)
<br>

# **ปัญหาที่เกิดขึ้น**
เริ่มแรกเกิดจากการที่ไม่ได้วางแผนในการออกแบบ architecture ก่อน ทำให้มองเห็นภาพการทำงานของระบบไม่ชัดเจน ทำให้เริ่มทำงานลำบากและไม่ค่อยเข้าใจกติกา และกฎต่าง ๆ ในช่วงแรก ทำให้เกิดการวางแผนผิดพลาดในช่วงแรก เนื่องจากไม่ได้ทำการแยกไฟล์ ควบคุมผ่านคีย์บอร์ด ไฟล์จัดการ state และไฟล์ controller ออกจากกัน ทำให้เสียเวลาในการทำค่อนข้างเยอะ เพราะระหว่างทำก็พบเจออุปสรรคหลายอย่าง ต่อมาได้มีการวางแแผนที่ดีขึ้น ทำให้มีการแยกเป็น 3 ไฟล์ตามทมี่กล่าวมาข้างต้น แต่ก็เกิดปัญหาต่าง ๆ อีกหลายอย่าง เช่น ไฟล์รันไม่ผ่าน colcon build ไม่ได้ ทำให้เสียเวลาในการแก้ไปส่วนใหญ่ และหลายครั้งแค่รันใหม่ ไม่ได้มำการแก้ไขอะไรก็ทำให้ใช้ได้อย่างน่าอัศจรรย์ หรือในบางครั้งแค่ปิดเครื่องแล้วกดรันใหม่ ก็ทำให้สามารถแก้บัคโค้ดได้เหมือนกัน และอีกประเด็นคือ สามารถรันในไฟล์ python ที่อยู่ใน VScode ได้ แต่พอมารันใน task space ก็เกิด error ขึ้น ซึ่งก็ใช้เวลาในการแก้ไขค่อนข้างเยอะ และแค่รันใหม่หลาย ๆ รอบก็สามารถแก้ไขได้ ปัญหาสุดท้ายคือ การวางแผนที่ผิดพลาด ในตอนแรกคิดว่าจะใช้เวลาในการทำแต่ละอันไม่นานมาก แต่พอทำจริงใช้เวลาในการแก้ไขโค้ดนาน ซึ่งสิ่งที่ตัดสินใจผิดคือ ไม่ได้ซ์้อของในตลาดมืดตั้งแต่แรก ถ้าซื้อมาอาจจะทำให้การทำงานราบรื่นขึ้น และทำให้มองเห็นภาพรวมได้ง่ายมากขึ้น



# **ผู้จัดทำ**
นายทัศน์พล สินเมือง 65340500025
นางสาวสิริมณี มณีเวศย์วโรดม 65340500055
<br>
# **รายวิชา** 
FRA501_2567 Robotics Dev
