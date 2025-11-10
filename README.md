# 🎓 Analyzing the Impact of Studying Abroad on Student Mental Health

## 🧠 Project Overview
This project explores whether **studying abroad affects students’ mental health**.  
Using data from a **2018 Japanese international university survey**, the goal was to analyze how factors like **social connectedness**, **acculturative stress**, and **length of stay** influence **depression levels** among international students.

The dataset was collected ethically and includes student demographics, language proficiency, academic level, and standardized test scores (PHQ-9, SCS, ASISS).

---

## 📊 Objective
To use **PostgreSQL** to:
- Compare **international** vs **domestic** students’ mental health scores  
- Examine whether **length of stay** impacts mental health outcomes  
- Identify potential correlations between **depression**, **social connectedness**, and **acculturative stress**

---

## 🧩 Dataset Description

| Column Name     | Description |
| ---------------- | ------------ |
| `inter_dom`      | Student type: International (`Inter`) or Domestic (`Dom`) |
| `japanese_cate`  | Japanese language proficiency level |
| `english_cate`   | English language proficiency level |
| `academic`       | Academic level: Undergraduate or Graduate |
| `age`            | Age of the student |
| `stay`           | Length of stay in years |
| `todep`          | Depression score (PHQ-9 test) |
| `tosc`           | Social connectedness score (SCS test) |
| `toas`           | Acculturative stress score (ASISS test) |

---

## ⚙️ Tools & Technologies
- **PostgreSQL**
- **SQL** (data cleaning, aggregation, and analysis)

---

## 📈 SQL Analysis

### 1️⃣ Analyze Depression by Length of Stay (International Students)
```sql
SELECT stay,
       COUNT(*) AS count_int,
       ROUND(AVG(todep),2) AS average_phq,
       ROUND(AVG(tosc),2) AS average_scs,
       ROUND(AVG(toas),2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
