# Logistic regression

**Keywords**: logistic regression,linear,classifier

---

**Logistic regression** is a **classification algorithm that predicts probabilities of binary outcomes** (yes/no, true/false, positive/negative) using the logistic (sigmoid) function. Despite the name, it's for classification, not regression.

**What Is Logistic Regression?**

- **Type**: Classification algorithm (binary or multiclass)
- **Name Confusion**: "Regression" refers to the underlying technique
- **Output**: Probability (0-1) instead of continuous value
- **Decision Boundary**: Linear in input space
- **Interpretability**: Highly interpretable coefficients
- **Simplicity**: One of the simplest ML algorithms

**Why Logistic Regression Matters**

- **Simplicity**: Easy to understand and implement
- **Interpretability**: Clear feature importance
- **Speed**: Fast training and prediction
- **Probabilistic Output**: Confidence scores, not just predictions
- **Baseline**: Standard baseline for classification
- **Scalability**: Works with large datasets
- **Robustness**: Less prone to overfitting than complex models

**How It Works**

**Step 1: Linear Transformation**:
z = w₁x₁ + w₂x₂ + ... + wₙxₙ + b

**Step 2: Sigmoid Function** (Logistic Function):
σ(z) = 1 / (1 + e⁻ᶻ)

**Step 3: Output Probability**:
p = σ(z) where p ∈ [0, 1]

**Step 4: Classification**:
- If p > 0.5: Predict class 1
- If p ≤ 0.5: Predict class 0

**Visualization**: The sigmoid function is S-shaped curve from 0 to 1

**Python Implementation**

**Basic Usage**:
```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Train
model = LogisticRegression()
model.fit(X_train, y_train)

# Predict class
predictions = model.predict(X_test)

# Predict probability
probabilities = model.predict_proba(X_test)
# Returns [[prob_class_0, prob_class_1], ...]

# Evaluate
accuracy = accuracy_score(y_test, predictions)
print(classification_report(y_test, predictions))
```

**Use Cases**

**Medical Diagnosis**:
- Disease present/absent
- Will need treatment/not
- Excellent for healthcare

**Banking & Finance**:
- Loan default/no default
- Credit card fraud/legitimate
- Fast decisions, interpretable

**Customer Churn**:
- Will customer leave/stay
- Guide retention programs
- Actionable predictions

**Spam Detection**:
- Email spam/not spam
- Fast classification
- Email-level probability

**Marketing**:
- Will customer buy/not buy
- Click prediction
- Conversion probability

**Manufacturing**:
- Product defect/no defect
- Equipment failure/normal
- Quality control

**Advantages**

✅ **Simple & Fast**: Minimal computation
✅ **Interpretable**: Understand why predictions made
✅ **Probabilistic**: Get confidence scores
✅ **Well-behaved**: Mathematical guarantees
✅ **Baseline Model**: Good for comparison
✅ **Scaling**: Handles large datasets
✅ **Regularization**: Built-in options (L1, L2)

**Disadvantages**

❌ **Linear Boundary**: Can't capture complex patterns
❌ **Assumes Linear Relationship**: Features must linearly separate classes
❌ **Limited Interactions**: Doesn't automatically find feature interactions
❌ **Feature Engineering**: Needs manual feature preparation
❌ **Imbalanced Data**: Struggles with very skewed classes

**Regularization Techniques**

**L2 Regularization** (Ridge):
```python
# Default, most common
model = LogisticRegression(penalty='l2', C=1.0)
# C is inverse of regularization strength
# Smaller C = stronger regularization
```

**L1 Regularization** (Lasso):
```python
# Feature selection
model = LogisticRegression(
    penalty='l1',
    solver='liblinear',
    C=1.0
)
# L1 shrinks irrelevant features to zero
# Automatic feature selection
```

**Elastic Net** (L1 + L2):
```python
model = LogisticRegression(
    penalty='elasticnet',
    solver='saga',
    l1_ratio=0.5  # Mix of L1 and L2
)
```

**Multiclass Classification**

**One-vs-Rest** (OvR):
```python
# Train K binary classifiers (K = number of classes)
model = LogisticRegression(multi_class='ovr')
model.fit(X_train, y_train)
```

**Multinomial**:
```python
# Softmax extension of sigmoid
model = LogisticRegression(multi_class='multinomial')
model.fit(X_train, y_train)
```

**Feature Importance & Interpretation**

**Coefficients Tell the Story**:
```python
# Get coefficients
coefficients = model.coef_[0]

# Feature importance
for feature, coef in zip(feature_names, coefficients):
    if coef > 0:
        print(f"{feature}: +{coef:.3f} (increases prob of class 1)")
    else:
        print(f"{feature}: {coef:.3f} (decreases prob of class 1)")
```

**Coefficient Interpretation**:
- **Positive coefficient**: Increases probability of positive class
- **Negative coefficient**: Decreases probability
- **Larger magnitude**: Stronger influence
- **Zero coefficient**: Doesn't influence decision

**Handling Class Imbalance**

```python
# Option 1: Class weights
model = LogisticRegression(class_weight='balanced')
# Automatically adjusts for imbalanced classes

# Option 2: Specify manually
model = LogisticRegression(
    class_weight={0: 1, 1: 10}  # 10x weight for class 1
)

# Option 3: Adjust decision threshold
y_pred = (model.predict_proba(X_test)[:, 1] > 0.3).astype(int)
# Move threshold from 0.5 to 0.3 for more class 1 predictions
```

**Model Evaluation**

```python
from sklearn.metrics import (
    confusion_matrix, roc_auc_score, roc_curve, 
    precision_recall_curve, f1_score
)

# Confusion matrix
cm = confusion_matrix(y_test, predictions)

# ROC AUC (area under curve)
roc_auc = roc_auc_score(y_test, probabilities[:, 1])

# F1 Score (harmonic mean of precision and recall)
f1 = f1_score(y_test, predictions)

# Plot ROC curve
fpr, tpr, thresholds = roc_curve(y_test, probabilities[:, 1])
```

**Logistic Regression vs Alternatives**

| Algorithm | Complexity | Speed | Power | Use When |
|-----------|-----------|-------|-------|----------|
| Logistic Regression | Low | Fast | Simple patterns | Baseline, interpretability |
| Decision Tree | Medium | Fast | Complex patterns | Non-linear data |
| Random Forest | High | Medium | Very powerful | Best accuracy |
| Neural Network | Very High | Slow | Any pattern | Complex data |

**Best Practices**

1. **Normalize features**: Scale to [0,1] or standardize
2. **Handle missing values**: Drop or impute
3. **Encode categorical**: One-hot or label encoding
4. **Check assumptions**: No perfect separation
5. **Evaluate properly**: Use cross-validation
6. **Try regularization**: Prevent overfitting
7. **Handle imbalance**: If classes very skewed

Logistic regression is the **foundational classification algorithm** — while simple, it's powerful enough for many real problems and serves as the essential baseline against which all other classifiers are compared.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/logistic-regression) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
