<p align="center">
<img width="300" height="435" alt="03-Instacart-Logo-Kale-1 (1)" src="https://github.com/user-attachments/assets/c81e0ac6-0b22-489c-8dbb-e681c3bd7343" />
</p>

# **Instacart Customer Behavior Analysis**

## **About Instacart**

Instacart is a leading North American grocery delivery platform that partners with supermarkets and retail chains to offer same-day delivery. Customers shop online, Instacart sends personal shoppers to pick and deliver the orders, and retailers gain an extended digital channel without building their own fulfillment network.

The platform captures detailed behavioral data: product choices, order timing, repeat purchases, and cart composition across millions of transactions. This makes Instacart a rich environment for understanding how customers form grocery habits.

---

## **The Business Problem**

Instacart operates in a low-margin, high-volume business where two metrics determine profitability:

**1. Reorder Rate (Customer Retention)**
If a product keeps getting reordered, Instacart earns predictable margin and retention improves. If not, Instacart must keep spending to reacquire demand.

Across 32.4 million orders, the company noticed a strong imbalance:
- Some items (milk, bananas, eggs) are reordered at extremely high rates
- Others (new snacks, beverages, or niche items) are purchased once and never show up again

**2. Basket Size (Revenue per Transaction)**
Some customers consistently place large orders (20-30+ items), while others place small orders (2-5 items). In a delivery business with fixed picking and delivery costs, larger baskets drive profitability.

**These patterns raise two fundamental questions:**

1. **Is low reorder behavior caused by customer preferences, product issues, or operational factors?**
2. **Why do some customers build large baskets while others don't—and can this be influenced?**

Understanding the drivers behind both metrics enables Instacart to optimize retention strategies and revenue per transaction.

---


## **Analysis Approach**

This project diagnoses two critical customer behavior patterns:

## Question 1: **Why Do Some Products Get Reordered While Others Don't?**

<details>
<summary><strong>Executive Summary</strong></summary>

## **The Question**

59% of all items purchased on Instacart are reordered. But this average hides massive variation:
- Dairy products: 67% reorder rate
- Pantry items: 35% reorder rate

Why does this 32-percentage-point gap exist? Is it fixable through better operations, or is it structural customer behavior?

---

## **The Findings**

Reorder behavior is **not an operational problem**—it's driven by category psychology and customer maturity:

**1. Category Type Dominates Everything**
- Dairy and beverages are **routine replenishment** (customers buy the same milk weekly)
- Pantry items are **variety-seeking** (customers try different pasta brands)
- Statistical proof: Dairy has **97% higher reorder odds** than baseline categories (p < 0.001)

**2. Frequent Shoppers Form Habits**
- Customers with 20+ orders reorder **69% of their basket**
- Customers with 1-4 orders reorder only **24%**
- **45-percentage-point gap** shows habit formation takes time

**3. Recency Matters, But Not as Much as Expected**
- Customers ordering weekly (0-7 days) reorder at 67.5%
- Customers ordering monthly (22-30 days) reorder at 49.2%
- **18-point drop** shows engagement decay, but category still dominates

**4. Time of Day Doesn't Matter**
- Morning, afternoon, and evening orders show identical reorder rates (within 3%)
- Time-based promotions are wasted effort

---

## **The Root Cause**

Low reorder rates in pantry categories are **not a retention problem to solve**—they reflect natural variety-seeking behavior. Customers *want* to try different brands of pasta, sauces, and snacks.

High reorder rates in dairy/beverages reflect **routine replenishment needs**. Customers have a preferred milk brand and stick to it.

**Misalignment:** Instacart was treating all low-reorder categories as retention failures and throwing promotional budget at them. The data proves this is the wrong strategy.

---

## **Business Impact**

### **What Instacart Should Do Differently:**

**1. Stop Wasting Retention Budget on Pantry Items**
- Current state: Reorder promotions sent equally across all categories
- Evidence: Pantry has 53% *lower* reorder odds even after promotions
- Recommendation: Use pantry for **acquisition** (attract new customers with deals), not retention
- **Expected savings: 15-20% reduction in wasted promotional spend**

**2. Double Down on High-Frequency Customers**
- Customers with 20+ orders drive 69% reorder rates
- These users want **convenience, not discovery**
- Recommendation: Personalized "Buy Again" lists, subscription discounts, priority delivery
- **Expected impact: 10-15% increase in retention for established customers**

