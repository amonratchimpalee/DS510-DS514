
**FoodPanda: วิเคราะห์ข้อมูลลูกค้า, Churn และการทำนายด้วย Machine Learning
โปรเจกต์นี้เป็นงาน Exploratory Data Analysis (EDA) + Statistical Testing + Machine Learning
 โดยใช้ข้อมูลแพลตฟอร์ม Food Delivery (FoodPanda-like dataset) เพื่อศึกษาพฤติกรรมลูกค้า
 วิเคราะห์ปัจจัยที่ส่งผลต่อ Churn (การเลิกใช้งาน) และสร้างโมเดลทำนาย**

 
**- วัตถุประสงค์ของโครงงาน**
วิเคราะห์รูปแบบการใช้จ่ายและพฤติกรรมการสั่งอาหาร


เปรียบเทียบความแตกต่างระหว่างลูกค้า Active vs Inactive (Churn)


ทดสอบสมมติฐานเชิงสถิติ (T-test)


คัดเลือก Feature สำคัญที่สัมพันธ์กับ Churn


สร้างโมเดล Logistic Regression  ,Random Forest ,XGBoost,LightGBM  เพื่อทำนายโอกาส Churn



**- ภาพรวมชุดข้อมูล**

** แหล่งที่มาของข้อมูล **
https://www.kaggle.com/datasets/nabihazahid/foodpanda-analysis-dataset-2025 

**จำนวนข้อมูล: 6,000 ออเดอร์**


**จำนวนตัวแปร: 20 ตัวแปร**
<img width="1171" height="605" alt="image" src="https://github.com/user-attachments/assets/88460a82-6148-4d8f-9db3-4a4a35cf1199" />




**ตัวแปรสำคัญ**


ราคา (price), จำนวน (quantity)


ความถี่การสั่ง (order_frequency)


คะแนนสะสม (loyalty_points)


ระยะห่างจากการสั่งล่าสุด (days_since_last_order)


สถานะลูกค้า (churned: Active / Inactive)


คะแนนรีวิว (rating)


สถานะการจัดส่ง (delivery_status)



** การตรวจสอบและทำความสะอาดข้อมูล**


ตรวจสอบชนิดข้อมูลและค่า missing (ไม่พบ missing values)


จัดการค่า Null


ลบ duplicates


แปลงประเภทข้อมูล (date → datetime, string → numeric)


เตรียมข้อมูลให้อยู่ในรูปแบบพร้อมวิเคราะห์และทำโมเดล



**Descriptive Statistics**

**ผลสถิติพื้นฐานของตัวแปรเชิงตัวเลข: **

<img width="359" height="290" alt="{BE2B4D9B-3322-4F92-96AA-A8E6BC66FA5F}" src="https://github.com/user-attachments/assets/aebc4cbd-a0d0-49d8-97c1-eb620aeae2d2" />


จากการวิเคราะห์สถิติเชิงพรรณนา พบว่าลูกค้าส่วนใหญ่มักสั่งอาหารเฉลี่ยประมาณ 3 รายการต่อออเดอร์ โดยมีมูลค่าเฉลี่ยประมาณ 800 บาท
ข้อมูลแสดงให้เห็นความแตกต่างของพฤติกรรมการใช้จ่าย และพบว่าตัวแปร days_since_last_order มีค่าเฉลี่ยสูง ซึ่งสะท้อนถึงความเสี่ยงในการเกิด churn



