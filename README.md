# ITD214-Sportswear-App-Review-Analytics
 
## 📌 Project Overview

This project performs sentiment quantification and pattern analysis on sportswear mobile application reviews (Nike, Adidas, Puma, Gymshark) to support data-driven business decision-making.

The study integrates exploratory data analysis with supervised text classification modelling to evaluate whether review text alone can automatically reproduce rating-derived sentiment labels.

The project incorporates predictive text classification using:

### Feature Engineering
- Bag-of-Words (BoW)
- TF-IDF

### Classification Models
- Naive Bayes
- Logistic Regression
- Support Vector Machine (SVM)

The modelling framework progresses from 3-class sentiment classification (Negative / Neutral / Positive) to a binary dissatisfaction detection approach (Negative vs Positive) to better support early complaint identification and business monitoring.

---

## 🎯 Business Objective (Objective 2 – Sentiment Quantification & Pattern Analysis)

To:

- Quantify sentiment distribution across brands
- Analyse engagement patterns (thumbs-up behaviour)
- Identify recurring complaint themes
- Develop and evaluate predictive sentiment classification models
- Select the most suitable model for practical deployment

---

## 📊 Dataset Source

- Platform: Google Play Store
- Dataset: Sportswear App Reviews
- Brands: Nike, Adidas, Puma, Gymshark
- Source: Kaggle – *sportswear-app-reviews-google-play*  
  https://www.kaggle.com/datasets/krisbruurs/sportswear-app-reviews-google-play

  
The raw dataset contains the following fields:

- brand – Application brand
- review_id – Unique review identifier
- score – Star rating (1–5)
- at – Review timestamp
- content – Review text
- reply_content – Developer reply (largely missing)
- thumbs_up – Number of users who found the review helpful
- review_created_version – App version at time of review
 

## 🧠 Sentiment Ground Truth Construction

Sentiment labels were derived using rule-based mapping from ratings:

- 1–2 → Negative
- 3 → Neutral
- 4–5 → Positive

These rating-derived labels serve as ground truth to evaluate whether textual review content can predict sentiment automatically.

---

## 📊 Data Understanding

### Dataset Overview

- 6,446 total reviews
- 4 brands (Nike, Adidas, Puma, Gymshark)
- No duplicate rows detected

### Rating Distribution

- 5-star reviews ≈ 66%
- Overall positive (4–5 stars) ≈ 74%
- Neutral ≈ 4%

This indicates strong class imbalance. This imbalance significantly influences model evaluation and selection.

### Engagement Distribution

Thumbs-up counts are highly right-skewed:
- Most reviews receive zero engagement
- A small number of negative reviews receive disproportionately high thumbs-up counts

This suggests that complaints resonate strongly with other users.

### Short Review Insight
Approximately 40% of reviews contain three words or fewer, limiting contextual richness and increasing classification uncertainty.

---

## 🧹 Data Preparation, Engineering & Feature Selection

The following cleaning and engineering steps were performed:

### Structural Cleaning

- Backed up raw dataset
- Standardised column names
- Renamed at → review_datetime
- Converted to datetime format
- Created year_month feature for trend analysis
- Dropped reply_content (93% missing values)
- Validated score range (1–5 only)
- Validated brand values (Nike, Adidas, Puma, Gymshark)
 
### Data Quality Validation

- No duplicate rows detected
- Score range validated (1–5 only)
- Brand values validated (Nike, Adidas, Puma, Gymshark)
- reply_content dropped due to 93% missing values
- 118 rows removed after preprocessing due to empty clean_content (fully cleaned text contained no valid tokens)

### Fields Used for Analysis & Modelling

The following variables were retained for analysis:
- brand
- score
- content
- thumbs_up
- review_datetime (renamed from at)
- review_created_version

The reply_content column was removed due to 93% missing values and limited analytical relevance.

### Engineered Features

The following features were engineered during Data Preparation:

- review_datetime (converted "at" string data type to datetime format)
- year_month (for temporal sentiment trend analysis)
- content_length_chars (review length in characters)
- content_length_words (review length in words)
- clean_content (fully preprocessed text for modelling)
- sentiment_label (rating-derived target variable)
 
### Text Preprocessing Pipeline