**3. Intervene at 14 Days, Not 30 Days**
- Reorder rates drop 8% between week 1 and week 2, then accelerate
- Current state: Lapsed-user campaigns trigger at 30+ days (too late)
- Recommendation: Send reorder reminders at 14-day mark for high-reorder categories only
- **Expected impact: 5-8% reduction in churn**

**4. Abandon Time-Based Targeting**
- Evidence shows no meaningful reorder difference by hour of day
- Recommendation: Reallocate engineering resources from time-based campaigns to category-based segmentation
- **Expected impact: Simpler operations, no revenue loss**

---

# **Analysis & Evidence**

## **Hypothesis Tested**

Reorder likelihood is driven by:
1. Product category (staples vs. impulse purchases)
2. Purchase recency (time since last order)
3. User purchase frequency (active vs. infrequent shoppers)
4. Product popularity (social proof effect)
5. Time of day (routine shopping vs. spontaneous orders)

**Method:** Statistical testing (chi-square) + predictive modeling (logistic regression) on 500,000 orders

---

## **Finding 1: Category Type Drives Everything**

### **Evidence Table**
| Department | Reorder Rate | Statistical Significance | Odds Ratio vs. Baseline |
|------------|--------------|-------------------------|------------------------|
| Dairy Eggs | 67.0% | p < 0.001 | **1.97x (97% higher)** |
| Beverages | 65.4% | p < 0.001 | 1.89x (89% higher) |
| Bakery | 62.8% | p < 0.001 | 1.77x (77% higher) |
| Produce | 65.1% | p < 0.001 | 1.29x (29% higher) |
| Frozen | 54.3% | p < 0.001 | 1.28x (28% higher) |
| Snacks | 57.5% | p < 0.001 | 1.34x (34% higher) |
| **Pantry** | **34.7%** | **p < 0.001** | **0.47x (53% LOWER)** |

### **What This Means**
- All differences are statistically significant (not random chance)
- Dairy customers are **nearly twice as likely** to reorder compared to pantry customers
- This is behavioral, not operational—you can't "fix" pantry reorder rates

---

## **Finding 2: Frequent Shoppers Form Habits**

### **Evidence Table**
| Customer Frequency | Reorder Rate | Sample Size | Behavioral Pattern |
|-------------------|--------------|-------------|-------------------|
| Very Low (1-4 orders) | 23.5% | 1.4M items | Exploring, no loyalty yet |
| Low (5-9 orders) | 37.1% | 4.0M items | Starting to find favorites |
| Medium (10-19 orders) | 50.8% | 7.0M items | Habits forming |
| High (20+ orders) | 68.9% | 19.9M items | Locked into routine |

### **What This Means**
- It takes **10+ orders** before customers start showing predictable reorder behavior
- By 20 orders, customers have identified their preferred products and reorder 7 out of 10 items
- New customer campaigns should focus on **breadth** (try many categories), not **retention** (reorder same items)

---

## **Finding 3: Recency Decays Engagement**

### **Evidence Table**
| Days Since Last Order | Reorder Rate | Behavioral Interpretation |
|----------------------|--------------|--------------------------|
| 0-7 days | 67.5% | Highly engaged, routine intact |
| 8-14 days | 64.4% | Slight decay, still predictable |
| 15-21 days | 59.5% | Engagement weakening |
| 22-30 days | 49.2% | At-risk, losing routine |

### **What This Means**
- Every week of delay reduces reorder likelihood by ~8%
- The critical intervention window is **14 days**, not 30 days
- After 3 weeks, customers have likely found alternatives or changed routines

---

## **Finding 4: Time of Day Is Irrelevant**

### **Evidence Table**
| Time of Day | Reorder Rate | Difference from Morning |
|-------------|--------------|------------------------|
| Morning (6-11 AM) | 61.1% | Baseline |
| Afternoon (12-5 PM) | 57.9% | -3.2% |
| Evening (6-9 PM) | 57.8% | -3.3% |
| Night (10 PM-5 AM) | 57.8% | -3.3% |

### **What This Means**
- Only 3-percentage-point variation across all hours
- After controlling for category and user frequency, time has no predictive power
- Don't waste engineering time on time-based promotions

---

## **Finding 5: Product Popularity Is Misleading**

### **Evidence from Two Tests**

**Univariate Analysis (Before Controlling for Other Factors):**
| Product Popularity | Reorder Rate |
|-------------------|--------------|
| Rare (< 100 orders) | 35.8% |
| Very Popular (5K+ orders) | 65.4% |

