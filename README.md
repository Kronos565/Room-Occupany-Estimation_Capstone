# Room Occupancy Estimation Using Environmental Sensor Data

This project predicts the number of occupants in a room using data collected from multiple non-intrusive environmental sensors. The sensors record temperature, light, sound, CO₂, and motion (PIR) in a controlled environment. This README details the data preprocessing and feature engineering steps completed up to data splitting.

## Dataset Overview
- **Instances**: 10,129 rows  
- **Features**: 19 columns, including:  
  - **Categorical**: `Date`, `Time`  
  - **Numerical Sensor Readings**:  
    - Temperature: `S1_Temp`, `S2_Temp`, `S3_Temp`, `S4_Temp`  
    - Light: `S1_Light`, `S2_Light`, `S3_Light`, `S4_Light`  
    - Sound: `S1_Sound`, `S2_Sound`, `S3_Sound`, `S4_Sound`  
    - CO₂: `S5_CO2`, `S5_CO2_Slope`  
    - Motion (PIR): `S6_PIR`, `S7_PIR`  
- **Target**: `Room_Occupancy_Count`  

---

## Preprocessing and Feature Engineering

### 1. Data Preprocessing
- **Missing Values**:  
  The dataset was checked for missing values, and none were found.  

- **Outlier Handling**:  
  - **Capping**: Extreme values in highly skewed columns (e.g., light sensor readings) were capped using the IQR method.  
  - **Log Transformation**: Moderately skewed columns (e.g., CO₂ and sound sensor readings) were log-transformed to reduce skewness.  
  - **Remaining Columns**: Columns with acceptable distributions were left unchanged.  

---

### 2. Feature Engineering
- **Encoding Categorical Variables**:  
  The `Date` and `Time` columns were converted into numerical dummy variables using one-hot encoding.  
  *Note*: `drop_first=True` was used to avoid multicollinearity.  

- **Feature Selection Using Random Forest**:  
  A Random Forest Classifier was trained on all features to compute feature importances. Features with an importance score above a threshold (**0.06**) were selected as they are more informative for predicting room occupancy.  

- **Feature Scaling**:  
  All features were standardized using `StandardScaler` to ensure they are on the same scale.  

---

### 3. Data Splitting
The final dataset was split into **training and testing sets (80/20 split)** using stratification to maintain the target class distribution.  

---

## Next Steps
1. **Model Training**:  
   With the data split into training and testing sets, the next steps involve selecting and training a classification model (e.g., Logistic Regression, Random Forest, or XGBoost).  

2. **Model Evaluation**:  
   Evaluate the model's performance using metrics such as accuracy, precision, recall, and F1-score.  

3. **Hyperparameter Tuning**:  
   Optimize the model parameters to further improve performance.  

---
## Model Training & Evaluation
After splitting the data into training and test sets, we trained several classification models:
- **Models**: Support Vector Machines (SVM), Decision Trees, Random Forest, K-Nearest Neighbors (KNN), Gradient Boosting.  
- **Evaluation Metrics**:  
  - Training/Test Accuracy  
  - Confusion Matrices  
  - Classification Reports (Precision, Recall, F1-Score)  

This step ensures the model generalizes well to unseen data while avoiding overfitting.

---

## Hyperparameter Tuning
To optimize performance, we tuned hyperparameters using **GridSearchCV**. Key parameters adjusted for tree-based models (Decision Tree, Random Forest, Gradient Boosting):  

| Parameter           | Purpose                                                                 |
|---------------------|-------------------------------------------------------------------------|
| `max_depth`         | Limits tree depth to prevent overfitting.                              |
| `min_samples_split` | Minimum samples required to split a node.                              |
| `min_samples_leaf`  | Ensures leaves have sufficient samples.                                |
| `max_features`      | Controls features considered per split.                                |
| `learning_rate`     | Reduces step size in Gradient Boosting for stable learning.            |

---

## Saving & Using the Final Model
The best-performing pipeline (preprocessing + model) was saved using `joblib` for future predictions
## Conclusions & Next Steps

### Key Outcomes
- **Built an end-to-end pipeline** for occupancy prediction using sensor data.  
- **Achieved robust performance** through preprocessing, feature engineering, and hyperparameter tuning.  

### Next Steps
1. **Explore Advanced Models**: Test ensemble methods (e.g., stacking, XGBoost).  
2. **Fine-Tuning**: Optimize hyperparameters further with expanded grids.  
3. **Deployment**: Convert the pipeline into an API for real-time predictions.  
4. **Evaluation**: Track precision/recall trade-offs for specific use cases.  

This project demonstrates a systematic approach to building and deploying machine learning models for real-world sensor data analysis.
