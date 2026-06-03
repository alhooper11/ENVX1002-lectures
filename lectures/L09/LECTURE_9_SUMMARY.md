# Lecture 9: Describing Relationships — Complete Summary Sheet

## 📋 Learning Outcomes

By the end of Lecture 9, you will be able to:

1. **Explore and describe datasets** using R and Excel
2. **Visualize and calculate probability distributions** 
3. **Apply statistical inference methods** appropriately
4. **Apply linear and non-linear models** to describe relationships between variables ⭐ (Focus of L9)
5. **Articulate statistical results** clearly in written and oral formats

---

## 🎯 Lecture 9 Focus

**Week 9: Describing Relationships**
- Correlation (calculation, interpretation)
- Regression (model structure, model fitting)
- What/when/why/how

**Context:** Foundation for Weeks 10-12
- Week 10: Simple Linear Regression (assumptions, hypothesis testing, model fit)
- Week 11: Multiple Linear Regression (MLR modelling, parsimony)
- Week 12: Nonlinear Regression (polynomials, exponentials, logarithms, transformations)

---

## 📊 PART 1: CORRELATION

### What is Correlation?

**Definition:** A number between **-1 and 1** describing the relationship between two **continuous variables**.

#### Direction & Strength

| Correlation Value | Interpretation |
|-------------------|-----------------|
| **+1** | Perfect positive relationship (both increase together) |
| **+0.7 to +1.0** | **Strong positive** |
| **+0.4 to +0.6** | **Moderate positive** |
| **+0.1 to +0.3** | **Weak positive** |
| **0** | No relationship |
| **-0.1 to -0.3** | **Weak negative** |
| **-0.4 to -0.6** | **Moderate negative** |
| **-0.7 to -1.0** | **Strong negative** |
| **-1** | Perfect negative relationship (opposite increase) |

**Note:** Strength categories (~0.1-0.3, ~0.4-0.6, ~0.7-1.0) are useful but subjective and vary by field.

---

### Pearson's Correlation Coefficient (r)

#### Formula
$$r = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^n (x_i - \bar{x})^2 \sum_{i=1}^n (y_i - \bar{y})^2}}$$

**In words:** Covariance divided by the product of standard deviations.

#### Characteristics
- ✅ **Most commonly used** correlation coefficient
- ✅ Developed by Karl Pearson (1800s), based on Francis Galton's work
- ⚠️ **Assumes normally distributed data** (parametric)
- ⚠️ **For linear relationships ONLY** (detects straight-line patterns)

#### When to Use
- Data appears normally distributed
- You expect a straight-line (linear) relationship
- You want the most statistically powerful test

#### R Code
```r
# Calculate Pearson correlation
cor(x, y)  # Default method is Pearson
cor(x, y, method = "pearson")  # Explicit
```

#### Excel
```
=CORREL(array1, array2)
```

---

### Example: Galton's Data

**Dataset:** 928 children of 205 parent pairs; heights measured in inches

**Interpretation:**
```r
cor(Galton$parent, Galton$child)  # r ≈ 0.46
```

**Conclusion:** "There is a **moderate positive linear relationship** between parent height and child height."
- Direction: **Positive** (taller parents → taller children)
- Strength: **Moderate** (r ≈ 0.46 falls in 0.4-0.6 range)
- Pattern: **Linear** (scatterplot shows straight-line trend)

---

### ⚠️ Critical Limitation: Anscombe's Quartet

**What it demonstrates:**
Four completely different datasets with:
- ✅ **Identical correlation coefficients** (r ≈ 0.82)
- ✅ **Identical means and standard deviations**
- ✅ **Identical regression lines**

**Yet their scatterplots show:**
1. Linear relationship (good for regression) ✅
2. Curved relationship (needs nonlinear model) ❌
3. Perfect pattern with one outlier ❌
4. Horizontal line with one extreme outlier ❌

**Key Lesson:**
> **"Correlation coefficients are NOT reliable in inferring the 'type' of relationship between variables — WE MUST VISUALIZE."**

**Takeaway:** Always plot your data before interpreting correlation!

