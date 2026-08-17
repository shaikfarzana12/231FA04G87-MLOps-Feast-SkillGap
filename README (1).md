CLA 1 — Curriculum-Industry Skill-Gap Feature Store Using Feast

Student Details
- Name: SHAIK FARZANA
- Register Number: 231FA04G87
- Section: 15

 Problem Statement
This project converts a CSE student skill-gap dataset into a simple Feast-based feature store. The objective is to demonstrate feature engineering, Feast entity creation, data-source creation, FeatureView creation, registration using `feast apply`, historical feature retrieval, materialization into an online store, online feature retrieval, and use of Feast features in a machine-learning model.

Dataset
The supplied dataset contains 150 student records and 15 original columns.

Target: `Skill_Gap_Label`

Target values:
- `Yes` = student has a skill gap
- `No` = student does not have a skill gap

Original columns:
- Student_ID
- Specialization
- CGPA
- Programming_Skill_Score
- Communication_Skill_Score
- Certifications_Count
- Internship_Experience
- Projects_Completed
- DSA_Platform_Rating
- Placement_Training_Attended
- Soft_Skills_Score
- Curriculum_Coverage_Percent
- Industry_Demand_Score
- Curriculum_Industry_Alignment_Score
- Skill_Gap_Label

The dataset has 150 non-null records in every original column, so no missing values were found. The target is balanced: 75 `Yes` and 75 `No`.

Feature Engineering

Three engineered features were created:

1. `Internship_Flag`: Yes = 1, No = 0.
2. `Placement_Training_Flag`: Yes = 1, No = 0.
3. `Experience_Score`: a 0–100 score combining internship, placement training, projects, and certifications.

Formula:

`Experience_Score = Internship_Flag*40 + Placement_Training_Flag*30 + (Projects_Completed/8)*20 + (Certifications_Count/6)*10`

The target `Skill_Gap_Label` is not included in the FeatureView because it is the prediction target.

 Feast Entity

Entity: `student`

Join key: `student_id`

Each student has a unique Student ID, so `student_id` is used to retrieve features.

Data Source

The feature data is stored in:

`data/student_features.parquet`

The Parquet file is used as the local batch/offline data source.

The original dataset does not contain a real event timestamp. Therefore, synthetic daily timestamps beginning on 2025-01-01 were added only to demonstrate Feast's time-aware historical retrieval and materialization workflow.

FeatureView

FeatureView name:

`student_skill_features`

Features:
- Specialization
- CGPA
- Programming_Skill_Score
- Communication_Skill_Score
- Certifications_Count
- Internship_Flag
- Projects_Completed
- DSA_Platform_Rating
- Placement_Training_Flag
- Soft_Skills_Score
- Curriculum_Coverage_Percent
- Industry_Demand_Score
- Curriculum_Industry_Alignment_Score
- Experience_Score

Architecture

Original Student Dataset
          ↓
Preprocessing + Feature Engineering
          ↓
Parquet Offline Data
          ↓
Feast Entity + Data Source + FeatureView
          ↓
      feast apply
          ↓
  ┌───────────────┬─────────────────┐
  ↓               ↓
Historical     Materialization
Features           ↓
  ↓          SQLite Online Store
Model Training       ↓
                 Online Retrieval
                      ↓
                  Prediction


 Implementation

 Entity
`student` with join key `student_id`.

Data Source
A Feast `FileSource` points to `data/student_features.parquet` and uses `event_timestamp`.

FeatureView
`student_skill_features` groups the student skill features and makes them available offline and online.

Registration
The feature definitions are registered using:

feast Apply

Historical Retrieval
Historical features are retrieved with:

store.get_historical_features(...)

Model
A Logistic Regression model is trained using the historical Feast features. Categorical features are one-hot encoded and numerical features are imputed and standardized.

Materialization
Features are loaded into the SQLite online store using:

feast materialize 2025-01-01T00:00:00 2025-06-01T00:00:00

Online Retrieval
Online features are retrieved with:

store.get_online_features(...)

Final Prediction
The online feature vector for one student is passed to the trained Logistic Regression model to predict whether the student has a skill gap.

Results

Historical Feature Output
Insert a screenshot of the `historical_df.head(10)` output here.

Model Accuracy
Run the notebook and enter the exact accuracy shown by your Colab output.

With the supplied dataset and the notebook's fixed 80/20 stratified split (`random_state=42`), the Logistic Regression workflow is expected to give approximately **96.67% accuracy**. Report the exact value produced by your run.

Online Feature Output
Insert a screenshot of the online feature retrieval output here.

Final Prediction
Insert the screenshot showing the selected Student ID, predicted skill-gap label, and probability.

Required Analysis Questions

1. What is the entity?
The entity is `student`. Its join key is `student_id`.

2. What features are stored in the FeatureView?
The FeatureView stores the 14 features listed above, from `Specialization` through `Experience_Score`.

3. How was one feature calculated?
`Experience_Score` combines internship, placement training, projects, and certifications using a simple 0–100 weighted formula.

4. What is the difference between the original dataset and the feature dataset?
The original dataset contains 15 raw columns. The feature dataset replaces the two Yes/No fields used for serving with numeric flags, adds `Experience_Score`, adds `event_timestamp`, renames `Student_ID` to `student_id`, and keeps the target label separately for model training.

5. What is the purpose of the offline store?
The offline store contains batch feature data used for historical retrieval and model-training datasets.

6. What is the purpose of the online store?
The online store keeps the latest feature values for fast retrieval during prediction.

7. What is the purpose of `feast apply`?
`feast apply` registers the entity, data source, and FeatureView definitions and sets up the local feature-store infrastructure.

8. What does materialization do?
Materialization copies feature values from the offline data source into the online store so they can be retrieved during online inference.

9. What is the advantage of Feast?
Feast provides a common feature definition and retrieval mechanism for historical training and online prediction, reducing the chance that training and prediction use different feature calculations.

10. State two limitations.
1. The dataset has only 150 students.
2. It has no real event timestamp, so synthetic timestamps were created for the Feast demonstration.

11. State two possible improvements.
1. Add more students and more curriculum/industry evidence.
2. Replace synthetic timestamps with real time-based observations and use a production-grade online store when the system grows.

Conclusion
The project demonstrates how the student skill-gap dataset can be converted into a simple Feast feature store. It covers feature engineering, entity creation, data-source creation, FeatureView creation, `feast apply`, historical retrieval, machine-learning training, materialization, online retrieval, and final prediction.

