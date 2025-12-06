# KAGGLE-E2-INSIGHTS-INTO-STUDENT-PERFORMANCE

Predicting Outcomes using Machine Learning and Identifying Risk Profiles by Clustering  

**Project E2 – Authors:** Ann Marleen Varul, Karina Pinajeva, Jana Kotšnova  
**Repository:** [https://github.com/kkerychka/KAGGLE-E2-INSIGHTS-INTO-STUDENT-PERFORMANCE-](https://github.com/kkerychka/KAGGLE-E2-INSIGHTS-INTO-STUDENT-PERFORMANCE-)

---

## 1. Motivation & business understanding

Modern schools collect large amounts of digital information about students: grades, attendance, homework submission, extra-curricular activities and sometimes even socio-economic background. However, these data are rarely used proactively. Support for struggling students is usually reactive: teachers notice problems only after poor exam results, repeated failures or behavioural issues. At that point, the damage is often already done.<br>
Our project explores how predictive analytics can help schools move from reactive to preventive support. If we can estimate final exam performance in advance and detect patterns associated with academic risk, teachers and advisors can intervene earlier – for example by offering targeted tutoring, counselling, or simply talking to a student whose motivation is dropping. At the same time, we want to avoid “black box” tools and focus on models that educators can understand and discuss.<br>
Another important motivation is data quality and realism. Many student-performance datasets on platforms like Kaggle are synthetic, or heavily anonymised, but this is often not clearly communicated. As we discovered, such datasets can produce impressively high accuracy scores while hiding the fact that they contain almost no failing students and only minimal variation in exam scores. Models trained on such data look good in a report but say very little about real at-risk students in real schools.
To address this, we decided to work with two datasets:
* an artificial Kaggle dataset with 6,590 students, and
* a smaller but real dataset from the UCI Machine Learning Repository, containing final grades (including failures) from Portuguese secondary schools. Using the same modelling pipeline on both allows us to compare how model performance and interpretability change when we move from synthetic to real educational data.

From a business and policy perspective, our motivation is not only to build accurate models, but also to understand which factors matter and how they can inform decision-making. Which combinations of study habits, attendance, and family circumstances are associated with risk? Are there distinct groups of students who might benefit from different types of support? How can we turn model outputs into simple, personalised messages that students themselves can act on, rather than just technical scores for experts?

---

## 2. Project goals

- **Goal 1 – Regression (predict exam scores).**  
  Build regression models that predict each student’s final exam score from study habits, attendance, motivation, family background and well-being.

- **Goal 2 – Clustering (identify risk profiles).**  
  Use clustering (K-Means, Hierarchical, Fuzzy C-Means) to group students into high-performing, moderate-risk, and at-risk profiles based on academic and behavioural patterns.

- **Goal 3 – Language Model (LLM).**  
  Train a small language model that generates **short, personalised feedback messages** for students, based on their predicted risk level and feature profile.
---

## 3. Datasets and why we added a second dataset

### 3.1 Kaggle dataset – *StudentPerformanceFactors.csv*

Our main starting point is an open Kaggle dataset with **6,590 students** and the following feature groups:

- **Learning behaviour:** Hours_Studied, Attendance, Access_to_Resources, Motivation_Level  
- **Family & socio-economic:** Parental_Involvement, Family_Income, Parental_Education_Level  
- **Well-being:** Sleep_Hours, Physical_Activity, Learning_Disabilities  
- **School environment:** Teacher_Quality, School_Type, Peer_Influence, Distance_from_Home  
- **Performance:** Previous_Scores, Exam_Score  

Initially we planned to do **all** modelling on this dataset.  
However, during data understanding and modelling we discovered serious issues:

- Exam scores are squeezed into a very narrow band (≈ **55–100**, 90 % between **60–70**); there are **no failing students at all**.  
- Most features show very weak or artificial relationships to the exam score.  
- Clustering creates grid-like, geometric patterns that strongly suggest **synthetic, rule-based generation** rather than real student behaviour.

Because of this, the Kaggle dataset is excellent for demonstrating **how synthetic data can mislead models and inflate metrics**, but it is **not realistic enough** to study at-risk students.

### 3.2 UCI dataset – real student performance data

To obtain more realistic insights, we added a **second dataset** from the  
[[UCI Machine Learning Repository – “Student Performance”]](https://archive.ics.uci.edu/dataset/320/student+performance).  
It contains **real grade records** from two Portuguese secondary schools:

- Final grade **G3** on a **0–20** scale (with real failing grades, G3 < 10).  
- Demographic, family and behavioural variables similar to the Kaggle features  
  (parent education and jobs, studytime, failures, absences, higher, goout, famrel, freetime, Dalc, Walc, health, etc.).

We combine the Math and Portuguese subjects (1,044 records) and **intentionally exclude G1 and G2** when predicting G3 to avoid target leakage.

**Why the second dataset?**

- The Kaggle dataset lets models achieve very high R² (≈0.75) simply because the target varies little.  
  It cannot answer the original business question: *“Who is at risk of failing?”* – there are no failing students to predict.
- The UCI dataset contains **real failures, high performers and diverse patterns**, so regression and clustering are much more meaningful, even though the numerical metrics (R² ≈ 0.27 for the best model) are lower.
- Comparing both datasets on the same pipeline shows **how data quality and realism directly affect model performance and interpretability**.

In short, the second dataset was added to **validate our methods on real educational data** and to show that Kaggle results would otherwise **overstate** how well we can predict and detect at-risk students.

---

## 4. Tasks and workflow overview

We organise the work into the following tasks:

1. **Data quality assessment & preparation**  
   - Inspect missing values, outliers, and feature distributions.  
   - Encode categorical variables, impute missing entries, standardise numeric features.  

2. **Regression model development (Goal 1)**  
   - Compare Linear Regression, Ridge, Lasso, Random Forest and Gradient Boosting.  
   - Use train/test split (80/20) and 5-fold cross-validation.  
   - Evaluate with MAE, RMSE and R², and compare Kaggle vs UCI results.

3. **Clustering implementation (Goal 2)**  
   - Apply K-Means, Hierarchical and Fuzzy C-Means clustering.  
   - Evaluate cluster quality (silhouette, Davies–Bouldin) and interpret risk profiles.

4. **Language model training (Goal 3)**  
   - Fine-tune a small LLM to generate feedback messages for different risk levels and feature patterns.

5. **Model evaluation, integration and reporting (Goal 4)**  
   - Combine all results, visualisations and interpretation into a poster and this repository.

---

## 5. Repository guide & how to reproduce our analysis

1. **Clone the repository**

```bash
git clone https://github.com/kkerychka/KAGGLE-E2-INSIGHTS-INTO-STUDENT-PERFORMANCE-.git
cd KAGGLE-E2-INSIGHTS-INTO-STUDENT-PERFORMANCE-
