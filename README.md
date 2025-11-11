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