---

### ⚠️ Datasaurus Dozen

**What it shows:**
12 datasets with **correlation coefficient close to zero**, yet scatterplots show wildly different patterns:
- Some show clear clustering
- Some show circular patterns
- Some show diagonal lines
- Some are completely random

**Same lesson as Anscombe's:** Summary statistics alone are insufficient. **Visualization is essential.**

---

### Monotonic vs. Linear Relationships

| Concept | Definition | Example |
|---------|-----------|---------|
| **Linear** | Relationship increasing/decreasing at a **constant rate** (straight line) | y = 2x + 5 |
| **Monotonic** | Relationship that is **consistently increasing or decreasing** (any shape) | y = log(x), y = e^x |

**Visual example:**
- Straight line → Linear AND Monotonic ✅
- Exponential curve (y = e^x) → Monotonic BUT NOT Linear
- Logarithm curve (y = log x) → Monotonic BUT NOT Linear
- Parabola/quadratic → NEITHER linear nor monotonic (changes direction)
- Sigmoid/S-curve → NEITHER linear nor monotonic

---

### Spearman's Rank Correlation

#### Characteristics
- ✅ For **monotonic relationships** (all directions, any shape)
- ✅ **Non-parametric** (doesn't assume normality)
- ✅ More **robust to outliers** than Pearson's
- ⚠️ More "conservative" (values can be smaller in magnitude)

#### When to Use
- Data is non-normally distributed
- Relationship might be curved (log, exponential, etc.)
- You need robustness against outliers

#### R Code
```r
cor(x, y, method = "spearman")
```

---

### Kendall's Tau Correlation

#### Characteristics
- ✅ For **monotonic relationships**
- ✅ **Non-parametric** and rank-based
- ✅ **Most robust to outliers** (more conservative than Spearman's)
- ✅ Better for **ordinal/non-normal data**

#### When to Use
- Need maximum robustness to outliers
- Data is ordinal or heavily non-normal
- Prefer conservative estimates

#### R Code
```r
cor(x, y, method = "kendall")
```

---

### Comparison: Pearson vs. Spearman vs. Kendall

**Example: Logarithmic Relationship (y = log x)**

| Method | Value | Why |
|--------|-------|-----|
| **Pearson's r** | ~0.76 | Detects linear patterns; log curve isn't perfectly linear |
| **Spearman's ρ** | 1.00 | Uses ranks; captures perfect monotonic increase |
| **Kendall's τ** | ~0.95 | More conservative rank-based method |

**Key insight:** The same monotonic relationship shows different correlations depending on which method measures linearity vs. monotonicity.

---

### ⚠️ Correlation ≠ Causation

**Critical Principle:**
> A relationship between two variables does **NOT** imply that one causes the other.

#### Why?

1. **Confounding Variables:** A third variable causes both
   - Example: Ice cream sales correlate with drowning deaths (confound: summer weather)
   
2. **Reverse Causation:** B might cause A (not A causes B)
   
3. **Coincidence:** Spurious correlation with no mechanism
   
4. **Observational Data:** Can only show associations, not causation

#### Example from Lecture
- **Claim:** "Higher insulin levels cause lower blood glucose"
- **Correlation:** r ≈ -0.90 (strong negative)
- **Reality:** Insulin is administered *because* blood glucose is high
- **Reverse causation:** Blood glucose causes insulin response

---

## 📈 PART 2: REGRESSION MODELLING

### What is Regression?

**Definition:** A statistical method to fit a model to data that describes the relationship between variables.

**Simple linear regression:** Fits a straight line (line of best fit) between a response variable (Y) and a predictor variable (x).

---

### Why Use Regression?

#### 1. **Describe the Relationship**
What is the relationship between response Y and predictor x?

**Terminology:**
- Y = response, dependent variable, target, outcome
- x = predictor, independent variable, feature, input

#### 2. **Explain the Relationship**
How much variation in Y can be explained by x?

#### 3. **Predict New Values**
What is Y for a given value of x?
- Often: x is easy to measure, Y is hard/expensive to measure
- Example: Predict child height from parent height

---

### Types of Regression Models

| Model Type | Example |
|------------|---------|
| **Simple Linear Regression** | y = β₀ + β₁x |
| **Multiple Linear Regression** | y = β₀ + β₁x₁ + β₂x₂ + ... |
| **Polynomial** | y = β₀ + β₁x + β₂x² + β₃x³ |
| **Exponential** | y = ae^(bx) |
| **Logarithmic** | y = β₀ + β₁log(x) |

---

### Historical Context

Three key figures developed regression:

1. **Adrien-Marie Legendre** (1805)
   - Proposed the **Method of Least Squares**

2. **Carl Friedrich Gauss** (1809)
   - First *used* least squares (fitting asteroid Ceres orbit)

3. **Francis Galton** (1886)
   - First published on **model fitting** (predicting child height from parent height)

---

## 🔧 PART 3: SIMPLE LINEAR REGRESSION (SLR) MODEL

### Model Equation

$$Y_i = \beta_0 + \beta_1 x_i + \epsilon_i$$

where $\epsilon_i \sim N(0, \sigma^2)$

### Components

| Component | Definition | Type |
|-----------|-----------|------|
| **Y_i** | Observed value of response variable for observation i | **Data** |
| **β₀** | Y-intercept; predicted value when x = 0; "the constant" | **Fixed parameter** |
| **β₁** | Slope; change in Y for 1-unit increase in x | **Fixed parameter** |
| **x_i** | Predictor/independent variable for observation i | **Data** |
| **ε_i** | Error/residual term; unexplained variation | **Random/variable** |

### Alternative Phrasings

Different ways to conceptualize the response:

```
Response = Prediction + Error
Response = Signal + Noise
Response = Model + Unexplained
Response = Deterministic + Random
Response = Explainable + Everything Else
Y = f(x)
```

---

### Method of Least Squares

#### The Core Idea

When we fit a line, there is **error** (residual) between observed and predicted values:

$$\hat{\epsilon_i} = y_i - \hat{y_i}$$

**Least squares minimizes the sum of squared errors:**

$$\sum_{i=1}^n (\hat{\epsilon_i})^2$$

#### Why Square?

**Problem without squaring:** Positive and negative errors cancel out
- Observed = 10, Predicted = 8 → error = +2
- Observed = 6, Predicted = 8 → error = -2
- Sum = 0 (misleadingly suggests perfect fit!)

**Solution:** Square each error
- (+2)² = 4
- (-2)² = 4
- Sum = 8 (correctly penalizes all deviations)

#### How It Works

Computer iteratively adjusts the slope and intercept until the sum of squared errors stabilizes (reaches minimum).

---

### Calculating Regression Parameters

#### Slope (β₁) — Analytical Formula

$$\beta_1 = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^n (x_i - \bar{x})^2} = \frac{\text{Cov}(x,y)}{\text{Var}(x)} = \frac{SS_{xy}}{SS_{xx}}$$

**Interpretation:**
- β₁ = covariance / variance of x
- Shows how sensitive Y is to changes in x
- Units: (units of Y) per (units of x)

#### Intercept (β₀) — Analytical Formula

$$\beta_0 = \bar{y} - \beta_1 \bar{x}$$

**Interpretation:**
- β₀ = predicted value when x = 0
- Line passes through the point ($\bar{x}$, $\bar{y}$)

---

### Model Fitting Approaches

#### 1. Analytical
- Use direct equations (formulas above)
- Finds exact solution immediately
- Used for simple models

#### 2. Numerical
- Computer makes "random guesses" to find parameters
- Iteratively minimizes objective function (residual sum of squares)
- Used for complex models (can't solve analytically)

---

## 💻 PART 4: FITTING SLR IN R

### Basic Syntax

```r
fit <- lm(child ~ parent, data = Galton)
```

**Interpretation:**
- `lm()` = linear model function
- `child ~ parent` = formula (response ~ predictor)
- `data = Galton` = dataset

### Extracting Results

```r
summary(fit)  # Full summary output
```

**Output includes:**
- Coefficients (β₀, β₁)
- Standard errors
- t-values
- p-values
- R-squared
- F-statistic

---

### Example: Galton's Data

```r
fit <- lm(child ~ parent, data = Galton)
summary(fit)
```

**Hypothetical Output:**
```
Coefficients:
              Estimate Std. Error t value Pr(>|t|)
(Intercept)   23.9415    2.8117   8.514   <2e-16 ***
parent         0.6463    0.0411  15.723   <2e-16 ***
```

#### Interpretation

- **β₀ = 23.9415:** Child height is predicted to be ~24 inches when parent height = 0 inches (extrapolation; not meaningful)
- **β₁ = 0.6463:** For each 1-inch increase in parent height, child height increases by ~0.65 inches
- **Fitted equation:** $\hat{Y} = 23.94 + 0.6463 \times \text{parent height}$

---

## 🧪 PART 5: HYPOTHESIS TESTING IN SLR

### Null vs. Alternative Hypotheses

| Hypothesis | Meaning |
|-----------|---------|
| **H₀: β₁ = 0** | Slope is zero; no relationship; mean ($\bar{y}$) is better predictor than x |
| **H₁: β₁ ≠ 0** | Slope is not zero; relationship exists; linear model is better than mean |

### Conceptual Comparison

**Null Model:** $Y_i = \beta_0 + \epsilon_i$
- Prediction: $\hat{Y} = \bar{y}$ (the mean)
- No role for x

**SLR Model:** $Y_i = \beta_0 + \beta_1 x_i + \epsilon_i$
- Prediction: $\hat{Y} = \beta_0 + \beta_1 x_i$
- x has explanatory power

**Question:** Which model fits the data better?

### Interpretation of p-value

From Galton example:
```
parent: Pr(>|t|) = <2e-16
```

- **p-value < 0.05:** Reject H₀
- **Conclusion:** β₁ is significantly different from zero; parent height significantly predicts child height

---

## 📊 PART 6: CORRELATION ANALYSIS → REGRESSION MODELLING

### Workflow

#### Step 1: Calculate Correlation
- Quick and easy
- Identifies which variables have strong linear relationships
- Fast screening tool

**Example:** In iris dataset, Petal.Length and Petal.Width are highly correlated (r = 0.96)

#### Step 2: Visualize
- Create scatterplot with regression line
- Check for linearity, outliers, patterns
- Ensure relationship makes sense

#### Step 3: Ask Questions
- Is there a *theoretical reason* to expect a relationship?
- Do I have a *hypothesis* about causation?
- Should I build a regression model?

#### Step 4: Fit Regression
- If theoretical justification exists → fit model
- Test hypothesis (β₁ = 0?)
- Interpret coefficients
- Assess model quality (next weeks: L10-L11)

---

### Why Start with Correlation?

✅ **Advantages:**
- Summarizes relationship quickly
- Identifies multicollinearity candidates (important for MLR, L11)
- Guides variable selection for regression

❌ **Limitations:**
- Summary statistic hides distributional details (Anscombe's, Datasaurus)
- Doesn't establish causation
- Assumes linear relationship (Pearson's only)

---

## 🎯 KEY DISTINCTIONS

### Correlation vs. Regression

| Aspect | Correlation | Regression |
|--------|-------------|-----------|
| **What it measures** | Strength & direction of relationship | Fit a predictive model |
| **Symmetry** | Symmetric (r(x,y) = r(y,x)) | Asymmetric; Y is response |
| **Use case** | Quick screening | Prediction & explanation |
| **Causation** | Never implies causation | Can test hypotheses IF theoretically justified |
| **Output** | Single number (-1 to 1) | Equation + parameters + statistics |

### Predictor vs. Response (⚠️ Don't Confuse!)

| Term | Role | Example |
|------|------|---------|
| **Predictor (x)** | Independent variable; what we use to predict | Parent height |
| **Response (Y)** | Dependent variable; what we're predicting | Child height |

**Formula:** $Y = f(\text{x})$ means Y depends on x

---

## 📋 QUICK REFERENCE: WHEN TO USE WHAT

### Choose Correlation When:
✅ You want to quickly assess relationship strength  
✅ You have no specific hypothesis about causation  
✅ You're screening variables before regression  
✅ You want to identify multicollinearity  

### Choose Regression When:
✅ You have a hypothesis about how one variable predicts another  
✅ You want to estimate the magnitude of the effect (β₁)  
✅ You want to make predictions  
✅ You want to test causation (with theoretical justification)  

### Choose Pearson's r When:
✅ Data appears normally distributed  
✅ Relationship appears linear  

### Choose Spearman's or Kendall's When:
✅ Data is non-normal  
✅ Relationship is monotonic but not linear  
✅ You want robustness to outliers  

---

## 🚨 COMMON MISTAKES & HOW TO AVOID THEM

| Mistake | Correction |
|---------|-----------|
| Assuming correlation means causation | Remember: correlation ≠ causation. Need experimental design or theory. |
| Trusting summary statistics alone | **Always visualize!** (Anscombe's Quartet) |
| Using Pearson's r for curved data | Use Spearman's or Kendall's for monotonic (non-linear) relationships |
| Confusing x and Y | Y = response (what you predict); x = predictor (what you use) |
| Forgetting to square errors | Squaring prevents positive/negative cancellation |
| Interpreting intercept literally | β₀ = Y when x=0; often not meaningful if x=0 is unrealistic |
| Over-interpreting p-values | p-value tells if β₁ ≠ 0, not if model is good (see L10) |

---

## 📚 FORMULA SUMMARY

| Concept | Formula |
|---------|---------|
| **Pearson correlation** | $r = \frac{\text{Cov}(x,y)}{\text{SD}(x) \times \text{SD}(y)}$ |
| **SLR Model** | $Y_i = \beta_0 + \beta_1 x_i + \epsilon_i$ |
| **Slope** | $\beta_1 = \frac{\sum(x_i - \bar{x})(y_i - \bar{y})}{\sum(x_i - \bar{x})^2}$ |
| **Intercept** | $\beta_0 = \bar{y} - \beta_1 \bar{x}$ |
| **Least Squares Criterion** | Minimize $\sum_{i=1}^n (\hat{\epsilon_i})^2$ |

---

## 🎬 LOOKING AHEAD

**Lecture 10: Simple Linear Regression (Continued)**
- ✅ Assumptions (normality, linearity, homogeneity of variance, independence)
- ✅ Hypothesis testing on β₁
- ✅ Model fit: R², residuals, diagnostics
- ✅ Interpretation of SLR output

**Lecture 11: Multiple Linear Regression**
- ✅ Multiple predictors
- ✅ Multicollinearity issues
- ✅ Principle of parsimony (simpler is better)

**Lecture 12: Nonlinear Regression**
- ✅ Polynomial models
- ✅ Exponential & logarithmic models
- ✅ Transformations

---

## 🎓 Learning Outcome Checklist

By the end of Lecture 9, you should be able to:

- [ ] **Explain correlation:** What it is, what it measures, its limitations
- [ ] **Distinguish methods:** Pearson's vs. Spearman's vs. Kendall's (when to use each)
- [ ] **Calculate & interpret:** Correlation coefficients in R and Excel
- [ ] **Identify patterns:** Linear vs. monotonic relationships from scatterplots
- [ ] **Avoid pitfalls:** Understand correlation ≠ causation; visualize data (Anscombe's)
- [ ] **Explain regression:** Why we use it (describe, explain, predict)
- [ ] **Understand least squares:** Why we minimize squared errors
- [ ] **Fit SLR models:** Use `lm()` in R
- [ ] **Interpret outputs:** β₀, β₁, p-values, fitted equations
- [ ] **Test hypotheses:** H₀: β₁ = 0 vs. H₁: β₁ ≠ 0
- [ ] **Connect concepts:** How to go from correlation analysis → regression modelling

---

**Next Week (L10):** Assumptions, hypothesis testing, model diagnostics, and interpreting R²

**Good luck! 📊**
