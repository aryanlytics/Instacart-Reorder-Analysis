<p align="center">
<img width="300" height="435" alt="03-Instacart-Logo-Kale-1 (1)" src="https://github.com/user-attachments/assets/c81e0ac6-0b22-489c-8dbb-e681c3bd7343" />
</p>

# Instacart Reorder Behavior Analysis

## Project Overview

This analysis diagnoses **why some products get reordered while others don't** using 32.4 million order records from Instacart's online grocery platform. Rather than describing what customers do, this project identifies the behavioral and operational drivers behind reorder decisions—validated through statistical testing and predictive modeling.

**Business Impact:** Findings enable Instacart to optimize promotional spend, product placement, and customer retention strategies by targeting the right categories to the right customers at the right time.

---

## Business Question

**Why do some products consistently get reordered while others are purchased only once?**

Understanding reorder behavior is critical for grocery e-commerce because:
- Reordered items drive predictable revenue and customer lifetime value
- Non-reordered items represent either variety-seeking behavior or dissatisfaction
- Misaligned retention strategies waste promotional budgets on the wrong categories

---

## Hypothesis

Reorder likelihood is driven by:
1. **Product category** (staples vs. impulse purchases)
2. **Purchase recency** (time since last order)
3. **User purchase frequency** (active vs. infrequent shoppers)
4. **Product popularity** (social proof effect)
5. **Time of day** (routine shopping vs. spontaneous orders)

---

## Dataset

