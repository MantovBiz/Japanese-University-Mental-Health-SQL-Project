🧠 Exploring Student Mental Health and Cultural Adjustment
📋 Project Overview

This project explores whether studying abroad affects university students’ mental health — based on a dataset from a Japanese international university.
The goal was to use PostgreSQL to analyze whether international students experience higher levels of depression and stress compared to domestic students, and whether length of stay or gender plays a role in these patterns.

🗂️ Dataset Description
Column Name	Description
inter_dom	Student type: International or Domestic
japanese_cate	Japanese language proficiency
english_cate	English language proficiency
academic	Academic level: Undergraduate or Graduate
age	Age of the student
stay	Length of stay in Japan (years)
todep	Total depression score (PHQ-9 test)
tosc	Total social connectedness score (SCS test)
toas	Total acculturative stress score (ASISS test)
🧩 SQL Exploration

The analysis was done entirely in PostgreSQL.

1️⃣ Compare International vs. Domestic Students
SELECT inter_dom,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY inter_dom;


Results:

Type	Avg Depression	Avg Social	Avg Stress
Domestic	8.61	37.64	62.84
International	8.04	37.42	75.56

Insights:

International students have higher acculturative stress (75.6 vs. 62.8) than domestic peers.

Depression scores are similar, though slightly lower for international students.

Social connectedness remains consistent between groups.

🟩 Interpretation:
International students likely experience greater cultural adjustment stress, supporting the idea that adapting to a new environment significantly affects well-being.

2️⃣ Gender-Based Analysis
SELECT gender,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY gender;


Results:

Gender	Avg Depression	Avg Social	Avg Stress
Male	7.82	38.19	69.04
Female	8.40	37.06	74.31

Insights:

Female students reported higher average depression and stress than males.

Male students scored slightly higher on social connectedness.

🟩 Interpretation:
This could suggest that female students face more emotional and adjustment-related challenges, highlighting the need for gender-sensitive support programs.

3️⃣ Correlation Analysis
SELECT 
    ROUND(CAST(CORR(todep, toas) AS numeric), 2) AS dep_stress_corr,
    ROUND(CAST(CORR(todep, tosc) AS numeric), 2) AS dep_social_corr
FROM students
WHERE inter_dom = 'Inter';


Results:

Depression ↔ Stress correlation: +0.41

Depression ↔ Social connectedness correlation: –0.54

Interpretation:

A positive correlation (+0.41) means higher stress tends to come with higher depression levels.

A negative correlation (–0.54) means higher social connectedness is linked to lower depression — suggesting that social support is a protective factor.

4️⃣ Length of Stay Analysis
SELECT stay,
       COUNT(*) AS num_students,
       ROUND(AVG(todep),2) AS avg_depression,
       ROUND(AVG(tosc),2) AS avg_social,
       ROUND(AVG(toas),2) AS avg_stress
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay;


Results:

Stay (Years)	Students	Avg Depression	Avg Social	Avg Stress
1	95	7.70	37.94	71.03
2	39	8.58	37.08	74.87
3	46	8.87	37.78	71.35
4	14	7.96	35.00	78.74
5	1	7.67	34.00	89.00
6	3	6.00	38.00	58.67
7	1	4.00	48.00	45.00
8	1	10.00	44.00	65.00
10	1	13.00	32.00	50.00
📈 Key Takeaways

Adjustment Period:
Stress and depression tend to increase between years 1–4, suggesting that the middle phase of studying abroad may be the most mentally challenging.

Social Connection:
Lower social connectedness correlates with higher depression levels, showing how critical community and belonging are for student well-being.

Gender Differences:
Female students show slightly higher depression and stress levels, indicating the potential value of targeted emotional and cultural support.

Length of Stay Effects:
Longer stays don’t always mean lower stress — it may depend on how well students adapt socially and academically over time.

🧭 Recommendations for Further Research

Include standard deviation or median values to measure variability within each group.

Visualize trends with line charts (e.g., avg stress vs. stay years).

Run correlation between stay and depression/stress:

SELECT CORR(stay, todep) AS stay_depression_corr,
       CORR(stay, toas) AS stay_stress_corr
FROM students
WHERE inter_dom = 'Inter';


Explore combined factors: e.g., correlation by gender and length of stay.

Qualitative follow-up: Conduct interviews or surveys to understand the why behind stress and depression levels.

🧰 Tools Used

PostgreSQL for data querying and correlation analysis

SQL functions: AVG(), COUNT(), CORR(), ROUND(), CAST()

Data source: 2018 Japanese International University mental health study

💡 Summary

This project highlights how mental health challenges for international students are multifaceted — shaped by cultural stress, social connectedness, and time spent abroad.
SQL was used to uncover meaningful relationships that can guide universities toward better mental health and integration support for their global student populations.
