# IoT Intrusion Detection Using Machine Learning

An end-to-end Machine Learning project developed as part of the **Samsung Innovation Campus (SIC)** program.

The goal of the project is to use network-flow data to distinguish between **normal IoT traffic** and **malicious/attack traffic**, then deploy the trained model as an interactive web application.

## Project Overview

IoT devices are increasingly connected to networks, which makes them potential targets for cyber attacks. In this project, we investigated whether Machine Learning can learn patterns in network traffic and identify suspicious activity.

The project covers the main stages of an ML workflow:

- Data cleaning and preprocessing
- Exploratory Data Analysis (EDA)
- Feature correlation and multicollinearity analysis
- Feature analysis
- Model training and comparison
- Model evaluation
- Feature importance
- Model deployment using Streamlit

## Dataset

The project uses an IoT intrusion-detection network-flow dataset.

The original dataset contained **1,175,220 records**. During cleaning, duplicate records, unnecessary features, and invalid negative IAT values were handled, resulting in **1,175,217 valid records**.

The target variable is:

- `0` → Normal traffic
- `1` → Attack traffic

The dataset is imbalanced toward attack traffic, with approximately:

- **26.30% Normal**
- **73.70% Attack**

## Data Preprocessing

The preprocessing stage included:

1. Removing duplicate records.
2. Removing `Idle Mean` and `Idle Max`.
3. Removing invalid negative values from IAT-related features.
4. Investigating missing values and feature types.
5. Checking feature distributions and skewness.
6. Analyzing feature correlations.
7. Removing highly collinear/redundant features.

Some of the strongest correlations found included:

- `Total Bwd packets` ↔ `ACK Flag Count` ≈ 0.975
- `Packet Length Max` ↔ `Packet Length Std` ≈ 0.969
- `Total Fwd Packet` ↔ `ACK Flag Count` ≈ 0.960
- `Total Fwd Packet` ↔ `Fwd Header Length` ≈ 0.960

Six redundant features were removed to reduce multicollinearity.

## Exploratory Data Analysis

EDA was used to understand the behavior of normal and attack traffic.

The analysis included:

- Target/class distribution
- Feature distributions
- Highly skewed features
- KDE distributions
- Protocol analysis
- Port analysis
- Attack rate versus important features
- Correlation matrix
- Highly correlated feature pairs

The analysis showed that network-flow features contain useful information for distinguishing attack traffic from normal traffic.

## Machine Learning

The data was divided into training and testing sets using an **80/20 split** with stratification to preserve the class distribution.

Because the classes were imbalanced, class weighting was also considered during model training.

The following models were compared:

1. Logistic Regression
2. Ridge Classifier
3. Gaussian Naive Bayes
4. Decision Tree
5. Random Forest

### Model Results

| Model | Accuracy |
|---|---:|
| Logistic Regression | 90.23% |
| Ridge Classifier | 90.73% |
| Naive Bayes | 82.18% |
| Decision Tree | 99.57% |
| Random Forest | **99.58%** |

The project presentation reports the Random Forest result as approximately **99.6% accuracy**.

For an intrusion-detection problem, false negatives are especially important because a false negative represents an attack that the model failed to detect.

The Random Forest model reduced the number of missed attacks to **569**, which is less than 1% of the actual attacks in the test set.

## Best Model: Random Forest

The final selected model was **Random Forest**.

Configuration used in the notebook:

- `n_estimators = 100`
- `max_depth = 15`
- `min_samples_split = 10`
- `min_samples_leaf = 5`
- `class_weight = "balanced"`
- `random_state = 42`

The model performed very well on the unseen test data while maintaining high precision and recall for the attack class.

## Important Features

The Random Forest feature-importance analysis identified these features as the most important:

### 1. Total Length of Fwd Packet

Helps identify unusual data-transfer behavior in network traffic.

### 2. Packet Length Std

Captures variations in packet sizes that can help distinguish abnormal traffic patterns.

### 3. RST Flag Count

Can provide an indication of probing or scanning behavior in network traffic.

## Deployment

To move the project beyond the notebook, the trained model was prepared for deployment.

### Technology

- **Python**
- **Scikit-learn**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Joblib**
- **Streamlit**

The deployment workflow is:

```text
Trained ML Model
       ↓
Streamlit Web Application
       ↓
User enters network traffic features
       ↓
Model processes the input
       ↓
Prediction
Normal / Attack
       ↓
Result displayed to the user
```

The project presentation specifies **Streamlit** as the application technology and **Streamlit Community Cloud** as the deployment platform.

## Saved Models

The notebook exports the preprocessing scaler and trained models using Joblib.

The deployment directory contains:

```text
deploy/
├── scaler.joblib
└── models/
    ├── logistic_regression.joblib
    ├── naive_bayes.joblib
    ├── ridge_classifier.joblib
    ├── decision_tree.joblib
    └── random_forest.joblib
```

## Project Structure

A recommended GitHub structure is:

```text
IoT-Intrusion-Detection/
│
├── README.md
├── IoT_Intrusion_Detection_T24.ipynb
│
├── app.py
│
├── deploy/
│   ├── scaler.joblib
│   └── models/
│       ├── logistic_regression.joblib
│       ├── naive_bayes.joblib
│       ├── ridge_classifier.joblib
│       ├── decision_tree.joblib
│       └── random_forest.joblib
│
├── requirements.txt
│
└── presentation/
    └── Team_24_Presentation.pptx
```

## How to Run the Project

### 1. Clone the repository

```bash
git clone YOUR_GITHUB_REPOSITORY_LINK
cd IoT-Intrusion-Detection
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Streamlit application

```bash
streamlit run app.py
```

The application will open in your browser.

## Deployment

The application can be deployed through **Streamlit Community Cloud** by connecting the GitHub repository and selecting the Streamlit application file.

Make sure the repository contains:

- `app.py`
- `requirements.txt`
- The required model files
- The scaler file
- Any other files imported by the application

## Future Improvements

Based on our project recommendations, possible future improvements include:

- Testing Deep Learning approaches such as LSTMs.
- Building a real-time intrusion-detection system.
- Continuously retraining the model with newer network traffic.
- Exploring anomaly-detection techniques for previously unseen attacks.
- Improving the deployment to work with real-time network traffic.

## Team Members

- Ahmed Hesham
- Youssef Mohamed
- Ahmed Samy
- Ziad Hany

## Acknowledgements

Special thanks to:

- **Eng. Hazem** — for explaining the Machine Learning concepts and helping us build a stronger understanding of ML.
- **Eng. Abdallah** — for his support and facilitation throughout the project.
- **Eng. Abdelrahman** — for his guidance and supervision.

And a big thank you to my teammates **Eng. Youssef, Eng. Ahmed, and Eng. Ziad** for the teamwork, effort, and support throughout the project.

## Program

**Samsung Innovation Campus (SIC) – AI803 / G24**

---

## Disclaimer

This project was developed for educational and training purposes as part of the Samsung Innovation Campus program. The reported performance is based on the dataset and evaluation setup used in this project and should not be interpreted as a guarantee of performance on real-world network traffic.