**Looks like popularity matters... but:**

**Logistic Regression (After Controlling for Category & Frequency):**
| Predictor | Coefficient | Interpretation |
|-----------|-------------|----------------|
| Product Popularity | +0.000005 | No effect |

**What This Means:**
- Popular products aren't reordered *because* they're popular
- They're popular *because* they're in high-reorder categories (dairy, beverages)
- Once you account for category type, popularity adds zero predictive value

---

## **Visualizations**

### **Feature Importance: What Predicts Reorder Behavior?**

<img width="1000" height="700" alt="feature_importance" src="https://github.com/user-attachments/assets/bea37a78-b132-4188-b0b7-8ab1d1651060" />

**Key Takeaway:** Department dummies dominate the model. Dairy, beverages, and bakery strongly increase reorder odds, while pantry dramatically decreases them. User frequency and recency are moderate predictors. Time-based features are irrelevant.

---

### **Model Performance: Can We Predict Reorder Behavior?**

<img width="1000" height="700" alt="prediction_distribution" src="https://github.com/user-attachments/assets/619ef999-3e5b-4294-99af-900a3f0f58d3" />

**Key Takeaway:** The model successfully separates reordered items (green, right-skewed toward high probability) from non-reordered items (red, left-skewed toward low probability). The overlap shows human behavior isn't perfectly predictable, but the model captures real patterns.

**Model Accuracy: 67% | ROC-AUC: 0.688**

---

## **Strategic Recommendations**

### **1. Segment Retention Strategy by Category**

| Category Type | Current Approach | Recommended Approach | Why |
|--------------|------------------|---------------------|-----|
| High-Reorder (Dairy, Beverages) | Generic retention emails | Personalized "Buy Again" lists, subscription offers | These customers want convenience |
| Low-Reorder (Pantry, Snacks) | Reorder promotions (wasted) | Discovery features, variety bundles | These customers want exploration |

**Impact:** Save 15-20% of retention budget by not fighting natural variety-seeking behavior.

---

### **2. Intervene Earlier for At-Risk Customers**

| Customer Segment | Current Trigger | Recommended Trigger | Tactic |
|-----------------|-----------------|---------------------|--------|
| Active (0-7 days) | None | None | Don't interrupt engaged users |
| Declining (14-21 days) | None | 14-day reminder | "Your favorites are waiting" for high-reorder categories only |
| Lapsed (30+ days) | Win-back campaign | Treat as new customer | Offer discovery deals, reset expectations |

**Impact:** Reduce churn by 5-8% through earlier intervention.

---

### **3. Prioritize High-Frequency Customers**

| Customer Tier | Orders Placed | Reorder Rate | Strategy |
|--------------|---------------|--------------|----------|
| New (1-9) | 1-9 | 24-37% | Encourage breadth, not loyalty |
| Established (10-19) | 10-19 | 51% | Introduce reorder features |
| Loyal (20+) | 20+ | 69% | Premium treatment: fast delivery, personalized lists |

**Impact:** Increase retention by 10-15% for customers reaching 20+ orders.

---

### **4. Eliminate Time-Based Targeting**

**Current State:** Resources spent on "morning deals," "evening specials," etc.

**Evidence:** Time of day shows no meaningful impact on reorder behavior.

**Recommendation:** Reallocate engineering and marketing resources to category-based and frequency-based segmentation.

**Impact:** Simplify operations without revenue loss.

---

## **Key Insight**

**Reorder behavior is primarily driven by what customers buy (category) and how often they shop (frequency), not when they shop or what's popular overall.**

Instacart should stop treating all low-reorder categories as problems to solve. Pantry variety-seeking is natural customer behavior. Focus retention efforts where they work: dairy, beverages, and bakery for established customers.

---

</details>


## **Question 2: Why Do Some Customers Build Larger Baskets Than Others?**
<details>
<summary><strong>Executive Summary</strong></summary>

<br>

**The Question**


## **The Question**

Average basket size on Instacart is 10.1 items per order. But this hides massive variation:
- Some orders contain just 2-3 items (quick top-offs)
- Others exceed 30+ items (weekly stock-ups)

Why does this 10x difference exist? Is it customer demographics, shopping habits, or timing? Understanding basket size drivers enables Instacart to optimize revenue per transaction.

---

## **The Findings**

Basket size is **not about who the customer is (frequency, loyalty)—it's about how they shop (category breadth and timing):**