Preprocessing applied to create clean_content:
- Lowercasing and whitespace normalisation
- Normalisation of negations (e.g., "can't" → "cant")
- Removal of punctuation, numbers, emojis, and non-ASCII characters
- Stopword removal (while preserving negation words such as "not", "never")
- Tokenisation using NLTK
- No stemming (to preserve word clarity)

Note: Removal of non-ASCII characters may reduce representation of multilingual content.

---

## 📈 Pattern Analysis

Exploratory analysis included:

- Sentiment distribution by brand
- Engagement behaviour (thumbs-up vs sentiment)
- Monthly sentiment trend analysis
- Word frequency analysis
- Bigram analysis

Key Findings:
- Reviews are predominantly positive.
- Negative reviews receive higher engagement.
- Complaint themes focus on:
  - Refund delays
  - Login and sync issues
  - App stability and checkout problems
- Mild increase in negative sentiment observed in 2025.
 
---

## 🤖 Modelling & Evaluation

### Evaluation Strategy
Due to strong class imbalance, overall accuracy was not treated as the primary evaluation metric.

Instead:
- Macro F1-score was used to ensure balanced performance across classes.
- Balanced accuracy was used to mitigate majority-class bias.
- Confusion matrices were analysed to inspect misclassification patterns.
- Stratified train-test split was applied.

### 3-Class Baseline Models
Models evaluated:
- BoW + Naive Bayes
- BoW + Logistic Regression
- TF-IDF + Naive Bayes
- TF-IDF + Logistic Regression
- TF-IDF + SVM (LinearSVC)

**BoW + Naive Bayes** achieved the highest Macro F1-score and overall balanced performance among baseline models.

However:
- Neutral recall remained consistently low (~4% class share).
- Positive class dominance affected model balance.

---
### Hyperparameter Tuning (Improvement Attempt)

TF-IDF + SVM was selected for hyperparameter tuning using GridSearchCV.

Tuned parameters included:
- min_df
- max_df
- sublinear_tf
- C
- class_weight

Although tuning slightly improved Neutral recall, it did not surpass BoW + Naive Bayes in overall Macro F1-score. 

---

### Baseline vs Tuned Comparison

Despite tuning TF-IDF + SVM, the baseline BoW + Naive Bayes model achieved higher Macro F1-score in the 3-class setting.

This indicates that model complexity does not necessarily outperform simpler probabilistic approaches under severe class imbalance conditions.

---

### 🔄 Model Improvement – Binary Redesign - Dissatisfaction Detection

Due to persistent Neutral class imbalance and low recall performance (~4% class share), a binary redesign was implemented to prioritise reliable dissatisfaction detection over full sentiment granularity.

Final classification:
- Negative
- Positive

The dataset remained imbalanced even after removing the Neutral class; therefore:
- Class-weighted models were applied
- Balanced evaluation metrics were prioritised

Models evaluated:
- TF-IDF + Logistic Regression (class_weight="balanced")
- TF-IDF + SVM (tuned)
 
---

## 🏆 Final Model Selected

**TF-IDF (Unigram) + Logistic Regression (class_weight="balanced")**

Rationale:

- Highest F1-score for Negative class
- Best balanced accuracy
- Strong alignment with business monitoring needs
- More stable minority-class detection

This model provides a practical balance between interpretability, computational efficiency, and minority-class performance.

---

## 🔍 Key Analytical Findings

- 74% of reviews are positive (4–5 stars), indicating overall satisfaction.
- Neutral class represents only ~4%, creating modelling difficulty.
- 40% of reviews are ≤3 words, limiting contextual signal.
- Negative reviews receive disproportionately higher thumbs-up engagement.
- Complaint themes focus on operational reliability (refund delays, login errors, sync failures).

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

## ▶️ How to Run

1. Open the notebook in Google Colab Notebook.
2. Upload reviews.csv into the working directory.
3. Ensure required libraries are installed (pandas, numpy, scikit-learn, matplotlib).
4. Run all cells sequentially from top to bottom.
All cleaning, preprocessing, modelling, and evaluation steps are implemented within the notebook.
   
---

## 📂 Repository Contents

- 5504890N_Applied Data Science Project.ipynb – End-to-end data cleaning, analysis, modelling and evaluation pipeline
 
---

## 👩‍💻 Author

Thin Thin Hlaing  
ITD214 – Applied Data Science Project   

