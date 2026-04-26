# (A) Problem Formulation 

This problem can be formulated as a supervised machine learning problem.
-> Target Variable: The promotion type that maximizes the number of item sold.
-> Input Features: Store location (urban/semi-urban/ryral), store size, monthly footfall, competition density, customer demographics, and previous promotion performance.
-> Type of ML Problem: Multi-class classification problem, since we are choosing one promotion out of multiple categories (Flat discount, BOGO, Free Gift, Category Offer, Loyalty Points).

The goal is to predict the best promotion for each store to maximize sales.

# (B) Target Variable Justification

-> Using items sold (sales volume) is more reliable than total revenue because revenue can be influenced by price variations, discounts, pr premium products. A promotion might generate high revenue due to expensive items, even if fewer units are sold.
-> In contrast, items sold directly reflects customer response and demand, making it a better measure of promotion effectiveness.
-> This illustrates an important principle in machine learning: the target variable should align closely with the actual business objective. Choosing the right target ensures the model optimizes for meaningful outcomes.

# (C) Alternative Modeling Strategy
 
-> Instead of using a single global model, a better approach is to use segment-based or cluster-based models.
-> stores can be grouped based on characteristics such as location, size, or customer demographics, and separate models can be trained for each group. This allows the model to capture differences in customer behaviour across regions.
-> This approach improves prediction accuracy because it accounts for the variablity in how different stores respond to promotions.

B2. Data and EDA Strategy 

# (A) Data Joining and Aggregation Strategy

The data from the four tables (transactions, store attributes, promotion details, and calender) will be joined using common keys such as store_id, data, and promotion_id.
- Transaction table will be the base table.
- Store attributes will be joined using store_id.
- Promotion details will be joined using promotion_id
- Calender data will be joined using data.

* Grain of the dataset: Each row will represent one store on one data with specific promotion applied.
* Aggregation: - Total items sold per store per day
- Total revenue per store per day
- Average footfall per store
- Promotion-wise performance metrics

These aggregations help reduce data size and make it suitable for modeling.

# (B) EDA Strategy 

1:- Sales Distribution Analysis (Histogram/Boxplot): To understand the distribution of items sold and detect outliers.
-> Helps in deciding whether transformation is needed.

2:- Promotion-wise Performance(Bar Chart): Compare average items sold across different promotions.
-> Helps identify which promotions are generally more effective.

3:- Store-wise Analysis (Groupby/Bar chart): Compare performance across store locations(urban,rural,etc.).
-> Helps in creating location-based features.

Time-based Trends (Line Chart): Analyze sales over time (daily/monthly trends).
-> Helps capture seasonality and trends.

5: Correlation Heatmap: check realationships between numerical feature like footfall, sales, etc.
-> Helps in feature selection and removing redundant features.

These analyses guide feature enginerring and improve model performance.

# (C) Handling Data Imbalance

The imbalance in the dataset (80% no promotion) can cause the model to become baised toward predicting the majority class, reducing its ability to learn the impact of promotions.

To address this: - Use resampling techniques such as oversampling (SMOTE) or undersampling
- Apply class weights to give more importance to minority classes
- Use evaluation metrics like F1-score instead of accuracy

These steps help the model learn balanced patterns and improve prediction quality.

# B3 Model Evaluation and Deployment

# (A) Train-test Split and Evaluation Metrics

since the data is time-series (monthly data over three years), a random split is inappropriate because it would mix past and future data, leading to data leakage.
Instead, a time-based split should be used: - Train on the first 2 years od data
- Test on the most recent 1 year

Evaluation Metrics:- - F1-score: Balances precision and recall, useful when classes are imbalanced
- Precision: Measures how accurate the promotion recomendations are
- Recall: Measures how well the model identifies effective promotions

Interpretation: A high F1-score idicates a good balance between identify the right promotions and avoiding incorrect recommendations, which is critical for business  decisions.

# (B) Explaining Model Recommendations

Different recommendations for the same store in different monts can be explained using feature importance.

for example: - In december, factors like festive season, higher footfall, and customer buying behavior may make loyalty points bonus more effective.
- In March, lower demand or different customer patterns may make flat discount more suitable.

By analyzing feature importance, we can identify which features(such as month, footfall, or promotion history) influenced the prediction.

This explaination helps communicate to the marketing team that recommendations are data-driven and vary based on seasonal and store-specific factors.

# (C) Deployment and Monitoring Strategy

Model Deployment: - Save the Trained model using tools like joblib or pickle
- Load the model at the start of each month to generate predictions 

Data Pipeline: - Collect new monthly data (store performance, promotions, calender data)
- Preprocess and transform it in the same way as training data
- Feed it into the model to generate recommendations

Monitoring: - Track model perfromance over time using metrics like F1-score
- Compare predicted vs actual sales perfromance
- Detect performance degredation (data drift or concept drift)

If performance drops significantly, retraining the model with updated data is required.

This ensures the model remains accurate and useful for business decisions.