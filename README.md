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
- **Acculturative Stress:** International students show **much higher acculturative stress** (avg 75.56) compared to domestic students (avg 62.84). This indicates that adapting to a new culture is a significant challenge and can contribute to mental health difficulties.  
- **Depression Levels:** Surprisingly, international students have slightly **lower average depression scores** (8.04) than domestic students (8.61). This may suggest that while cultural adjustment is stressful, international students might have coping strategies or resilience factors that mitigate depression.  
- **Social Connectedness:** Both groups report similar social connectedness (around 37), suggesting that social integration does not differ significantly between domestic and international students, despite stress differences.

### Gender Insights
- **Depression:** Female students report higher depression (avg 8.40) compared to males (avg 7.82).  
- **Acculturative Stress:** Female students also report higher stress (74.31 vs 69.04), indicating they may experience the adjustment challenges more intensely.  
- **Social Connectedness:** Male students report slightly higher social connectedness (38.19 vs 37.06), suggesting social networks may partially buffer stress and depression in male students.  
- **Interpretation:** Gender-specific interventions could help address these differences, especially for female students who may need more targeted mental health support.

### Correlation Insights
- **Depression & Acculturative Stress (r = +0.41):** A moderate positive correlation shows that students experiencing higher acculturative stress tend to have higher depression scores. This supports the idea that cultural adjustment is a key predictor of mental health difficulties.  
- **Depression & Social Connectedness (r = -0.54):** A moderate negative correlation indicates that higher social connectedness is associated with lower depression. Building strong social networks is therefore protective for student mental health.

### Length of Stay Insights
- **Early Years (1–3 years):** Depression and stress are highest in years 2–3 (avg depression 8.58–8.87, avg stress 71–75), indicating that the **initial adaptation phase is the most challenging**. Students may still be adjusting to the culture, academic expectations, and social environment.  
- **Mid to Later Years (4–6 years):** Social connectedness dips around year 4 (avg 35), while stress peaks in some small samples (up to 89). This may reflect academic pressures or isolation as students progress.  
- **Longer Stays:** Years 6–10 show inconsistent results due to very small sample sizes. However, some students report higher social connectedness (up to 48), suggesting that longer adaptation can improve social integration and reduce stress for some individuals.  
- **Takeaway:** The **length of stay impacts stress and depression**, but individual experience varies; social support and integration are key factors in long-term mental health outcomes.


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
