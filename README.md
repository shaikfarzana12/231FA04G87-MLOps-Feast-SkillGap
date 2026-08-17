# 231FA04G87-MLOps-Feast-SkillGap
Name : Shaik Farzana,
Reg.no : 231FA04G87,
Sec : 15


1. What is the entity in your Feast implementation?
The entity is student, identified using the student_id column. Each student represents one entity in Feast.
2. List the features stored in your FeatureView.
The FeatureView stores technical_skill_score, soft_skill_score, strong_skill_count, weak_skill_count, and overall_skill_score.
3. Explain how one feature was calculated.
overall_skill_score is calculated as the average of technical_skill_score and soft_skill_score.
4. Difference between original dataset and feature dataset?
The original dataset contains raw student skill data, while the feature dataset contains processed and engineered features prepared for Feast and ML.
5. Purpose of the offline store?
The offline store stores historical feature data and is mainly used for historical feature retrieval and model training.
6. Purpose of the online store?
The online store provides feature values quickly for real-time prediction and online feature retrieval.
7. Purpose of feast apply?
feast apply registers the Feast entities, data sources, FeatureViews, and FeatureServices with the feature store.
8. What does materialization do?
Materialization transfers feature data from the offline store to the online store for fast online retrieval.
9. Advantage of using Feast instead of manually calculating features?
Feast provides consistent features for both training and prediction, reducing the risk of training-serving inconsistency.
10. Two limitations of your current dataset.
The dataset is relatively small and may not fully represent real-world curriculum and industry skill requirements.
11. Two ways to improve the feature store?
Add more curriculum/industry data and create advanced features such as job-role skill requirements and skill-demand trends.
