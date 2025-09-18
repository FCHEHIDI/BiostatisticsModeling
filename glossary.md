# 📚 Biostatistics & Medical AI Glossary

## Table of Contents
- [Statistical Concepts](#statistical-concepts)
- [Feature Selection Methods](#feature-selection-methods)
- [Evaluation Metrics](#evaluation-metrics)
- [Survival Analysis](#survival-analysis)
- [Clinical Significance](#clinical-significance)
- [Regulatory & Ethics](#regulatory--ethics)
- [Machine Learning in Healthcare](#machine-learning-in-healthcare)

---

## Statistical Concepts

### **ANOVA F-test**
**Definition**: Analysis of Variance F-test compares means across groups to determine if at least one group differs significantly from others.

**Intuitive Example**: Testing if average tumor size differs between malignant vs benign breast masses. If F-score is high and p-value < 0.05, tumor size is a useful diagnostic feature.

**Clinical Application**: Screening biomarkers and clinical variables for their ability to distinguish disease states.

---

### **P-Value**
**Definition**: Probability of observing the data (or more extreme) if the null hypothesis (no effect) is true.

**Intuitive Example**: p = 0.001 means there's only a 0.1% chance we'd see this large a difference in cholesterol levels between heart disease patients and healthy controls if cholesterol truly doesn't matter.

**Clinical Interpretation**: p < 0.05 suggests statistical significance, but doesn't guarantee clinical importance.

---

### **Multiple Testing Correction**
**Definition**: Statistical adjustments when testing many hypotheses simultaneously to control false discovery rate.

**Intuitive Example**: Testing 1000 genes for cancer association. Without correction, 50 genes would appear significant by chance alone (5% false positive rate).

**Methods**:
- **Bonferroni**: Divides α by number of tests (conservative)
- **FDR (Benjamini-Hochberg)**: Controls proportion of false discoveries (less conservative)

---

### **Effect Size**
**Definition**: Magnitude of difference between groups, independent of sample size.

**Intuitive Example**: Drug A reduces blood pressure by 2 mmHg (small effect) vs Drug B reduces it by 15 mmHg (large effect), even if both have p < 0.05.

**Clinical Relevance**: Large effect sizes are more likely to be clinically meaningful than small ones.

---

### **Confidence Intervals (CI)**
**Definition**: Range of values likely to contain the true population parameter with specified confidence level.

**Intuitive Example**: "Treatment reduces mortality by 25% (95% CI: 15%-35%)" means we're 95% confident the true reduction is between 15% and 35%.

**Clinical Use**: Helps assess precision of estimates and clinical significance of effects.

---

## Feature Selection Methods

### **Forward Selection**
**Definition**: Iteratively adds features that most improve model performance, starting with empty set.

**Intuitive Example**: Building a heart disease predictor:
1. Start with no features
2. Add "chest pain" (biggest improvement)
3. Add "age" (next biggest improvement)
4. Stop when no feature improves performance

**Advantages**: Simple, interpretable, often finds minimal effective feature set
**Disadvantages**: May miss feature interactions, can be computationally expensive

---

### **Recursive Feature Elimination (RFE)**
**Definition**: Starts with all features, iteratively removes least important ones based on model weights.

**Intuitive Example**: Cancer diagnosis with 30 imaging features:
1. Train model with all 30 features
2. Remove 3 least important features
3. Retrain with 27 features
4. Repeat until optimal number found

**Cross-Validation (RFECV)**: Uses CV to find optimal number of features automatically.

---

### **LASSO (L1 Regularization)**
**Definition**: Linear model that penalizes absolute values of coefficients, automatically setting some to zero.

**Intuitive Example**: Predicting house prices with 100 variables. LASSO might select only 15 important ones (bedrooms, location, size) and ignore irrelevant ones (paint color, doorknob style).

**Medical Application**: Identifies most important biomarkers from high-dimensional data (genomics, imaging).

**Alpha Parameter**: Controls regularization strength. Higher α = fewer features selected.

---

### **Random Forest Feature Importance**
**Definition**: Measures how much each feature decreases impurity when used for splits across all trees.

**Intuitive Example**: In 1000 decision trees for diabetes prediction, if "blood glucose" is used for important splits in 800 trees, it has high importance.

**Advantages**: Handles non-linear relationships, feature interactions
**Disadvantages**: Can favor numerical/high-cardinality features

---

### **Univariate vs Multivariate Analysis**

#### **Univariate Analysis**
**Definition**: Examines one feature at a time in relation to the outcome, ignoring all other variables.

**Medical Example**: Testing if "age alone" predicts heart disease risk
- Look only at age vs heart disease
- Ignore cholesterol, blood pressure, smoking, etc.
- Simple correlation: older patients → higher risk

**Advantages**: Simple, interpretable, good for initial screening
**Disadvantages**: Misses confounding variables and feature interactions

#### **Multivariate Analysis** 
**Definition**: Examines multiple features simultaneously, accounting for their combined and interactive effects.

**Medical Example**: Comprehensive heart disease risk model
- Consider age, cholesterol, BP, smoking together
- Might find: "Age matters more in non-smokers"
- Or: "High cholesterol + high BP = synergistic risk"

**Real-World Insight**: A 70-year-old with perfect cholesterol and BP might have lower risk than a 40-year-old smoker with diabetes.

#### **Key Clinical Difference**
- **Univariate**: "Is cholesterol associated with heart disease?" (Answer: Yes)
- **Multivariate**: "Does cholesterol matter after accounting for age, smoking, and diabetes?" (Answer: Maybe less than we thought)

#### **Simpson's Paradox Example**
Univariate analysis might show:
- Drug A: 80% success rate
- Drug B: 70% success rate
- Conclusion: Drug A is better

Multivariate analysis reveals:
- Drug A used mostly on mild cases (90% success on mild, 60% on severe)
- Drug B used mostly on severe cases (75% success on mild, 65% on severe)
- **True conclusion**: Drug B is actually better for both mild AND severe cases!

### **Univariate Statistical Tests**
**Definition**: Tests each feature independently against the target variable.

#### **Chi-Square Test**
**Use**: Categorical feature vs categorical outcome

**Intuitive Example**: Testing if smoking status (smoker/non-smoker) is associated with lung cancer (yes/no)
- Create 2x2 table: Smokers with/without cancer vs Non-smokers with/without cancer
- Chi-square tests if the pattern differs from what we'd expect by chance
- **Result**: χ² = 45.2, p < 0.001 → "Smoking is significantly associated with lung cancer"

**Clinical Interpretation**: Helps identify categorical risk factors (gender, blood type, treatment response categories)

#### **t-Test**
**Use**: Continuous feature vs binary outcome (two groups)

**Intuitive Example**: Testing if cholesterol levels differ between heart disease patients and healthy controls
- Group 1: Heart disease patients (mean cholesterol = 240 mg/dL)
- Group 2: Healthy controls (mean cholesterol = 180 mg/dL)
- t-test determines if this 60 mg/dL difference is statistically significant
- **Result**: t = 8.5, p < 0.001 → "Heart disease patients have significantly higher cholesterol"

**Clinical Interpretation**: Compares average values of lab tests, vital signs, or biomarkers between disease groups

#### **F-Test (ANOVA)**
**Use**: Continuous feature vs multi-class outcome (three or more groups)

**Intuitive Example**: Testing if blood glucose levels differ across diabetes stages
- Group 1: Normal (mean glucose = 90 mg/dL)
- Group 2: Pre-diabetic (mean glucose = 110 mg/dL)  
- Group 3: Diabetic (mean glucose = 180 mg/dL)
- Group 4: Severe diabetic (mean glucose = 250 mg/dL)
- F-test determines if these group differences are statistically significant
- **Result**: F = 156.3, p < 0.001 → "Blood glucose significantly differs across diabetes stages"

**Clinical Interpretation**: Useful for disease staging, treatment response levels, or severity classifications

#### **Mann-Whitney U Test (Non-parametric alternative to t-test)**
**Use**: When data doesn't follow normal distribution

**Intuitive Example**: Testing if hospital stay length differs between two treatments
- Treatment A: Stays = [3, 4, 5, 7, 45] days (one outlier: 45 days)
- Treatment B: Stays = [8, 9, 10, 12, 14] days
- Mann-Whitney compares ranks instead of means (handles the 45-day outlier better)
- **Result**: U = 2.5, p = 0.02 → "Treatment A results in shorter stays"

**Clinical Interpretation**: Preferred when dealing with skewed data (length of stay, survival times, viral loads)

#### **Fisher's Exact Test**
**Use**: Categorical data with small sample sizes

**Intuitive Example**: Testing rare drug side effects in small clinical trial
- Drug group: 2 out of 15 patients have side effect
- Placebo group: 0 out of 13 patients have side effect
- Fisher's exact test works well with small numbers (chi-square might be unreliable)
- **Result**: p = 0.18 → "No significant difference in side effect rates"

**Clinical Interpretation**: Essential for rare diseases, small pilot studies, or uncommon adverse events

**Overall Limitation**: All univariate tests ignore feature interactions and correlations - they can't tell you if age and smoking together create synergistic risk.

---

## Evaluation Metrics

### **AUC-ROC (Area Under Curve - Receiver Operating Characteristic)**
**Definition**: Measures classifier's ability to distinguish between classes across all threshold settings.

**Interpretation**:
- **AUC = 1.0**: Perfect classifier
- **AUC = 0.9-0.99**: Excellent (suitable for clinical use)
- **AUC = 0.8-0.89**: Good
- **AUC = 0.7-0.79**: Fair
- **AUC = 0.5**: No better than random guessing

**Clinical Example**: Breast cancer diagnostic model with AUC = 0.95 correctly identifies 95% of all possible pairs where one patient has cancer and another doesn't.

---

### **Sensitivity (Recall)**
**Definition**: Proportion of actual positives correctly identified.

**Formula**: True Positives / (True Positives + False Negatives)

**Clinical Example**: Cancer screening test with 90% sensitivity detects 90 out of 100 actual cancers, missing 10.

**Medical Priority**: High sensitivity crucial for disease screening (don't miss cases).

---

### **Specificity**
**Definition**: Proportion of actual negatives correctly identified.

**Formula**: True Negatives / (True Negatives + False Positives)

**Clinical Example**: Test with 95% specificity correctly identifies 95 out of 100 healthy patients, falsely flagging 5 as diseased.

**Medical Priority**: High specificity important to avoid unnecessary treatments/anxiety.

---

### **Precision (Positive Predictive Value)**
**Definition**: Proportion of positive predictions that are actually correct.

**Formula**: True Positives / (True Positives + False Positives)

**Clinical Example**: If diagnostic test predicts cancer in 100 patients and 85 actually have cancer, precision = 85%.

**Practical Meaning**: "When this test says cancer, how often is it right?"

---

### **F1-Score**
**Definition**: Harmonic mean of precision and recall, balances both metrics.

**Formula**: 2 × (Precision × Recall) / (Precision + Recall)

**Use Case**: When you need balance between catching all cases (recall) and avoiding false alarms (precision).

---

### **Cross-Validation**
**Definition**: Technique to assess model performance by training/testing on different data subsets.

**K-Fold CV**: Divide data into K subsets, train on K-1, test on 1, repeat K times.

**Stratified CV**: Ensures each fold has same class distribution as original dataset.

**Medical Importance**: Prevents overfitting, provides realistic performance estimates before clinical deployment.

---

## Survival Analysis

### **Kaplan-Meier Estimator**
**Definition**: Non-parametric method to estimate survival probability over time from observed data.

**Intuitive Example**: Following 100 cancer patients:
- At 6 months: 90 alive → survival probability = 90%
- At 12 months: 75 alive → survival probability = 75%
- Accounts for patients lost to follow-up (censored)

**Clinical Use**: Standard method for presenting survival data in clinical trials.

---

### **Censoring**
**Definition**: Occurs when the outcome (death/event) is not observed during study period.

**Types**:
- **Right censoring**: Patient alive at study end
- **Left censoring**: Event occurred before study start
- **Interval censoring**: Event occurred within a time interval

**Example**: 5-year cancer study where 30% of patients are still alive at study end (right-censored).

---

### **Cox Proportional Hazards Model**
**Definition**: Statistical model relating survival time to predictor variables, estimating hazard ratios.

**Hazard Ratio (HR)**:
- **HR = 1**: No effect
- **HR > 1**: Increased risk (harmful)
- **HR < 1**: Decreased risk (protective)

**Example**: HR = 2.0 for smoking means smokers have twice the risk of lung cancer at any given time.

**Assumptions**: Hazard ratios remain constant over time (proportional hazards).

---

### **Log-Rank Test**
**Definition**: Statistical test comparing survival curves between groups.

**Intuitive Example**: Comparing survival between two cancer treatments:
- Drug A: Median survival 24 months
- Drug B: Median survival 18 months
- Log-rank test determines if difference is statistically significant

**Null Hypothesis**: No difference in survival between groups.

---

### **Concordance Index (C-Index)**
**Definition**: Probability that model correctly orders survival times for random pairs of patients.

**Interpretation**:
- **C-index = 0.5**: Random predictions
- **C-index = 0.7**: Good discrimination
- **C-index = 0.8+**: Excellent discrimination

**Example**: C-index = 0.75 means model correctly predicts which patient will survive longer 75% of the time.

---

## Clinical Significance

### **Clinical vs Statistical Significance**
**Statistical Significance**: Results unlikely due to chance (p < 0.05)
**Clinical Significance**: Results meaningful for patient care

**Example**: Drug reduces systolic blood pressure by 2 mmHg (p < 0.001)
- **Statistically significant**: Yes (large sample size)
- **Clinically significant**: Questionable (minimal impact on health)

**Key Point**: Large studies can detect tiny, clinically irrelevant differences.

---

### **Number Needed to Treat (NNT)**
**Definition**: Number of patients who need treatment for one to benefit.

**Formula**: 1 / Absolute Risk Reduction

**Example**: If drug prevents heart attack in 5% more patients than placebo, NNT = 20 (treat 20 patients to prevent 1 heart attack).

**Clinical Value**: Helps assess cost-effectiveness and treatment decisions.

---

### **Minimal Clinically Important Difference (MCID)**
**Definition**: Smallest change in outcome that patients perceive as beneficial.

**Examples**:
- Pain scales: 1-2 point reduction (0-10 scale)
- Blood pressure: 5-10 mmHg reduction
- Cholesterol: 10% reduction

**Regulatory Importance**: FDA considers MCID for drug approvals.

---

## Regulatory & Ethics

### **FDA AI/ML Guidance**
**Key Requirements**:
1. **Algorithm transparency**: Explainable decision-making
2. **Validation strategy**: Appropriate clinical testing
3. **Bias assessment**: Performance across demographic groups
4. **Change control**: Managing model updates
5. **Real-world monitoring**: Continued performance assessment

---

### **Explainable AI (XAI)**
**Definition**: AI systems that provide understandable explanations for their decisions.

**Medical Need**: Physicians must understand why model recommends treatment/diagnosis.

**Methods**:
- **Feature importance**: Which variables drive prediction
- **SHAP values**: Individual prediction explanations
- **Decision rules**: If-then logic physicians can follow

---

### **Algorithmic Bias**
**Definition**: Systematic errors that result in unfair treatment of different groups.

**Medical Examples**:
- Pulse oximeters less accurate for dark skin
- AI models trained predominantly on male patients
- Diagnostic tools performing poorly in certain ethnic groups

**Mitigation**: Diverse training data, fairness metrics, bias testing

---

### **Health Equity**
**Definition**: Everyone has fair opportunity to achieve optimal health.

**AI Considerations**:
- Equal performance across demographic groups
- Access to AI-enabled healthcare
- Avoiding perpetuation of healthcare disparities
- Cultural sensitivity in model design

---

## Machine Learning in Healthcare

### **Overfitting**
**Definition**: Model learns training data too specifically, performs poorly on new data.

**Medical Risk**: Model might memorize specific patient characteristics rather than learning generalizable disease patterns.

**Prevention**:
- Cross-validation
- Regularization (LASSO, Ridge)
- Independent test sets
- Bootstrap validation

---

### **Feature Engineering**
**Definition**: Creating new variables from raw data to improve model performance.

**Medical Examples**:
- BMI from height/weight
- Cardiovascular risk scores from multiple factors
- Temporal features from time-series vital signs
- Interaction terms between genetic variants

---

### **Class Imbalance**
**Definition**: When target classes have very different frequencies.

**Medical Example**: Cancer screening where 2% have cancer, 98% don't.

**Solutions**:
- Stratified sampling
- SMOTE (synthetic minority oversampling)
- Cost-sensitive learning
- Ensemble methods

---

### **Validation Strategies**

#### **Hold-out Validation**
Split data once: 70% train, 15% validation, 15% test.

#### **K-Fold Cross-Validation**
Split data into K parts, use K-1 for training, 1 for testing, repeat K times.

#### **Temporal Validation**
Train on older data, test on newer data (important for medical data with time trends).

#### **External Validation**
Test model on data from different hospitals/populations.

---

### **Regularization**

#### **L1 (LASSO)**
Penalty proportional to absolute coefficient values. Produces sparse models (feature selection).

#### **L2 (Ridge)**
Penalty proportional to squared coefficient values. Shrinks coefficients but doesn't eliminate features.

#### **Elastic Net**
Combines L1 and L2 penalties for balance between feature selection and coefficient shrinkage.

---

## Key Takeaways for Medical AI

1. **Statistical significance ≠ Clinical significance**
2. **Interpretability is crucial in healthcare**
3. **Bias testing is mandatory, not optional**
4. **External validation required before clinical deployment**
5. **Conservative approaches when patient safety is at stake**
6. **Regulatory compliance must be built in from the start**
7. **Clinical domain knowledge essential for feature engineering**
8. **Survival analysis for time-to-event outcomes**
9. **Multiple testing correction for high-dimensional data**
10. **Continuous monitoring required for deployed models**

---

*This glossary covers the essential concepts from our comprehensive biostatistics and medical AI analysis. Each term includes practical examples relevant to clinical applications and regulatory requirements.*