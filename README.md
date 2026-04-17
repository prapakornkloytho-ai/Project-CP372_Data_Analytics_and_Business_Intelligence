# Azure Stay CP372 Final Project

โปรเจ็กต์นี้จัดทำสำหรับวิชา **CP372 Data Analytics and Business Intelligence**  
หัวข้อ: **Channel Profitability & Distribution Cost Analysis for Azure Stay**

งานชุดนี้ถูกออกแบบให้ **สอดคล้องกับ 2 งานเดิมของทีม**
- `Project Canvas.pptx` : แนวคิดการใช้ข้อมูลเพื่อแก้ปัญหาธุรกิจของโรงแรม Azure Stay
- `HW3_Hotel_Descriptive_Diagnostic.xlsx` : โครงข้อมูลการจองเดิมของโรงแรมที่ใช้เป็นฐานในการต่อยอด

โปรเจ็กต์นี้จึงเป็นการ **ต่อยอดจากโลกข้อมูลเดิม** แต่เปลี่ยน focus จากปัญหา Revenue Stagnation ไปเป็น **High Distribution Costs / Commission Trap**

---

## 1) Business Problem

Azure Stay มียอดจองเข้ามาอย่างต่อเนื่อง แต่กำไรสุทธิลดลงเพราะต้นทุนค่าคอมมิชชันจากช่องทาง OTA สูงเกินไป  
โจทย์หลักคือหาให้ชัดว่า **ช่องทางไหนสร้าง Net Revenue จริง** ไม่ใช่ดูแค่ยอดจองหรือ Gross Revenue

---

## 2) SMART Objective

- **Specific:** ลดการพึ่งพา OTA ที่มีค่าคอมมิชชันสูง และเพิ่มสัดส่วน Direct / Low-cost channels
- **Measurable:** เพิ่ม Net RevPAR 10% และลด commission cost ratio ลง 5%
- **Achievable:** ใช้ข้อมูล booking + channel + rate code + marketing spend เพื่อปรับ channel mix
- **Relevant:** สอดคล้องกับเป้าหมายการเพิ่มกำไรสุทธิและควบคุม acquisition cost
- **Time-bound:** วัดผลภายใน 1 ไตรมาส

---

## 3) Project Structure

```text
azure_stay_cp372_project/
├─ Azure_Stay_CP372_Dataset.xlsx
├─ data/
│  ├─ fact_bookings.csv
│  ├─ dim_channels.csv
│  ├─ dim_rate_codes.csv
│  └─ fact_marketing_spend.csv
├─ notebooks/
│  └─ azure_stay_cp372_analysis.ipynb
└─ README.md
```

---

## 4) Dataset Design

### Table 1: `fact_bookings`
ธุรกรรมการจองหลัก
- `booking_id`
- `booking_date`
- `check_in_date`
- `channel_id`
- `rate_code_id`
- `gross_room_revenue`
- `commission_amount`
- `net_room_revenue`
- `status`

### Table 2: `dim_channels`
นิยามต้นทุนของแต่ละช่องทาง
- `channel_id`
- `channel_name`
- `channel_type`
- `commission_model`
- `default_commission_rate`
- `contract_owner`

### Table 3: `dim_rate_codes`
นิยามประเภทเรตราคา
- `rate_code_id`
- `rate_name`
- `is_commissionable`

### Table 4: `fact_marketing_spend`
งบการตลาดของ Direct Website
- `spend_id`
- `spend_date`
- `channel_id`
- `platform`
- `cost_amount`
- `clicks`

---

## 5) Key Hypotheses

1. **Direct มี Net ADR สูงกว่า OTA**
2. **Flat Fee คุ้มกว่าการหักแบบ Percentage**
3. **Promotional Rate มี Cancellation Rate สูง**
4. **Direct Marketing ยังต้อง optimize ก่อน scale budget เพิ่ม**

---

## 6) Main Results

### KPI 1: Net ADR by Channel Type
| Channel Type | Net ADR |
|---|---:|
| Direct | 7,351.33 |
| OTA | 5,692.74 |
| B2B | 5,293.91 |

