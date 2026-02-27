# ITD214-Sportswear-App-Review-Analytics
 
## 📌 Project Overview

This project performs sentiment and pattern analysis on sportswear mobile application reviews (Nike, Adidas, Puma, Gymshark) to support data-driven business decision-making.

The project incorporates predictive text classification using:

- Bag-of-Words (BoW)
- TF-IDF
- Logistic Regression
- Support Vector Machine (SVM)

The final objective is to enable automated dissatisfaction detection and extract actionable business insights.

---

## 🎯 Business Objective (Objective 2 – Sentiment Quantification & Pattern Analysis)

To:

- Quantify sentiment distribution across brands
- Analyse engagement patterns (thumbs-up behaviour)
- Identify recurring complaint themes
- Develop and evaluate predictive sentiment classification models
- Select the most suitable model for practical deployment

---

## 📊 Data Understanding

Dataset Source:
Google Play Store user reviews

Key Fields:
- Review text
- Rating score (1–5)
- Thumbs-up count
- Date
- Brand

Sentiment labels were derived using rule-based mapping:
- 1–2 → Negative
- 3 → Neutral
- 4–5 → Positive

---

## 🧹 Data Preparation

The following steps were performed:

- Removal of missing and invalid entries
- Text preprocessing (lowercasing, cleaning)
- Label encoding
- Stratified train-test split
- Class imbalance handling using:
  - Macro F1-score
  - Balanced accuracy
  - Class_weight adjustment

---

## 📈 Pattern Analysis

Exploratory analysis included:

- Sentiment distribution by brand
- Engagement behaviour (thumbs-up vs sentiment)
- Monthly sentiment trend analysis
- Word frequency and bigram analysis

Key observations:

- Reviews are predominantly positive.
- Negative reviews receive higher engagement.
- Emerging mild increase in negative sentiment in 2025.
- Complaints mainly relate to service and operational issues.

---

## 🤖 Modelling & Evaluation

### 3-Class Baseline Models

- BoW + Naive Bayes
- TF-IDF + Logistic Regression
- TF-IDF + SVM (tuned)

Observation:
Neutral class showed high misclassification due to class imbalance.

---

### 🔄 Model Improvement – Binary Redesign

To improve dissatisfaction detection, Neutral class was removed.

Final classification:
- Negative
- Positive

Models evaluated:

- TF-IDF + Logistic Regression (class_weight="balanced")
- TF-IDF + SVM (tuned)

Evaluation Metrics:

- F1-score (Negative class)
- Balanced Accuracy
- Confusion Matrix

---

## 🏆 Final Model Selected

**TF-IDF (Unigram) + Logistic Regression (class_weight="balanced")**

Rationale:

- Highest F1-score for Negative class
- Best balanced accuracy
- Strong alignment with business monitoring needs

---

## 💡 Business Recommendations

1. Prioritise high-engagement complaints
2. Improve customer support and operational workflows
3. Maintain usability strengths identified in positive reviews
4. Implement continuous sentiment monitoring dashboard
5. Integrate recurring complaint themes into product improvement cycles

---

## ⚖️ AI Ethics Considerations

Privacy:
- Dataset consists of publicly available reviews.
- No personal identifiable information intentionally collected.

Fairness:
- Dataset is class-imbalanced.
- Macro F1-score and class_weight were used to mitigate bias.

Transparency:
- Labels derived using explicit rating-based mapping.
- Model selection based on clearly reported evaluation metrics.

Accountability:
- Model supports decision-making but does not replace human judgement.

Reliability:
- Continuous monitoring and periodic retraining recommended.

---

## 📂 Repository Contents

- Final modelling notebook
- Cleaned dataset used for modelling
- Visualisations and evaluation outputs

---

## 👩‍💻 Author

Thin Thin Hlaing  
ITD214 – Applied Data Science Project  
Specialist Diploma in Business & Big Data Analytics

