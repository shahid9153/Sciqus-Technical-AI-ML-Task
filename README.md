# Task 1


## 📊 1. Understanding the Data
The dataset represents 10,000 industrial operations with the following characteristics:
- **Technical Features:** 5 sensor readings (Air Temperature, Process Temperature, Rotational Speed, Torque, and Tool Wear).
- **Categorical Data:** Machine quality type (Low, Medium, High).
- **Class Imbalance:** Only ~3.4% of the data points represent a failure. This is a "needle in a haystack" problem where simple accuracy is a misleading metric.
- **Data Leakage Risk:** The dataset contains a `Failure Type` column. This is a post-hoc description; using it would lead to a 100% accurate but physically impossible model (predicting the past).

---

## ⚙️ 2. Preprocessing & Feature Handling
To ensure the model is "Production Ready," I performed the following steps:
1. **Leakage Removal:** Dropped `Failure Type`, `UDI`, and `Product ID`. This forces the model to learn only from the physical state of the machine.
2. **Label Encoding:** Converted the categorical `Type` feature (L, M, H) into numerical integers.
3. **Feature Scaling:** Applied `StandardScaler` to the sensor readings. This ensures that features with large ranges (like Rotational Speed) do not dominate those with smaller ranges (like Temperature).
4. **Stratified Splitting:** Used an 80/20 split with stratification to preserve the minority class (failures) ratio in both the training and testing sets.

---

## 🤖 3. Model Selection: Random Forest
I chose the **Random Forest Classifier** for its logical alignment with industrial data:
- **Non-Linear Relationships:** Mechanical failures often involve complex thresholds (e.g., if Torque > X AND Speed < Y). Tree-based ensembles capture these much better than linear models.
- **Handling Imbalance:** Used the `class_weight='balanced'` parameter. This mathematically penalizes the model more for missing a failure than for a false alarm.
- **Explainability:** It provides "Feature Importance" scores, which allow engineers to understand exactly which sensor is the primary predictor of a breakdown.
- **No Deep Learning:** Adhering to constraints, this ML model provides state-of-the-art performance for tabular data without the complexity of a Neural Network.

---

## 📈 4. Evaluation Strategy
The model was evaluated using a comprehensive suite of metrics:
- **Recall (Primary Metric):** In predictive maintenance, missing a failure (False Negative) is much more expensive than a false alarm. I prioritized high Recall for the "Failure" class.
- **F1-Score:** Used to ensure a balance between Precision and Recall.
- **Confusion Matrix:** Provided to visualize the exact count of caught failures vs. missed ones.

---

## 🚀 5. Future Improvements
Given more time, the solution could be improved by:
1. **Physical Feature Engineering:** Creating new variables like "Power" (Torque × Speed) or "Temperature Delta" (Process Temp - Air Temp).
2. **SMOTE:** Using Synthetic Minority Over-sampling to generate artificial failure samples and further improve the decision boundary.
3. **Cross-Validation:** Implementing K-Fold cross-validation to ensure the model's stability across different subsets of data.
