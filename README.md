<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Exploring Student Mental Health and Cultural Adjustment</title>
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; line-height: 1.6; color: #0f172a; margin: 24px; background: #f8fafc; }
    .container { max-width: 900px; margin: 0 auto; background: #ffffff; padding: 28px; border-radius: 12px; box-shadow: 0 6px 18px rgba(2,6,23,0.08); }
    h1 { font-size: 28px; margin-bottom: 8px; }
    h2 { font-size: 20px; margin-top: 20px; margin-bottom: 8px; color: #0b1220; }
    h3 { font-size: 16px; margin-top: 16px; margin-bottom: 6px; color: #0b1220; }
    p { margin: 8px 0; color: #0b1220; }
    table { width: 100%; border-collapse: collapse; margin: 12px 0; }
    th, td { border: 1px solid #e6ecf0; padding: 8px 10px; text-align: left; }
    th { background: #f1f5f9; color: #0b1220; }
    code, pre { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Courier New", monospace; font-size: 13px; background: #0f172a05; padding: 6px; border-radius: 6px; }
    pre { padding: 12px; overflow: auto; border: 1px solid #e6ecf0; background: #fbfdff; }
    .badge { display:inline-block; background:#eef2ff; color:#3730a3; padding:4px 8px; border-radius:6px; font-weight:600; margin-right:6px; font-size:13px; }
    .note { background:#fff8e6; border-left:4px solid #ffd166; padding:10px; border-radius:6px; margin:12px 0; }
    .cta { margin-top:18px; padding:12px; background:#eef2f3; border-radius:8px; }
    footer { margin-top:28px; font-size:13px; color:#334155; }
  </style>
</head>
<body>
  <div class="container">
    <h1>🧠 Exploring Student Mental Health and Cultural Adjustment</h1>

    <p><strong>Project Overview</strong><br>
      This project explores whether studying abroad affects university students’ mental health using a dataset from a Japanese international university. The objective was to use <span class="badge">PostgreSQL</span> to analyze differences between international and domestic students, and to investigate the effects of <em>length of stay</em> and <em>gender</em> on depression, social connectedness, and acculturative stress.</p>

    <hr />

    <h2>📂 Dataset Description</h2>
    <table>
      <thead>
        <tr><th>Column Name</th><th>Description</th></tr>
      </thead>
      <tbody>
        <tr><td><code>inter_dom</code></td><td>Student type: International or Domestic</td></tr>
        <tr><td><code>japanese_cate</code></td><td>Japanese language proficiency</td></tr>
        <tr><td><code>english_cate</code></td><td>English language proficiency</td></tr>
        <tr><td><code>academic</code></td><td>Academic level: Undergraduate or Graduate</td></tr>
        <tr><td><code>age</code></td><td>Age of the student</td></tr>
        <tr><td><code>stay</code></td><td>Length of stay in Japan (years)</td></tr>
        <tr><td><code>todep</code></td><td>Total depression score (PHQ-9 test)</td></tr>
        <tr><td><code>tosc</code></td><td>Total social connectedness score (SCS test)</td></tr>
        <tr><td><code>toas</code></td><td>Total acculturative stress score (ASISS test)</td></tr>
      </tbody>
    </table>

    <h2>🧩 SQL Exploration</h2>
    <p>All analysis was performed with PostgreSQL queries. Example queries and results are shown below.</p>

    <h3>1. Compare International vs. Domestic Students</h3>
    <pre><code>SELECT inter_dom,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY inter_dom;</code></pre>

    <p><strong>Results:</strong></p>
    <table>
      <thead>
        <tr><th>Type</th><th>Avg Depression</th><th>Avg Social</th><th>Avg Stress</th></tr>
      </thead>
      <tbody>
        <tr><td>Domestic</td><td>8.61</td><td>37.64</td><td>62.84</td></tr>
        <tr><td>International</td><td>8.04</td><td>37.42</td><td>75.56</td></tr>
      </tbody>
    </table>

    <p><strong>Interpretation:</strong> International students show notably higher acculturative stress (75.56 vs 62.84). Depression scores are similar, and social connectedness is roughly the same between groups — suggesting cultural adjustment specifically increases stress.</p>

    <h3>2. Gender-Based Analysis</h3>
    <pre><code>SELECT gender,
    ROUND(AVG(todep),2) AS avg_depression,
    ROUND(AVG(tosc),2) AS avg_social,
    ROUND(AVG(toas),2) AS avg_stress
FROM students
GROUP BY gender;</code></pre>

    <p><strong>Results:</strong></p>
    <table>
      <thead>
        <tr><th>Gender</th><th>Avg Depression</th><th>Avg Social</th><th>Avg Stress</th></tr>
      </thead>
      <tbody>
        <tr><td>Male</td><td>7.82</td><td>38.19</td><td>69.04</td></tr>
        <tr><td>Female</td><td>8.40</td><td>37.06</td><td>74.31</td></tr>
      </tbody>
    </table>

    <p><strong>Interpretation:</strong> Female students report slightly higher depression and stress and slightly lower social connectedness — indicating a potential need for gender-sensitive support.</p>

    <h3>3. Correlation Analysis</h3>
    <pre><code>SELECT
    ROUND(CAST(CORR(todep, toas) AS numeric), 2) AS dep_stress_corr,
    ROUND(CAST(CORR(todep, tosc) AS numeric), 2) AS dep_social_corr
FROM students
WHERE inter_dom = 'Inter';</code></pre>

    <p><strong>Results:</strong></p>
    <ul>
      <li><strong>Depression ↔ Stress:</strong> +0.41 (moderate positive)</li>
      <li><strong>Depression ↔ Social connectedness:</strong> −0.54 (moderate negative)</li>
    </ul>

    <p><strong>Interpretation:</strong> Higher acculturative stress is associated with higher depression; higher social connectedness is associated with lower depression.</p>

    <h3>4. Length of Stay Analysis (International Students)</h3>
    <pre><code>SELECT stay,
       COUNT(*) AS num_students,
       ROUND(AVG(todep),2) AS avg_depression,
       ROUND(AVG(tosc),2) AS avg_social,
       ROUND(AVG(toas),2) AS avg_stress
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay;</code></pre>

    <p><strong>Results:</strong></p>
    <table>
      <thead>
        <tr><th>Stay (Years)</th><th>Students</th><th>Avg Depression</th><th>Avg Social</th><th>Avg Stress</th></tr>
      </thead>
      <tbody>
        <tr><td>1</td><td>95</td><td>7.70</td><td>37.94</td><td>71.03</td></tr>
        <tr><td>2</td><td>39</td><td>8.58</td><td>37.08</td><td>74.87</td></tr>
        <tr><td>3</td><td>46</td><td>8.87</td><td>37.78</td><td>71.35</td></tr>
        <tr><td>4</td><td>14</td><td>7.96</td><td>35.00</td><td>78.74</td></tr>
        <tr><td>5</td><td>1</td><td>7.67</td><td>34.00</td><td>89.00</td></tr>
        <tr><td>6</td><td>3</td><td>6.00</td><td>38.00</td><td>58.67</td></tr>
        <tr><td>7</td><td>1</td><td>4.00</td><td>48.00</td><td>45.00</td></tr>
        <tr><td>8</td><td>1</td><td>10.00</td><td>44.00</td><td>65.00</td></tr>
        <tr><td>10</td><td>1</td><td>13.00</td><td>32.00</td><td>50.00</td></tr>
      </tbody>
    </table>

    <div class="note">
      <strong>Note:</strong> Years 1–4 have reasonable sample sizes and are most reliable. Years ≥5 have very small sample sizes (1–3 students) and should be interpreted cautiously — they may reflect outliers or individual cases rather than population trends.
    </div>

    <h2>📈 Key Takeaways</h2>
    <ol>
      <li><strong>Adjustment period:</strong> Stress and depression increase across years 1–3 (peaking in the mid-stay period), suggesting the middle phase of studying abroad is particularly challenging.</li>
      <li><strong>Social connection matters:</strong> Higher social connectedness is linked to lower depression (r = −0.54), indicating community and belonging are protective.</li>
      <li><strong>Gender differences:</strong> Female students show slightly higher depression and stress; gender-sensitive programs could help.</li>
      <li><strong>Stay effects:</strong> Longer stay does not guarantee lower stress — adaptation appears complex and may depend on social factors and academic pressures.</li>
    </ol>

    <h2>🧭 Recommendations & Next Steps</h2>
    <ul>
      <li><strong>Compute variability:</strong> Add standard deviation or median to each group to understand within-group spread (e.g., <code>STDDEV_POP(todep)</code>).</li>
      <li><strong>Visualize trends:</strong> Create line charts (avg depression / avg stress vs. stay years) to highlight patterns and show confidence intervals where possible.</li>
      <li><strong>Statistical checks:</strong> Test correlations between <code>stay</code> and <code>todep</code>/<code>toas</code> and run subgroup analyses (e.g., by gender or academic level).</li>
      <li><strong>Filter small groups:</strong> For robust conclusions, focus on years with n &gt; 10 or explicitly flag small-sample years in results.</li>
      <li><strong>Qualitative follow-up:</strong> Interviews or focus groups could explain <em>why</em> stress rises in years 2–4 (academic stressors, diminishing support, language challenges, etc.).</li>
    </ul>

    <h2>🧰 Tools Used</h2>
    <p><strong>PostgreSQL</strong> for querying and correlation analysis. SQL features used include <code>AVG()</code>, <code>COUNT()</code>, <code>CORR()</code>, <code>ROUND()</code>, and <code>CAST()</code>.</p>

    <h2>💡 Summary</h2>
    <p>This project demonstrates how cultural stress, social connectedness, and time abroad interact to shape mental health outcomes for international students. The findings suggest targeted, ongoing support — especially during the middle years of study — and highlight social integration as a key protective factor. The analysis also shows how SQL can be used to quickly surface meaningful research insights from survey data.</p>

    <div class="cta">
      <strong>Want a shorter portfolio blurb or a LinkedIn-friendly summary?</strong>
      <p>Tell me the tone you want (formal, casual, or punchy) and I’ll write a 2–3 sentence version you can paste directly into your profiles.</p>
    </div>

    <footer>
      <p>Author: Manuel Tovar • DePaul University<br>Data source: 2018 Japanese International University mental health survey (used for educational analysis)</p>
    </footer>
  </div>
</body>
</html>
