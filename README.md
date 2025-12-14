
**FoodPanda: วิเคราะห์ข้อมูลลูกค้า, Churn และการทำนายด้วย Machine Learning
โปรเจกต์นี้เป็นงาน Exploratory Data Analysis (EDA) + Statistical Testing + Machine Learning
 โดยใช้ข้อมูลแพลตฟอร์ม Food Delivery (FoodPanda-like dataset) เพื่อศึกษาพฤติกรรมลูกค้า
 วิเคราะห์ปัจจัยที่ส่งผลต่อ Churn (การเลิกใช้งาน) และสร้างโมเดลทำนาย**
 
 ---
**- วัตถุประสงค์ของโครงงาน**
วิเคราะห์รูปแบบการใช้จ่ายและพฤติกรรมการสั่งอาหาร


เปรียบเทียบความแตกต่างระหว่างลูกค้า Active vs Inactive (Churn)


ทดสอบสมมติฐานเชิงสถิติ (T-test)


คัดเลือก Feature สำคัญที่สัมพันธ์กับ Churn


สร้างโมเดล Logistic Regression  ,Random Forest ,XGBoost,LightGBM  เพื่อทำนายโอกาส Churn


---
**- ภาพรวมชุดข้อมูล**

** แหล่งที่มาของข้อมูล **
https://www.kaggle.com/datasets/nabihazahid/foodpanda-analysis-dataset-2025 

**จำนวนข้อมูล: 6,000 ออเดอร์**


**จำนวนตัวแปร: 20 ตัวแปร**
<img width="1171" height="605" alt="image" src="https://github.com/user-attachments/assets/88460a82-6148-4d8f-9db3-4a4a35cf1199" />



---
**ตัวแปรสำคัญ**


ราคา (price), จำนวน (quantity)


ความถี่การสั่ง (order_frequency)


คะแนนสะสม (loyalty_points)


ระยะห่างจากการสั่งล่าสุด (days_since_last_order)


สถานะลูกค้า (churned: Active / Inactive)


คะแนนรีวิว (rating)


สถานะการจัดส่ง (delivery_status)


---
**การตรวจสอบและทำความสะอาดข้อมูล**


ตรวจสอบชนิดข้อมูลและค่า missing (ไม่พบ missing values)


จัดการค่า Null


ลบ duplicates


แปลงประเภทข้อมูล (date → datetime, string → numeric)


เตรียมข้อมูลให้อยู่ในรูปแบบพร้อมวิเคราะห์และทำโมเดล


---
**Descriptive Statistics**

**ผลสถิติพื้นฐานของตัวแปรเชิงตัวเลข: **

<img width="359" height="290" alt="{BE2B4D9B-3322-4F92-96AA-A8E6BC66FA5F}" src="https://github.com/user-attachments/assets/aebc4cbd-a0d0-49d8-97c1-eb620aeae2d2" />


จากการวิเคราะห์สถิติเชิงพรรณนา พบว่าลูกค้าส่วนใหญ่มักสั่งอาหารเฉลี่ยประมาณ 3 รายการต่อออเดอร์ โดยมีมูลค่าเฉลี่ยประมาณ 800 บาท
ข้อมูลแสดงให้เห็นความแตกต่างของพฤติกรรมการใช้จ่าย และพบว่าตัวแปร days_since_last_order มีค่าเฉลี่ยสูง ซึ่งสะท้อนถึงความเสี่ยงในการเกิด churn

---



```python
from scipy.stats import norm

# ค่าเฉลี่ยและส่วนเบี่ยงเบน
mean_active = active_sales.mean()
mean_inactive = inactive_sales.mean()

std_active = active_sales.std()
std_inactive = inactive_sales.std()

n_active = active_sales.shape[0]
n_inactive = inactive_sales.shape[0]

# Standard Error
se_active = std_active / np.sqrt(n_active)
se_inactive = std_inactive / np.sqrt(n_inactive)

# ===============================
# สร้างแกน x สำหรับกราฟ
# ===============================
x_min = min(mean_active - 4*se_active, mean_inactive - 4*se_inactive)
x_max = max(mean_active + 4*se_active, mean_inactive + 4*se_inactive)
x = np.linspace(x_min, x_max, 500)

# ===============================
# Normal Distribution
# ===============================
y_active = norm.pdf(x, mean_active, se_active)
y_inactive = norm.pdf(x, mean_inactive, se_inactive)

# ===============================
# Plot
# ===============================
plt.figure(figsize=(8,5))
plt.plot(x, y_active, label='Active (Sampling Distribution)')
plt.plot(x, y_inactive, label='Inactive (Sampling Distribution)')
plt.axvline(mean_active, linestyle='--', label='Active Mean')
plt.axvline(mean_inactive, linestyle='--', label='Inactive Mean')
plt.title('T-Test Explanation: Sampling Distribution of Mean Price')
plt.xlabel('Mean Price')
plt.ylabel('Density')
plt.legend()
plt.show()
```

---
<img width="730" height="324" alt="{AF1CC214-1CFC-43A2-ABAE-271CF1016DD9}" src="https://github.com/user-attachments/assets/d3c01756-f4e3-4d63-b23c-69d8494941b0" />

The Test 
การทดสอบนี้ใช้ Independent two-sample, two-tailed t-test

เพื่อเปรียบเทียบ ค่าเฉลี่ยยอดขาย (price) ระหว่างกลุ่มลูกค้า Active และ Inactive

หากค่า p-value < α จะปฏิเสธ H₀

The Result
T-statistic: -0.7969206171066529
P-value: 0.4255293665108075

เนื่องจาก p-value (0.426) > 0.05 จึงไม่ปฏิเสธ H₀ และไม่พบความแตกต่างของยอดขายเฉลี่ยระหว่างลูกค้า Active และ Inactive อย่างมีนัยสำคัญ

Churn ไม่ได้ทำให้ยอดซื้อต่อออเดอร์แตกต่างอย่างมีนัยสำคัญ

```python
# correlation matrix
corr = numeric_cols.corr()

plt.figure(figsize=(12, 8))
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt=".2f")
plt.title("Correlation Heatmap of Numeric Features")
plt.show()
```
<img width="515" height="403" alt="{87984070-BD64-4254-AC00-4B2ECD79CE84}" src="https://github.com/user-attachments/assets/5c4355da-1279-4197-83a3-709e6f0727c4" />

Correlation Heatmap (Numeric Features)
ตัวแปรเชิงตัวเลขส่วนใหญ่มีค่า correlation ใกล้ 0
quantity กับ price_per_item มีความสัมพันธ์เชิงลบสูง (≈ -0.66)
price กับ price_per_item มีความสัมพันธ์เชิงบวกปานกลาง (≈ 0.58)
ตัวแปรที่เกี่ยวข้องกับ churn (order_frequency, days_since_last_order, loyalty_points)
 ไม่มีความสัมพันธ์เชิงเส้นที่ชัดเจนกับตัวแปรรายได้
Insight
พฤติกรรมการซื้อมีความซับซ้อนและไม่เป็นเชิงเส้น
ลูกค้าที่สั่งจำนวนมากมักเลือกสินค้าราคาต่อชิ้นต่ำ
มูลค่าการใช้จ่ายต่อออเดอร์ไม่ใช่ตัวขับเคลื่อนหลักของ churn
Churn มีแนวโน้มถูกอธิบายได้ดีกว่าด้วยพฤติกรรมเชิงเวลาและความถี่ มากกว่ารายได้เพียงอย่างเดียว