**Result:** Direct สูงกว่า OTA อยู่ **1,658.59 ต่อ booking**

### KPI 2: Commission Cost %
| Commission Model | Cost % |
|---|---:|
| Flat Fee | 0.29% |
| Percentage | 18.90% |
| Net Rate | 8.00% |

**Result:** Percentage model กิน margin สูงที่สุดอย่างชัดเจน

### KPI 3: Cancellation Risk
- ช่องทางที่ cancel สูงสุด: **Expedia (18.90%)**
- Rate code ที่ cancel สูงสุด: **Promotional Rate (26.97%)**

### KPI 4: Direct Marketing Efficiency
- Total spend: **2,021,332.64**
- Total clicks: **227,104**
- Non-cancelled direct bookings: **1,543**
- Average marketing cost per direct booking: **1,310.00**

**Interpretation:** ต้นทุนต่อ direct booking ยังไม่ได้ต่ำกว่า OTA commission มากพอ จึงควร optimize conversion ก่อนเพิ่ม budget

---

## 7) Insights

### Insight 1: The Commission Trap
แม้ OTA จะสร้าง volume ได้ดี แต่เมื่อหัก commission แล้ว Net ADR ต่ำกว่า Direct อย่างชัดเจน  
จึงไม่ควรใช้ยอดจองรวมเป็นตัวชี้วัดเดียว

### Insight 2: Promotional Rate Drives Cancellations
Promotional Rate มี cancellation สูงที่สุด สะท้อนพฤติกรรมจองกั๊กและ free cancellation  
ทำให้โรงแรมเสียโอกาสขายห้องในราคาที่ดีกว่า

### Insight 3: Direct Is Better, But Marketing Must Be Optimized
Direct channel ให้ margin ดีกว่า แต่ต้นทุนการตลาดยังสูง  
ดังนั้นการย้าย share จาก OTA ไป Direct ต้องทำคู่กับการปรับ UX / funnel / campaign targeting

---

## 8) Recommendations

### 1. Shift booking mix toward Direct and low-cost channels
- เพิ่ม incentive สำหรับ direct booking
- จำกัด OTA inventory ในช่วง demand สูง
- ดัน Walk-in / Corporate / Travel Agent ที่ cost ต่ำกว่า

### 2. Tighten promotional cancellation policy
- เปลี่ยน Promotional Rate เป็น non-refundable หรือ deposit required
- ลด free cancellation ในช่วง high season / high demand

### 3. Improve direct booking conversion
- ทำ A/B testing หน้า booking engine
- วิเคราะห์ conversion แยกตามแพลตฟอร์มโฆษณา
- ใช้ value-added perks แทนส่วนลดแรง ๆ

---

## 9) How to Run

### Python / Colab
เปิดไฟล์ `notebooks/azure_stay_cp372_analysis.ipynb` แล้วรันทีละเซลล์

### Tableau
สามารถ import จากไฟล์เหล่านี้ได้ทันที:
- `fact_bookings.csv`
- `dim_channels.csv`
- `dim_rate_codes.csv`
- `fact_marketing_spend.csv`

### Suggested Tableau Dashboards
1. **Channel Profitability Dashboard**
   - Gross Revenue by Channel
   - Net Revenue by Channel
   - Net ADR by Channel Type

2. **Commission Model Dashboard**
   - Commission Amount by Channel
   - Cost % by Commission Model

3. **Cancellation Dashboard**
   - Cancellation Rate by Channel
   - Cancellation Rate by Rate Code

4. **Direct Marketing Dashboard**
   - Spend by Platform
   - Clicks by Platform
   - Cost per Direct Booking by Month

---

## 10) Notes

- ชุดข้อมูลนี้ถูกสร้างให้ **เชื่อมต่อกับงาน HW3 เดิม** โดยใช้โครงธุรกิจโรงแรม Azure Stay และฐานข้อมูลการจองเป็นต้นทาง
- มีการปรับ schema ให้ตรงตามโจทย์ final project โดยเพิ่มมิติ **channel cost**, **rate code**, และ **marketing spend**
- เหมาะสำหรับใช้ทั้งใน **Excel, Tableau, และ Colab**

