# Task 1




## 📊 1. Understanding the Data

The dataset contains **10,000 industrial machine operations**, each representing a snapshot of machine health.

### 🔹 Feature Overview

- **Sensor Readings (Numerical):**
  - Air Temperature [K]
  - Process Temperature [K]
  - Rotational Speed [rpm]
  - Torque [Nm]
  - Tool Wear [min]

- **Categorical Feature:**
  - `Type` → Machine quality level: **Low (L), Medium (M), High (H)**

- **Target Variable:**
  - `Target` → Binary classification  
    - `0`: No Failure  
    - `1`: Failure  

### 🔹 Key Observations

- **Severe Class Imbalance:** Only ~3.4% of records represent failures. A naive model predicting “No Failure” for all cases would achieve >96% accuracy while being useless in practice.
- **Real-World Complexity:** Failures arise due to *interactions* between multiple sensors rather than a single threshold.
- **Data Leakage Risk:** The dataset includes a `Failure Type` column, which is a *post-failure description*. Using it would unrealistically inflate model performance.

✔️ This understanding guided every modeling decision in the project.

---

## ⚙️ 2. Preprocessing & Feature Handling

The preprocessing pipeline was designed with **correctness, realism, and production readiness** in mind.

### 1️⃣ Leakage Prevention (Critical Step)

Removed the following columns:

- `UDI`, `Product ID` → Identifiers with no predictive value
- `Failure Type` → Post-event information (data leakage)

➡️ This ensures the model learns *only from observable machine conditions*.

### 2️⃣ Categorical Encoding

- Applied **Label Encoding** to the `Type` column (L → 0, M → 1, H → 2)
- Suitable as the categories represent ordered machine quality levels

### 3️⃣ Feature Scaling

- Applied **StandardScaler** to sensor readings
- Prevents features with large numeric ranges from dominating learning

> Although Random Forest models are scale-invariant, scaling was included for:
> - Pipeline consistency  
> - Model comparability  
> - Future extensibility  

### 4️⃣ Stratified Train–Test Split

- **80% Training / 20% Testing**
- Stratification preserves the rare failure ratio in both sets

✔️ This avoids misleading evaluation results.

---

## 🤖 3. Model Selection: Why Random Forest?

The **Random Forest Classifier** was chosen based on both theoretical and practical considerations.

### 🔍 Technical Justification

- **Captures Non-Linear Relationships:**  
  Mechanical failures often occur when *multiple conditions align* (e.g., high torque AND low speed). Tree-based ensembles naturally model such interactions.

- **Robust to Noise & Outliers:**  
  Industrial sensor data is inherently noisy. Ensemble averaging reduces variance and improves reliability.

- **Handles Class Imbalance:**  
  Using `class_weight='balanced'` penalizes the model more for missing failures, aligning learning with real operational costs.

- **Explainability:**  
  Feature importance scores provide actionable insights for engineers and maintenance teams.

- **Constraint-Compliant:**  
  Uses **classical machine learning**, avoiding unnecessary deep learning complexity while achieving strong performance on tabular data.

✔️ Random Forest offers the best balance between **performance, interpretability, and correctness**.

---

## 📈 4. Evaluation Strategy

Instead of optimizing for raw accuracy, evaluation focused on **decision-critical metrics**.

### 🎯 Metrics Used

- **Recall (Failure Class – Primary Metric):**  
  Measures how many actual failures were correctly identified.  
  > Missing a failure (False Negative) is far more costly than a false alarm.

- **F1-Score:**  
  Ensures balance between Recall and Precision.

- **Confusion Matrix:**  
  Provides a transparent breakdown of:
  - Correct detections
  - Missed failures
  - False alarms

### 📊 Visual Evaluation

- Correlation Heatmap → Understanding sensor relationships
  <img width="939" height="836" alt="image" src="https://github.com/user-attachments/assets/ca56ff96-b058-4184-a551-c595d7947259" />

- Scatter Plots → Visual separation of failure states
- <img width="973" height="547" alt="image" src="https://github.com/user-attachments/assets/ea1a480f-2f47-46f2-9e03-f7e9d4b4cb4b" />


✔️ The evaluation strategy mirrors **real-world deployment priorities**, not leaderboard optimization.

---

## 🧠 5. Insights & Interpretation

Feature importance analysis reveals:
- Which sensors contribute most to failure prediction
- How physical stress factors dominate failure risk

This bridges **data science and mechanical engineering**, enabling informed maintenance decisions rather than blind automation.

---

## 🚀 6. Future Improvements 

With additional time, the solution could be enhanced by:

### 1️⃣ Physics-Informed Feature Engineering
- Power = Torque × Rotational Speed
- Temperature Delta = Process Temp − Air Temp
- Wear Rate = Tool Wear / Operating Time

### 2️⃣ Advanced Imbalance Handling
- Apply **SMOTE** or **ADASYN** to generate realistic failure samples

### 3️⃣ Robust Validation
- K-Fold Cross-Validation to ensure stability across datasets

### 4️⃣ Threshold Optimization
- Tune classification thresholds using cost-sensitive analysis

### 5️⃣ Deployment Readiness
- Model monitoring for concept drift
- Integration with real-time sensor streams

---

