#  Business Case Analysis: Promotion Effectiveness at a Fashion Retail Chain


##  B1. Problem Formulation

### (a) Machine Learning Problem Definition

The objective is to predict the **number of items sold** for each store under different promotional strategies.

**Target Variable:**
- `items_sold`

**Input Features:**
- Store attributes: `store_size`, `location_type`
- Promotion details: `promotion_type`
- Temporal features: `month`, `is_weekend`, `is_festival`
- Market factors: `competition_density`
- Customer behavior: footfall, past sales (if available)

**Type of ML Problem:**
- Supervised Regression

**Justification:**
The target variable (`items_sold`) is continuous, and the goal is to predict a numeric outcome. Hence, regression is the most appropriate approach.


### (b) Why Items Sold is Better than Revenue

Using **items sold** as the target variable is more reliable because:

- Revenue is influenced by pricing, discounts, and product mix
- Promotions (like discounts) may reduce revenue but increase sales volume
- It directly captures **customer demand and promotion effectiveness**

**Broader Principle:**
The target variable should align closely with the **business objective**. In this case, maximizing sales volume is more relevant than revenue.


### (c) Alternative Modelling Strategy

Instead of using a single global model:

**Recommended Approach:**
- Segmented models (e.g., by location type or store clusters)

**Justification:**
- Different store types (urban, semi-urban, rural) behave differently
- Customer preferences and responses to promotions vary
- Improves model accuracy by capturing **local patterns**


##  B2. Data and EDA Strategy

### (a) Data Joining and Dataset Design

**Data Sources:**
- Transactions
- Store attributes
- Promotion details
- Calendar data

**Joining Strategy:**
- Join tables using common keys like `store_id` and `transaction_date`

**Final Dataset Grain:**
- One row = **Store–Month level observation**

**Aggregations:**
- Total `items_sold` per store per month
- Average basket size
- Monthly footfall
- Promotion type applied
- Competition density (store-level constant)


### (b) Exploratory Data Analysis (EDA)

**1. Promotion Type vs Items Sold (Bar Chart)**
- Identifies which promotions perform best

**2. Time Series Plot (Monthly Sales Trends)**
- Detects seasonality and trends

**3. Distribution of Items Sold (Histogram)**
- Checks skewness and outliers

**4. Correlation Heatmap**
- Identifies relationships between variables
- Detects multicollinearity

**Impact on Modelling:**
- Strong correlations → feature selection
- Seasonal patterns → time-based features
- Outliers → transformation or cleaning


### (c) Handling Imbalance in Promotions

**Observation:**
- 80% of transactions occur without promotions

**Impact:**
- Model may become biased toward "no promotion"
- Poor learning of promotion effectiveness

**Solutions:**
- Oversample minority promotion types
- Perform stratified analysis
- Add promotion-specific features
- Consider separate models for promotion vs non-promotion cases


##  B3. Model Evaluation and Deployment

### (a) Train-Test Split and Evaluation Metrics

**Train-Test Strategy:**
- Use time-based split:
  - First ~80% → Training set
  - Last ~20% → Test set

**Why Not Random Split:**
- Breaks time order
- Causes data leakage
- Unrealistic for forecasting scenarios

**Evaluation Metrics:**

- **RMSE (Root Mean Squared Error):**
  - Penalizes large errors more heavily

- **MAE (Mean Absolute Error):**
  - Measures average prediction error

**Interpretation:**
- Lower RMSE and MAE indicate better performance
- Both metrics together give a balanced evaluation


### (b) Explaining Model Recommendations

To understand why different promotions are suggested:

**Approach:**
- Analyze feature importance from the model

**Key Influencing Factors:**
- Month (seasonality)
- Festival indicators
- Store type
- Competition density

**Example Insight:**
- December → Loyalty Points Bonus (due to festive demand)
- March → Flat Discount (to boost lower demand periods)

**Communication Strategy:**
- Use simple visuals (feature importance charts)
- Translate technical outputs into business insights


### (c) Deployment Strategy

**1. Model Saving:**
- Save trained pipeline using `joblib` or `pickle`

**2. Monthly Prediction Workflow:**
- Collect new monthly data
- Apply same preprocessing pipeline
- Generate predictions for each store

**3. Promotion Recommendation:**
- Evaluate all promotion types
- Select the one with highest predicted `items_sold`

**4. Monitoring:**
- Track predicted vs actual performance
- Monitor changes in:
  - Sales patterns
  - Customer behavior

**5. Retraining:**
- Retrain periodically (e.g., quarterly)
- Update model with latest data