**1. Category Diversity Dominates Everything**
- Shopping across multiple departments is the single strongest predictor of basket size
- Each additional category adds **2.43 items** to the basket
- Statistical proof: **Pearson correlation = 0.82** (very strong, p < 0.001)

**2. Sunday is Stock-Up Day**
- Sunday baskets are **19% larger** than Wednesday baskets (11.1 vs 9.3 items)
- Weekend baskets are **13% larger** than weekday baskets
- Statistical proof: **ANOVA F-statistic = 615.24** (p < 0.001)

**3. High Reorder Percentage = Smaller Baskets (Counterintuitive)**
- Customers with 80%+ reorder rates have **smaller baskets** (9.0 items vs 11.5 for exploratory shoppers)
- Why? They buy the same small set of staples weekly (milk, eggs, bread—done)
- Routine shoppers prioritize efficiency, not exploration

**4. User Frequency Doesn't Matter**
- Frequent shoppers (20+ orders) have nearly identical basket sizes to new customers (9.6 vs 10.2 items)
- Regression coefficient: **+0.003 items per order** (essentially zero)
- Frequent shoppers shop more often, but don't buy more per trip

---

## **The Root Cause**

Basket size is driven by **breadth (how many departments)** and **timing (weekly stock-up vs daily top-off)**, not customer loyalty or frequency.

**Key Insight:** A customer shopping for a Sunday meal prep across produce, dairy, meat, and pantry will have a 15-item basket. The same customer buying just milk and eggs on Wednesday will have a 3-item basket. The difference isn't who they are—it's what type of trip they're making.

**Misalignment:** Instacart was treating basket growth as a loyalty problem ("get customers to order more often"). The data proves it's a **cross-selling problem** ("get customers to shop more departments per trip").

---

## **Business Impact**

### **What Instacart Should Do Differently:**

**1. Drive Cross-Category Shopping to Grow Baskets**
- Current state: Product recommendations stay within the same category
- Evidence: Each additional category adds 2.43 items (R² = 0.675, p < 0.001)
- Recommendation: Build "complete your meal" or "pantry + produce" bundles spanning multiple departments
- **Expected impact: Each additional category = +2.4 items = $10-15 revenue per order**

**2. Shift Promotional Budget to Sundays**
- Current state: Promotions distributed evenly across the week
- Evidence: Sunday baskets are 19% larger (11.1 vs 9.3 items, p < 0.001)
- Recommendation: Concentrate multi-category deals on Sundays when customers are doing weekly stock-ups
- **Expected impact: 15-20% increase in promotional ROI by targeting high-basket days**

**3. Stop Confusing Loyalty with Basket Growth**
- Current state: High reorder customers treated as "high value" across all metrics
- Evidence: 80%+ reorder customers have 21% smaller baskets (9.0 vs 11.5 items)
- Recommendation: Segment KPIs—retention rate for routine shoppers, basket size for exploratory shoppers
- **Expected impact: Avoid wasting cross-sell efforts on efficiency-focused routine shoppers**

**4. Deprioritize Frequency-Based Basket Growth Tactics**
- Current state: "Order more often" campaigns aimed at basket growth
- Evidence: User frequency has near-zero impact on basket size (+0.003 coefficient)
- Recommendation: Focus on breadth (categories per trip), not frequency (trips per month)
- **Expected impact: Reallocate resources from ineffective frequency campaigns to category expansion**

---

# **Analysis & Evidence**

## **Hypothesis Tested**

Basket size is driven by:
1. Category diversity (shopping across multiple departments)
2. Day of week (weekend stock-ups vs weekday top-offs)
3. User frequency (established vs new customers)
4. Reorder percentage (planned vs exploratory shopping)
5. Time of day (routine vs spontaneous orders)

**Method:** Correlation analysis, ANOVA, t-tests, and linear regression on 500,000 orders

---

## **Finding 1: Category Diversity is THE Driver**

### **Statistical Evidence**

**Pearson Correlation Test:**
| Metric | Value | Interpretation |
|--------|-------|----------------|
| Correlation Coefficient | **0.8206** | Very strong positive correlation |
| P-value | < 0.001 | Highly significant |
| Sample Size | 500,000 orders | Statistically powered |

**Linear Regression:**
| Predictor | Coefficient | Interpretation |
|-----------|-------------|----------------|
| Category Diversity | **+2.43 items** | Each additional category adds 2.43 items to basket |
| P-value | < 0.001 | Highly significant |

