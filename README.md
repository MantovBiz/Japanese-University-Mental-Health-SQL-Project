# 🧠 Exploring Student Mental Health and Cultural Adjustment

## 📋 Project Overview
This project explores whether studying abroad affects university students’ mental health using a dataset from a **Japanese international university**.  
The goal was to use **PostgreSQL** to analyze whether international students experience higher levels of depression and stress compared to domestic students, and whether **length of stay** or **gender** plays a role in these patterns.

---

## 🗂️ Dataset Description

| Column Name     | Description |
|-----------------|--------------|
| `inter_dom`     | Student type: International or Domestic |
| `japanese_cate` | Japanese language proficiency |
| `english_cate`  | English language proficiency |
| `academic`      | Academic level: Undergraduate or Graduate |
| `age`           | Age of the student |
| `stay`          | Length of stay in Japan (years) |
| `todep`         | Total depression score (PHQ-9 test) |
| `tosc`          | Total social connectedness score (SCS test) |
| `toas`          | Total acculturative stress score (ASISS test) |

---

## 🧩 SQL Exploration
All analysis was performed with PostgreSQL.

### 1️⃣ Compare International vs. Domestic Students
```sql
SELECT inter_dom,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY inter_dom;
```
### 2️⃣ Comparison by Gender
```sql
SELECT gender,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY gender;
```
### 3️⃣ Correlation Analysis
```sql
SELECT 
    ROUND(CAST(CORR(todep, toas) AS numeric), 2) AS dep_stress_corr,
    ROUND(CAST(CORR(todep, tosc) AS numeric), 2) AS dep_social_corr
FROM students
WHERE inter_dom = 'Inter';
```
### 4️⃣ Length of Stay Analysis
```sql
SELECT stay,
       COUNT(*) AS num_students,
       ROUND(AVG(todep),2) AS avg_depression,
       ROUND(AVG(tosc),2) AS avg_social,
       ROUND(AVG(toas),2) AS avg_stress
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay;
```
## 💡 Insights

### Student Type Insights
- International students experience **higher acculturative stress** than domestic students.  
- Depression scores are slightly lower for international students.  
- Social connectedness is similar across both groups, indicating social integration may be consistent.

### Gender Insights
- Female students report **higher depression and stress** than male students.  
- Male students report slightly higher social connectedness.  
- Suggests a need for **gender-sensitive interventions** to better support student mental health.

### Correlation Insights
- **Depression ↔ Acculturative Stress:** Positive correlation (+0.41) → higher stress is associated with higher depression.  
- **Depression ↔ Social Connectedness:** Negative correlation (-0.54) → stronger social support is associated with lower depression.

### Length of Stay Insights
- Depression and stress **peak during years 2–3**, the most challenging adjustment period.  
- Social connectedness is lowest in **year 4**, coinciding with increased stress.  
- Longer stays do not automatically reduce stress or depression; adaptation depends on multiple factors.

---

## 📈 Key Takeaways
- Middle years abroad (2–4) are the most stressful for students.  
- Social support significantly reduces depression.  
- Female students show slightly higher stress; gender-sensitive programs are recommended.  
- Acculturative stress is a strong predictor of depression.

---

## 🧰 Tools Used
- **PostgreSQL** for data querying and analysis  
- SQL functions: `AVG()`, `COUNT()`, `CORR()`, `ROUND()`, `CAST()`  
- Data source: 2018 Japanese International University mental health survey  

---

## 📄 Summary
This project demonstrates that **mental health challenges for international students** are influenced by acculturative stress, social connectedness, gender, and length of stay.  
The findings emphasize the importance of social support and targeted interventions during critical adjustment periods abroad.  
These insights can inform programs and policies to support the well-being of students studying in a foreign country.
