<p align="center">
<img width="300" height="435" alt="03-Instacart-Logo-Kale-1 (1)" src="https://github.com/user-attachments/assets/c81e0ac6-0b22-489c-8dbb-e681c3bd7343" />
</p>

# Instacart Reorder Behavior Analysis

## **Instacart**

Instacart is a leading North American grocery delivery platform that partners with supermarkets and retail chains to offer same-day delivery. Customers shop online, Instacart sends personal shoppers to pick and deliver the orders, and retailers gain an extended digital channel without building their own fulfillment network.

The platform captures detailed behavioral data: product choices, order timing, repeat purchases, and cart composition across millions of transactions. This makes Instacart a rich environment for understanding how customers form grocery habits.

---

## The Business Problem

Instacart operates in a category where repeat purchasing is the core of the business model. If a product keeps getting reordered, Instacart earns predictable margin and retention improves. If not, Instacart must keep spending to reacquire demand.

Across 32.4 million orders, the company noticed a strong imbalance:

- Some items (milk, bananas, eggs) are reordered at extremely high rates.

- Others (new snacks, beverages, or niche items) are purchased once and never show up again.

This raises a fundamental performance question for both Instacart and retailer partners:
Is low reorder behavior caused by customer preferences, product issues, or operational factors?


-----



## Core Question

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
**Source:** InstaCart Online Grocery Basket Analysis Dataset (**Kaggle**)      
[View Dataset](https://www.kaggle.com/datasets/yasserh/instacart-online-grocery-basket-analysis-dataset)


---
# **Executive Summary**

## **The Findings**






### **Evidence**




### **The Root Cause** 






### **Business Impact**







# **Analysis & Diagnosis**

### 1: Department Type Drives Reorder Behavior (STRONGEST PREDICTOR)

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

### 2: User Frequency Predicts Reorder Habits (45-POINT SPREAD)

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

### 3: Recency Matters, But Less Than Expected (18-POINT DROP)

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

### 4: Time of Day Is Irrelevant (HYPOTHESIS REJECTED)

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

### 5: Product Popularity Is Correlation, Not Causation

**Univariate Analysis Showed:**
- Rare products (< 100 orders): 36% reorder rate
- Very popular products (5K+ orders): 65% reorder rate

**But logistic regression coefficient: +0.000005 (essentially zero)**

**Why?**
Popular products are popular *because* they're reordered frequently, not the other way around. Once you control for department and user frequency, popularity adds no predictive power.

**Business Insight:**
Don't prioritize products just because they're popular overall. Focus on what *this specific customer* reorders based on their department preferences and shopping frequency.

---
## **Key Insight:


---

## Visualizations

### Feature Importance

<img width="1000" height="700" alt="feature_importance" src="https://github.com/user-attachments/assets/bea37a78-b132-4188-b0b7-8ab1d1651060" />


**Key Takeaway:** Department dummies dominate the model. Dairy, beverages, and bakery strongly increase reorder odds, while pantry dramatically decreases them.

---

### Model Prediction Distribution

<img width="1000" height="700" alt="prediction_distribution" src="https://github.com/user-attachments/assets/619ef999-3e5b-4294-99af-900a3f0f58d3" />


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