### **Evidence Table: Category Diversity → Basket Size**
| Categories Shopped | Average Basket Size | Relationship |
|-------------------|---------------------|--------------|
| 1 category | 2.1 items | Baseline |
| 3 categories | 5.9 items | +3.8 items |
| 6 categories | 12.5 items | +10.4 items |
| 9 categories | 20.8 items | +18.7 items |
| 12 categories | 31.3 items | +29.2 items |

### **What This Means**
The relationship is nearly **perfectly linear**. Category diversity explains 67.5% of basket size variance (R² = 0.675). This is the single most powerful predictor in the model.

**Business Insight:** To grow basket size, get customers to shop across departments. A customer buying only produce will average 3-4 items. Add dairy and meat, and the basket jumps to 12+ items.

---

## **Finding 2: Sunday Stock-Up Effect is Real**

### **Statistical Evidence**

**ANOVA Test (Day of Week):**
| Metric | Value | Interpretation |
|--------|-------|----------------|
| F-statistic | **615.24** | Highly significant variance across days |
| P-value | < 0.001 | Not random chance |
| Sample Size | 3.2M orders | Robust finding |

**T-Test (Weekend vs Weekday):**
| Metric | Value | Interpretation |
|--------|-------|----------------|
| T-statistic | **54.51** | Large effect size |
| P-value | < 0.001 | Highly significant |
| Weekend Avg | 11.0 items | 13% larger |
| Weekday Avg | 9.7 items | Baseline |

### **Evidence Table: Day of Week → Basket Size**
| Day | Average Basket | Average Categories | Pattern |
|-----|----------------|-------------------|---------|
| **Sunday** | **11.1 items** | **5.1 categories** | Peak stock-up |
| Monday | 10.2 items | 4.7 categories | Post-weekend dip |
| Tuesday | 9.6 items | 4.5 categories | Midweek low |
| Wednesday | 9.3 items | 4.5 categories | Lowest point |
| Thursday | 9.5 items | 4.5 categories | Midweek low |
| Friday | 9.8 items | 4.6 categories | Pre-weekend rise |
| **Saturday** | **10.7 items** | **5.0 categories** | Weekend stock-up |

### **What This Means**
Sunday and Saturday show clear stock-up behavior—customers shop across more categories and buy more items. Wednesday is the "milk and eggs run"—fewer categories, smaller baskets.

**Business Insight:** Weekly shopping rhythms are real. Target multi-category promotions to weekends when customers are planning larger trips.

---

## **Finding 3: High Reorder % = Lower Category Diversity = Smaller Baskets**

### **Statistical Evidence**

**Linear Regression:**
| Predictor | Coefficient | Interpretation |
|-----------|-------------|----------------|
| Reorder Percentage | **+0.003 items** | Positive but minimal direct effect |
| Category Diversity | **+2.43 items** | Dominates the relationship |

**The key is the indirect relationship shown in the data:**

### **Evidence Table: Reorder % → Category Diversity → Basket Size**
| Shopping Style | Reorder % | Avg Basket Size | Avg Categories | Pattern |
|---------------|-----------|-----------------|----------------|---------|
| Exploratory | < 20% | 11.5 items | 5.5 categories | High breadth |
| Mixed | 20-40% | 11.1 items | 5.2 categories | Balanced |
| Routine | 40-60% | 11.2 items | 5.2 categories | Balanced |
| Highly Routine | 60-80% | 11.7 items | 5.3 categories | Slight increase |
| **Ultra Routine** | **80-100%** | **9.0 items** | **4.2 categories** | **Efficiency mode** |

### **What This Means**
Ultra-routine shoppers (80%+ reorder rate) have **21% smaller baskets** than exploratory shoppers. They're buying the same small set of staples every week with laser focus: dairy, produce, done.

**Business Insight:** Don't treat all loyal customers the same. Routine shoppers want speed and convenience, not cross-sell recommendations. Save basket-growth tactics for exploratory and mixed shoppers.

---

## **Finding 4: User Frequency Doesn't Predict Basket Size**

### **Statistical Evidence**

**Linear Regression:**
| Predictor | Coefficient | Interpretation |
|-----------|-------------|----------------|
| User Total Orders | **+0.003 items** | Essentially zero effect |
| P-value | > 0.05 | Not statistically significant |

