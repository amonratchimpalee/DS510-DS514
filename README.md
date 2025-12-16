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


- ขนาดข้อมูล **6000 รายการ** และ **20 คอลัมน์**


**2.การตรวจสอบและทำความสะอาดข้อมูล**

- ลบข้อมูลที่ไม่จำเป็นออก

- ข้อมูลที่ซ้ำกัน (Duplicate records)

- แถวที่มีค่าว่าง / ค่า NULL (ในกรณีที่เหมาะสม)

- ตรวจสอบชนิดข้อมูลและค่า missing (ไม่พบ missing values)

- แปลงประเภทข้อมูล (date → datetime, string → numeric)

**3.(Feature Engineering & Selection)**
The model uses the following features:

### Demographics
- `gender`
- `age`
- `city`

### Purchase Behavior
- `total_orders`
- `total_spend`
- `avg_order_value`
- `avg_quantity`
- `avg_price_per_item`
- `order_frequency`
- `loyalty_points`

### Customer Experience
- `avg_rating`
- `delivered_rate`

### Time-based Features
- `customer_tenure_days`
- `days_since_last_order`
- `orders_per_tenure`





กำหนดตัวแปรเป้าหมาย  **Target**  = `churn_flag`

**4.การทำโมเดล**

- แบ่งข้อมูลเป็น Train/Test (Train/Test Split)
เพื่อประเมินความสามารถของโมเดลในการทำนายกับข้อมูลใหม่ (Generalization)

- สร้าง scikit-learn Pipeline
รวมขั้นตอนการเตรียมข้อมูล (Preprocessing) และโมเดลเข้าด้วยกัน

- ปรับจูนพารามิเตอร์ด้วย GridSearchCV

- ฝึกโมเดลทำนาย (Train Models)

Logistic Regression

Random Forest

XGBoost

LightGBM

ประเมินและเปรียบเทียบโมเดล (Predictive Model Comparison)
เปรียบเทียบประสิทธิภาพของโมเดลโดยเน้นค่า Recall สำหรับกลุ่มลูกค้าที่มีการ Churn
**5.Evaluation**
ตัวชี้วัดประสิทธิภาพ (Metrics): 

- Classification report (precision, recall, F1-score)
- Confusion matrix
- ROC-AUC on test data
  
โดยหลักๆที่เราโฟกัสคือ ค่า recall ของโมเดล

Visualizations:

-Confusion matrix heatmap

##  Model Performance

<img width="349" height="93" alt="{C703FB6F-FDD8-4BC1-8D2E-6502F21DFF46}" src="https://github.com/user-attachments/assets/99994549-bb35-45d2-b364-d3c7f1335a43" />


<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/a2a5aba7-826e-487e-ac42-ac8f68978f92" />


<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/32a2e12f-8c89-4d17-8422-5ec066e1d448" />

# Findings and Insights

โดยโมเดลที่ดีที่สุดคือ Random Forest

- Test Accuracy: 0.5125
- Test Recall (Churn=1): 0.5159
- Test ROC-AUC: 0.5218

<img width="396" height="249" alt="{EC46F23C-DC86-4380-90E9-502E8573BC76}" src="https://github.com/user-attachments/assets/e234bb3e-8af3-48a0-a8f3-d5b1d2bec8fe" />


<img width="760" height="393" alt="image" src="https://github.com/user-attachments/assets/79c98e90-5d54-4251-acb6-572df2443b62" />

Recall (Churn = 1): 0.5159
→ สามารถตรวจจับลูกค้าที่มีแนวโน้ม churn ได้ประมาณ 51.59%

ROC-AUC: 0.5218
→ โมเดลมีความสามารถในการแยกกลุ่ม churn และ non-churn ได้ดีกว่าการสุ่มเล็กน้อย

Accuracy: 0.5218
→ ความแม่นยำโดยรวมอยู่ในระดับปานกลาง และไม่ใช่ตัวชี้วัดหลักสำหรับปัญหา churn

## Recommendations​ 

- ใช้โมเดลเป็นระบบเตือนล่วงหน้า (Early Warning System)
- นำโมเดล Random Forest ไปใช้ระบุลูกค้าที่มีความเสี่ยงจะ Inactive เพื่อให้ทีม CRM ดำเนินการเชิงรุกก่อนเกิดการ churn จริง

- นำการทำนายแบบความน่าจะเป็นมาใช้ (Probability-based Decision)
ใช้ค่า predict_proba เพื่อจัดลำดับความเสี่ยงของลูกค้า แทนการตัดสินแบบไบนารี (Active/Inactive) เพียงอย่างเดียว

- กำหนด Threshold ร่วมกับฝ่ายธุรกิจ ทำงานร่วมกับผู้มีส่วนได้ส่วนเสียเพื่อกำหนดระดับความเสี่ยงที่เหมาะสมในการดำเนินการ เช่น โทรติดต่อ ส่งโปรโมชัน หรือแจ้งเตือนลูกค้า

- พัฒนาแคมเปญรักษาลูกค้าแบบเจาะจง (Targeted Retention)ส่งข้อเสนอหรือสิทธิพิเศษเฉพาะกลุ่มลูกค้าที่โมเดลระบุว่ามีความเสี่ยงสูง เพื่อลดต้นทุนและเพิ่มประสิทธิภาพของแคมเปญ

-ปรับปรุงโมเดลด้วย Feature Engineering เพิ่มเติมเพิ่มฟีเจอร์เชิงเวลาและแนวโน้มพฤติกรรม เช่น การเปลี่ยนแปลงของพฤติกรรม เพื่อเพิ่มประสิทธิภาพการทำนาย

-ใช้โมเดลเป็นเครื่องมือสนับสนุนการตัดสินใจ ไม่ใช่ระบบอัตโนมัติเต็มรูปแบบเนื่องจากประสิทธิภาพยังอยู่ในระดับปานกลาง ควรใช้โมเดลเพื่อช่วยจัดลำดับและสนับสนุนการตัดสินใจของทีมงาน