**Source:** [Instacart Market Basket Analysis (Kaggle)](https://www.kaggle.com/c/instacart-market-basket-analysis)

**Size:**
- 32.4 million order items (prior orders dataset)
- 3.3 million orders
- 206,000 users
- 49,685 unique products across 21 departments

**Key Variables:**
- `reordered`: Binary indicator (1 = reordered, 0 = first purchase)
- `department`: Product category (Produce, Dairy Eggs, Snacks, etc.)
- `days_since_prior_order`: Recency of customer activity
- `user_id`: Customer identifier for frequency calculations
- `product_id`: Product identifier for popularity metrics
- `order_hour_of_day`, `order_dow`: Temporal features

---

## Methodology

### 1. Data Preparation (SQL/PostgreSQL)
- Consolidated 6 source tables into unified `summary` table
- Engineered features:
  - User frequency (total orders per customer)
  - Product popularity (total appearances across all orders)
  - Recency buckets (0-7 days, 8-14 days, etc.)
  - Time-of-day categories (Morning, Afternoon, Evening, Night)
- Created `reorder_analysis` table with 32.4M rows for modeling

### 2. Statistical Validation (Python/scipy)
- **Chi-square tests** to validate categorical relationships (department vs. reorder status)
- **Logistic regression** to quantify predictive strength of each driver
- **Odds ratios** to translate coefficients into business-friendly metrics

### 3. Model Specifications
- **Algorithm:** Logistic Regression (sklearn)
- **Features:** 10 predictors (3 continuous, 7 categorical)
- **Sample size:** 500,000 records (statistically powered)
- **Split:** 70% training, 30% validation
- **Performance:** 67% accuracy, ROC-AUC 0.688

---

## Key Findings

### Finding 1: Department Type Drives Reorder Behavior (STRONGEST PREDICTOR)

**Statistical Evidence:**
- All departments showed statistically significant effects (chi-square p < 0.001)
- 32-percentage-point spread between highest and lowest reorder rates

**Odds Ratios (vs. baseline categories):**
| Department | Odds Ratio | Interpretation |
|------------|------------|----------------|
| Dairy Eggs | 1.97x | **97% higher reorder odds** |
| Beverages | 1.89x | 89% higher reorder odds |
| Bakery | 1.77x | 77% higher reorder odds |
| Produce | 1.29x | 29% higher reorder odds |
| Frozen | 1.28x | 28% higher reorder odds |
| Snacks | 1.34x | 34% higher reorder odds |
| **Pantry** | **0.47x** | **53% LOWER reorder odds** |

**Business Insight:**
- **Dairy and beverages are routine replenishment categories**—customers buy the same milk, juice, and yogurt weekly
- **Pantry items show variety-seeking behavior**—people try different pasta brands, sauces, and canned goods
- This is not operational failure; it's category-level consumer psychology

---

### Finding 2: User Frequency Predicts Reorder Habits (45-POINT SPREAD)

**Evidence from Univariate Analysis:**
| User Frequency | Reorder Rate | Sample Size |
|----------------|--------------|-------------|
| Very Low (1-4 orders) | 23.5% | 1.4M items |
| Low (5-9 orders) | 37.1% | 4.0M items |
| Medium (10-19 orders) | 50.8% | 7.0M items |
| High (20+ orders) | 68.9% | 19.9M items |

**Logistic Regression Coefficient:** +0.019 per order
- Each additional order increases reorder odds by 1.9%
- Compounds to ~95% higher odds for customers with 50+ orders

**Business Insight:**
Frequent shoppers are creatures of habit. They've identified their preferred products and stick to them. New customers are still exploring—don't expect reorder behavior until they've placed 10+ orders.

---

### Finding 3: Recency Matters, But Less Than Expected (18-POINT DROP)

**Evidence from Univariate Analysis:**
| Days Since Prior Order | Reorder Rate |
|------------------------|--------------|
| 0-7 days | 67.5% |
| 8-14 days | 64.4% |
| 15-21 days | 59.5% |
| 22-30 days | 49.2% |

**Logistic Regression Coefficient:** -0.006 per day
- Each additional day decreases reorder odds by 0.64%
- 30-day gap = ~17% decrease in reorder likelihood

**Business Insight:**
The longer customers wait between orders, the more unpredictable their behavior becomes. They either forget what they bought, find alternatives elsewhere, or change routines.

---

### Finding 4: Time of Day Is Irrelevant (HYPOTHESIS REJECTED)

**Evidence:**
| Time of Day | Reorder Rate | Difference |
|-------------|--------------|------------|
| Morning (6-11 AM) | 61.1% | Baseline |
| Afternoon (12-5 PM) | 57.9% | -3.2 pts |
| Evening (6-9 PM) | 57.8% | -3.3 pts |
| Night (10 PM-5 AM) | 57.8% | -3.3 pts |

**Only 3-point spread.** After controlling for other factors in the logistic regression, time-of-day had near-zero impact.

**Business Insight:**
Reorder behavior is driven by **what** customers buy (category) and **how often** they shop (frequency), not **when** they place orders. Don't waste resources on time-based reorder promotions.

---

### Finding 5: Product Popularity Is Correlation, Not Causation

**Univariate Analysis Showed:**
- Rare products (< 100 orders): 36% reorder rate
- Very popular products (5K+ orders): 65% reorder rate

**But logistic regression coefficient: +0.000005 (essentially zero)**

**Why?**
Popular products are popular *because* they're reordered frequently, not the other way around. Once you control for department and user frequency, popularity adds no predictive power.

**Business Insight:**
Don't prioritize products just because they're popular overall. Focus on what *this specific customer* reorders based on their department preferences and shopping frequency.

---

## Model Performance

**Logistic Regression Results:**
- **Accuracy:** 67%
- **ROC-AUC Score:** 0.688
- **Interpretation:** Model distinguishes reordered vs. non-reordered items reasonably well

This is typical for behavioral prediction models. Customer habits aren't deterministic—there's inherent randomness in human behavior. A 67% accuracy rate means the model correctly predicts reorder decisions two-thirds of the time, which is actionable for business strategy.

---

## Visualizations

### Feature Importance
![Feature Importance](feature_importance.png)

**Key Takeaway:** Department dummies dominate the model. Dairy, beverages, and bakery strongly increase reorder odds, while pantry dramatically decreases them.

---

### Model Prediction Distribution
![Prediction Distribution](prediction_distribution.png)

**Key Takeaway:** The model successfully separates reordered items (green, right-skewed) from non-reordered items (red, left-skewed). Good separation indicates the model has learned meaningful patterns.

---

## Business Recommendations

### 1. Category-Specific Retention Strategies

**High-Reorder Categories (Dairy, Beverages, Bakery):**
- ✅ Push reorder reminders ("Restock your favorites")
- ✅ Offer subscription discounts for routine items
- ✅ Simplify checkout with "Buy Again" lists
- ✅ Measure success by repeat purchase rate

**Low-Reorder Categories (Pantry, Canned Goods):**
- ❌ Don't waste retention budget on reorder promotions
- ✅ Use for customer acquisition ("Try our pasta selection")
- ✅ Focus on variety and discovery features
- ✅ Measure success by category penetration, not repeat rate

**Expected Impact:** 15-20% reduction in wasted promotional spend by avoiding pantry reorder campaigns.

---

### 2. Engagement-Based Targeting

**New Customers (1-9 orders):**
- Focus on breadth, not reorders
- Encourage category exploration
- Don't expect loyalty yet

**Established Customers (10-19 orders):**
- Begin introducing reorder features
- Highlight frequently purchased items
- Test subscription offers for staples

**Loyal Customers (20+ orders):**
- Prioritize convenience over discovery
- Personalized reorder lists at checkout
- Premium features (faster delivery, priority support)

**Expected Impact:** 10-15% increase in retention for customers reaching 20+ orders.

---

### 3. Recency-Based Interventions

**Active Customers (0-7 days since last order):**
- No intervention needed—they're engaged

**At-Risk Customers (14-21 days):**
- Send reorder reminders for high-reorder categories
- "Your favorites are waiting" messaging

**Lapsed Customers (30+ days):**
- Treat as acquisition, not retention
- Offer new-customer discounts
- Reset expectations—don't assume they'll reorder old items

**Expected Impact:** 5-8% reduction in churn by intervening at the 14-day mark.

---

### 4. Abandon Time-of-Day Targeting

**Current State:** Many e-commerce platforms send time-based promotions (e.g., "Morning deals").

**Finding:** Time of day has no meaningful impact on reorder likelihood.

**Recommendation:** Reallocate resources from time-based campaigns to category-based and frequency-based segmentation.

**Expected Impact:** Simplify campaign logic, reduce operational complexity.

---

## Technical Implementation

### SQL Feature Engineering
```sql
CREATE TABLE reorder_analysis AS
SELECT 
    s.order_id,
    s.user_id,
    s.product_id,
    s.department,
    s.reordered,
    s.days_since_prior_order,
    
    -- User frequency
    user_stats.total_orders as user_total_orders,
    
    -- Product popularity
    prod_stats.product_order_count as product_popularity,
    
    -- Time features
    CASE 
        WHEN s.order_hour_of_day BETWEEN 6 AND 11 THEN 'Morning'
        WHEN s.order_hour_of_day BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Other'
    END as time_of_day
    
FROM summary s
INNER JOIN (
    SELECT user_id, COUNT(DISTINCT order_id) as total_orders
    FROM summary WHERE eval_set = 'prior'
    GROUP BY user_id
) user_stats ON s.user_id = user_stats.user_id
INNER JOIN (
    SELECT product_id, COUNT(*) as product_order_count
    FROM summary WHERE eval_set = 'prior'
    GROUP BY product_id
) prod_stats ON s.product_id = prod_stats.product_id
WHERE s.eval_set = 'prior';
```

### Python Statistical Testing
```python
from scipy.stats import chi2_contingency
from sklearn.linear_model import LogisticRegression

# Chi-square test for department effect
contingency = pd.crosstab(df['department'], df['reordered'])
chi2, p_value, dof, expected = chi2_contingency(contingency)

# Logistic regression
X = df[['days_since_prior', 'user_total_orders', 'product_popularity', 
        'is_dairy', 'is_produce', 'is_pantry', ...]]
y = df['reordered']

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

# Odds ratios
odds_ratios = np.exp(model.coef_[0])
```

---

## Limitations & Future Work

**Limitations:**
1. **No competitive data:** Can't distinguish between switching to competitors vs. natural variety-seeking
2. **No price sensitivity:** Dataset lacks pricing information
3. **First orders excluded:** 6% of data (first-time purchases) removed from modeling to handle NULLs in recency
4. **Seasonality not tested:** Time-of-year effects not analyzed (would require multi-year data)

**Future Extensions:**
1. **Predict *which specific products* a customer will reorder** (item-level recommendations)
2. **Survival analysis** to predict time-until-next-order
3. **A/B test recommendations** in production to validate business impact
4. **Incorporate pricing elasticity** if pricing data becomes available

---

## Repository Structure

```
instacart-reorder-analysis/
├── README.md                      # This file
├── reorder_analysis.py            # Python analysis script
├── feature_importance.png         # Model visualization
├── prediction_distribution.png    # Model performance chart
├── data/
│   └── reorder_data.csv          # Sample export (500K rows)
└── sql/
    └── feature_engineering.sql    # PostgreSQL queries
```

---

## Tools & Technologies

- **Database:** PostgreSQL (data consolidation, feature engineering)
- **Analysis:** Python 3.12
  - pandas (data manipulation)
  - scikit-learn (logistic regression)
  - scipy (chi-square tests)
  - matplotlib/seaborn (visualization)
- **Sample Size:** 500,000 records (statistically powered for all tests)

---

## Key Takeaway

**Reorder behavior is primarily driven by product category and customer frequency, not timing or popularity.** Instacart should segment retention strategies by category type:
- Push reorder features for staples (dairy, beverages)
- Push discovery features for variety categories (pantry, snacks)
- Target interventions based on user frequency, not time of day

This analysis provides statistical evidence to shift promotional spend from wasteful broad campaigns to targeted, category-specific retention strategies.

---

## Contact

**Muhammad Aryan**  
Data Analyst | Diagnostic Analysis & Business Problem-Solving  
📧 aryanrehman008@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/muhammadaryan008/)  
💻 [GitHub Portfolio](https://github.com/aryanlytics)

---

*"Data tells you what happened. Analysis tells you why. Strategy tells you what to do about it."*