### **Evidence Table: User Frequency → Basket Size**
| Customer Tier | Orders Placed | Avg Basket Size | Difference from Baseline |
|--------------|---------------|-----------------|------------------------|
| Very Low (1-4) | 1-4 | 9.6 items | Baseline |
| Low (5-9) | 5-9 | 9.9 items | +0.3 items (3%) |
| Medium (10-19) | 10-19 | 10.1 items | +0.5 items (5%) |
| High (20+) | 20+ | 10.2 items | +0.6 items (6%) |

### **What This Means**
Only a **6% difference** between new and highly frequent customers. After controlling for category diversity in the regression, the effect disappears entirely.

**Why?** Frequent shoppers don't buy more per trip—they just shop more often. A customer who orders 2x per week with 8-item baskets has the same basket size as a monthly shopper with 8-item baskets.

**Business Insight:** "Order more frequently" campaigns don't grow basket size. Focus on "shop more categories" instead.

---

## **Finding 5: Time of Day Doesn't Matter**

### **Evidence Table: Time of Day → Basket Size**
| Time Period | Avg Basket Size | Avg Categories | Difference |
|-------------|-----------------|----------------|------------|
| Night (10 PM-5 AM) | 10.6 items | 4.8 categories | +0.5 items |
| Morning (6-11 AM) | 10.2 items | 4.7 categories | Baseline |
| Afternoon (12-5 PM) | 10.1 items | 4.8 categories | -0.1 items |
| Evening (6-9 PM) | 9.9 items | 4.8 categories | -0.3 items |

### **What This Means**
Only **7% variance** across all hours. Not meaningful for business strategy.

**Business Insight:** Time-of-day targeting is irrelevant for basket growth. Day of week matters; hour of day doesn't.

---

## **Visualizations**

### **1. Category Diversity Drives Basket Size (Nearly Perfect Linear Relationship)**

<img width="1000" height="700" alt="category_diversity_impact" src="https://github.com/user-attachments/assets/fedfe61b-18d6-468a-9e1c-3e77fc637fd2" />


**Key Takeaway:** This is a near-perfect straight line (r = 0.82). Each additional category adds ~2.4 items. Shopping 12 categories yields 15x more items than shopping 1 category.

---

### **2. Sunday Stock-Up Effect: Clear Weekly Pattern**

<img width="1000" height="700" alt="day_of_week_pattern" src="https://github.com/user-attachments/assets/0b79179a-01e2-42dc-b694-17152ba3d92e" />


**Key Takeaway:** Sunday and Saturday (red bars) show clear peaks. Wednesday is the trough. This is consistent weekly behavior, not random variation.

---

### **3. Model Performance: R² = 0.675 (Strong Predictive Power)**

<img width="1000" height="700" alt="model_performance" src="https://github.com/user-attachments/assets/d2e65929-f530-4154-8d24-6813f549bf9c" />


**Key Takeaway:** The model explains 67.5% of basket size variance. Points cluster near the prediction line, showing the model captures real patterns. Mean Absolute Error of 3.01 items means predictions are typically within 3 items of actual.

---

### **4. High Reorder % = Lower Category Diversity = Smaller Baskets**

<img width="1000" height="700" alt="reorder_percentage_impact" src="https://github.com/user-attachments/assets/18b43e62-7d97-4254-b23a-3aa47e4906df" />


**Key Takeaway:** The red line (category diversity) crashes for ultra-routine shoppers (80-100% reorder). As categories drop, basket size follows. Routine shoppers are efficient, not exploratory.

---

## **Strategic Recommendations**

### **1. Drive Cross-Category Shopping**

| Current State | Recommended Approach | Why | Expected Impact |
|--------------|---------------------|-----|-----------------|
| Product recommendations stay within same category | Build "complete your meal" bundles spanning departments | Each category adds 2.43 items (r = 0.82) | +2-3 items per order = $10-15 revenue increase |
| Example: "More bananas" | Example: "Bananas + yogurt + granola" | Cross-category triggers breadth | 20-25% basket growth for targeted customers |

**Implementation:**
- "Customers who bought X also shopped these departments" recommendations
- Multi-category starter kits for new customers
- Track "category penetration rate" as a KPI alongside basket size

---

### **2. Optimize Promotional Calendar for Weekly Rhythms**

