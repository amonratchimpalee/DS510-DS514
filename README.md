# การวิเคราะห์ข้อมูลลูกค้าที่ยกเลิก Food Panda เพื่อวางแผนการตลาดให้ตอบโจทย์กับปัญหาที่เจอ
โครงการนี้สร้างโมเดลเชิงพยากรณ์เพื่อคาดการณ์ว่าลูกค้า Food Panda จะยกเลิกการใช้งานหรือไม่ (Churn) ซึ่งช่วยให้สามารถวางแผนความต้องการและกลยุทธ์ทางการตลาดได้อย่างมีประสิทธิภาพมากขึ้น

## ภาพรวมชุดข้อมูล

**แหล่งที่มาของข้อมูล  :** 
https://www.kaggle.com/datasets/nabihazahid/foodpanda-analysis-dataset-2025 

**จำนวนข้อมูล: 6,000 ออเดอร์**


**จำนวนตัวแปร: 20 ตัวแปร**
**-Description**


ชุดข้อมูลการสั่งอาหารผ่านเพลตฟอร์ม Foodpanda จำนวน 6,000 รายการ ซึ่งประกอบด้วยข้อมูลของลูกค้า(ข้อมูลการสั่งอาหาร การชำระเงิน คะแนนรีวิว และรายละเอียดการจัดส่ง
ในประเทศปากีสถาน


**ตั้งแต่ระยะเวลา 22 สิงหาคม 2023 ถึง วันที่ 21 สิงหาคม 2025**

### Data Dictionary



<img width="1171" height="605" alt="image" src="https://github.com/user-attachments/assets/88460a82-6148-4d8f-9db3-4a4a35cf1199" />





## ขั้นตอนการทำโมเดล
**1.การนำเข้าข้อมูล**

- ที่มา : Foodpanda Analysis Dataset จาก Kaggle ข้อมูลถูกนำเข้าในรูปแบบไฟล์ CSV
- คอลัมน์ข้อมูลดิบที่สำคัญประกอบด้วย `customer_id`, `gender`, `age`, `city`, `signup_date`, `order_id`, `order_date`, `restaurant_name`, `dish_name`, `category`, `quantity`, `price`, `payment_method`, `order_frequency`, `last_order_date`, `loyalty_points`, `churned`, `rating`, `rating_date`, `delivery_status`

- ขนาดข้อมูล **6000 รายการ** และ **20 คอลัมน์**


**2.การตรวจสอบและทำความสะอาดข้อมูล**

- ลบข้อมูลที่ไม่จำเป็นออก

- ข้อมูลที่ซ้ำกัน (Duplicate records)

- แถวที่มีค่าว่าง / ค่า NULL (ในกรณีที่เหมาะสม)

- ตรวจสอบชนิดข้อมูลและค่า missing (ไม่พบ missing values)

- แปลงประเภทข้อมูล (date → datetime, string → numeric)

**3.(Feature Engineering & Selection)**

ตัดตัวแปรที่ไม่จำเป็น

`city`, `category`, `dish_name`,
`payment_method`, `rating_date`, `gender`, `age`,
`customer_id`, `order_id`, `signup_date`, `order_date`


เพิ่ม Feature Engineering


`avg_price_per_unit` =`price` / `quantity`

`is_repeat_customer` =`order_frequency`> 1

`high_loyalty`=(`loyalty_points` >= `loyalty_points`.median()).astype(int)

`customer_tenure_days` = `order_date`- `signup_date`

`is_high_value_order`= `price` > `price`.median()).astype(int)

`is_delivered` = `delivery_status` == 'Delivered').astype(int)

`is_failed_delivery` = `delivery_status` != 'Delivered').astype(int)

กำหนดตัวแปรเป้าหมาย  **Target**  = `churned`

**4.การทำโมเดล**
แบ่งข้อมูล Train/Test (Train/Test Split) เพื่อประเมินความสามารถของโมเดลในการทำนายกับข้อมูลใหม่ (generalization)




**5.Evaluation**
ตัวชี้วัดประสิทธิภาพ (Metrics): 
- Confusion matrix
- ROC-AUC on test data

โดยหลักๆที่เราโฟกัสคือ ค่า Accuracy ของโมเดล 