| Day Type | Current Approach | Recommended Approach | Why |
|----------|------------------|---------------------|-----|
| Sunday | Standard promotions | Multi-category "stock-up" bundles | 19% larger baskets, 5.1 avg categories |
| Wednesday | Standard promotions | Quick-add single items, convenience focus | Smallest baskets (9.3 items), low intent |
| Weekend Overall | Even budget distribution | Shift 60% of promotional budget to Sat/Sun | 13% larger baskets on weekends (p < 0.001) |

**Implementation:**
- "Stock-Up Sunday" campaigns featuring cross-category deals
- Midweek "essentials only" fast checkout promotions
- A/B test day-specific messaging

**Expected Impact:** 15-20% increase in promotional ROI by targeting high-basket days.

---

### **3. Segment Customers by Shopping Style, Not Just Frequency**

| Segment | Reorder % | Avg Basket | Strategy | KPI to Track |
|---------|-----------|------------|----------|--------------|
| Exploratory | < 40% | 11.1-11.5 items | Cross-sell, category expansion | Basket size, category diversity |
| Routine | 40-80% | 11.2 items | Balanced approach | Basket size, retention rate |
| Ultra-Routine | 80%+ | 9.0 items | Speed, convenience, loyalty perks | Retention rate, order frequency |

**Why This Matters:**
- Ultra-routine shoppers have **21% smaller baskets** by design—they want efficiency
- Don't waste cross-sell efforts on customers who deliberately avoid exploration
- Treat basket growth and retention as separate objectives

**Implementation:**
- Suppress multi-category recommendations for 80%+ reorder customers
- Offer "favorites fast checkout" for routine shoppers
- Reserve basket-growth tactics for exploratory/mixed segments

**Expected Impact:** 10-15% improvement in recommendation relevance, higher engagement.

---

### **4. Deprioritize Frequency-Based Basket Growth**

| Current Belief | Evidence | Corrected Strategy |
|---------------|----------|-------------------|
| "Get customers to order more often → bigger baskets" | User frequency coefficient = +0.003 (not significant) | "Get customers to shop more categories per trip" |
| Frequent shoppers have larger baskets | Only 6% larger (9.6 vs 10.2 items) | Frequency drives revenue via trips, not basket size |

**What to Change:**
- ❌ Stop: "Order twice a week for bigger savings" campaigns
- ✅ Start: "Shop produce + dairy + meat for complete meals" campaigns
- ❌ Stop: Measuring basket size by customer frequency tier
- ✅ Start: Measuring basket size by category penetration

**Expected Impact:** Reallocate wasted campaign budget to effective category-expansion tactics.

---

## **Model Summary**

**Linear Regression Results:**
- **R² = 0.675** (67.5% of variance explained)
- **Mean Absolute Error = 3.01 items**
- **Sample Size: 500,000 orders**

**Coefficients (Impact on Basket Size):**
| Feature | Coefficient | Interpretation |
|---------|-------------|----------------|
| **Category Diversity** | **+2.43 items** | Primary driver |
| Sunday | +0.09 items | Stock-up effect (after controlling for categories) |
| Reorder % | +0.003 items | Minimal direct effect |
| User Frequency | +0.003 items | Not significant |

---

## **Key Insight**

**Basket size is about breadth (categories) and timing (weekly rhythm), not loyalty or frequency.**

To grow revenue per transaction:
- Get customers to shop across more departments (each category = +$10-15)
- Target multi-category promotions to weekends when customers are planning larger trips
- Stop treating high-frequency routine shoppers as basket-growth targets—they want efficiency, not exploration

This analysis provides statistical evidence to shift strategy from "order more often" to "shop more broadly."


</details>

---
## **Dataset**
**Source:** Instacart Online Grocery Basket Analysis Dataset (**Kaggle**)      
[View Dataset](https://www.kaggle.com/datasets/yasserh/instacart-online-grocery-basket-analysis-dataset)

**Scale:**
- 32.4 million order items
- 3.3 million orders
- 206,000 customers
- 49,685 unique products across 21 departments

---


## **Tools Used**

- **PostgreSQL** — Data consolidation and feature engineering
- **Python** — Statistical testing (chi-square, logistic regression)
- **Visualization** — matplotlib, seaborn

---

## **Contact**

**Muhammad Aryan**  
Data Analyst | Diagnostic Analysis & Business Problem-Solving  
📧 aryanlytics@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/muhammadaryan008/)  
💻 [GitHub Portfolio](https://github.com/aryanlytics)

---

*"Data tells you what happened. Analysis tells you why. Strategy tells you what to do about it."*
