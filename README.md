
# 1.	Business Goals for the Pricing Analysis
Optimize Car Retail Pricing Strategy

Goal: Identify the optimal price range for different car models and conditions and help sellers price their vehicles competitively to increase sells volumes/revenues while maximizing profitability.

Analysis: Use SQL and Python to analyze car transaction data


# 2.	Technical highlights

(1)	Odometer and Price:
Calculate the average price by odometer ranges to see how price drops with mileage.

```
SELECT 
     odometer
        WHEN odometer < 50000 THEN '0-50K'
        WHEN odometer BETWEEN 50000 AND 100000 THEN '50K-100K'
        WHEN odometer BETWEEN 100000 AND 150000 THEN '100K-150K'
        ELSE '150K+' 
    END AS mileage_range,
    AVG(price) AS avg_price
FROM car_example
WHERE price IS NOT NULL AND odometer IS NOT NULL
GROUP BY mileage_range
ORDER BY avg_price DESC;
```



(2)	Condition and Price:
Analyze how different conditions affect average pricing. The condition variable ranges from 0 to 4, likely representing different conditions from "poor" to "excellent".

```
SELECT 
    condition, 
    AVG(price) AS avg_price, 
    COUNT(*) AS total_cars
FROM car_example
WHERE price IS NOT NULL
GROUP BY condition
ORDER BY avg_price DESC;
```



(3)	Model Year and Price:
Examine the average price for each model year to identify how prices change over time.

```
SELECT 
    model_year, 
    AVG(price) AS avg_price, 
    COUNT(*) AS total_listings
FROM car_example
WHERE price IS NOT NULL AND model_year > 0
GROUP BY model_year
ORDER BY model_year DESC;
```




Basically, these three factors 'odometer reading, condition, and model year' are the primary drivers influencing car prices. In a competitive pricing strategy, sellers can focus on setting prices relative to these key factors to attract buyers while maximizing profitability.

# 3.	Summary

A brief summary and strategy of the outcomes based on the data:

1. Average Price by Odometer Range:

   - (1) Cars with 0-50K miles have the highest average prices.
   - (2) Low-mileage cars can be priced at a premium, as buyers are willing to pay more for less mileage.
   - ** Fun facts: The finding that cars with 150K+ miles have a higher average price than those with <50K miles is unusual. To investigate further, high-mileage vehicles in the data might be luxury models that retain value despite higher mileage. For example, some buyers are willing to pay more for a high-mileage premium or well-maintained vehicle like Mercedes compared to a lower-mileage and economy car.

   <img src="https://github.com/user-attachments/assets/fd2159df-7e0d-4d3e-b6a5-b918f45b8ba7" width="400" height="200">

3. Average Price by Condition:

   - (1) Cars in better conditions (like "like new" or "excellent") command higher prices.
   - (2) Vehicles in "salvage" or "fair" condition show significantly lower prices, indicate that condition is a key factor in determining price and value.
   - (3) Sellers can increase profitability by good maintaining vehicle condition.
   - (4) For cars in poor condition, sellers should adopt a discount strategy or focus on niche buyers to attract specific customers like students.
  
   <img src="https://github.com/user-attachments/assets/ffa43cca-da04-40ae-ba33-1ee0cd19941c" width="400" height="200">

5. Price Trends by Model Year:

   - (1) Newer model years generally have higher prices, it shows with a clear decline as model years get older.
   - (2) Sellers should highlight the features and reliability of newer models to justify higher prices. For older cars, sellers can emphasize good maintenance history to sustain competitive pricing.
  
   <img src="https://github.com/user-attachments/assets/752d59b8-439c-4e82-a70c-20692ba4471d" width="400" height="200">


# Regression statistics:

```
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
import pandas as pd

# Load the data
file_path = '/mnt/data/car_example.csv'
data = pd.read_csv(file_path)

# Drop rows with missing 'price', 'odometer', 'model_year' or 'condition' values
data_cleaned = data.dropna(subset=['price', 'odometer', 'model_year', 'condition'])
```

```
# Import necessary library
from sklearn.preprocessing import LabelEncoder

# Encode the 'condition' column
data_cleaned['condition'].fillna('unknown', inplace=True)
label_encoder = LabelEncoder()
data_cleaned['condition_encoded'] = label_encoder.fit_transform(data_cleaned['condition'])
```

```
# Function to perform and print results of linear regression for a single feature
def analyze_feature(feature_name, X, y):
    model = LinearRegression()
    model.fit(X, y)
    y_pred = model.predict(X)
    
    mse = mean_squared_error(y, y_pred)
    r2 = r2_score(y, y_pred)
    coefficient = model.coef_[0]
    
    print(f"--- {feature_name} ---")
    print("Mean Squared Error (MSE):", mse)
    print("R-squared (R²):", r2)
    print(f"Coefficient for {feature_name}: {coefficient}\n")
```

```
# Target variable
y = data_cleaned['price']

# Analyzing each feature separately
# Price vs Odometer
X_odometer = data_cleaned[['odometer']]
analyze_feature("Price vs Odometer", X_odometer, y)

# Price vs Condition
X_condition = data_cleaned[['condition_encoded']]
analyze_feature("Price vs Condition", X_condition, y)

# Price vs Model Year
X_model_year = data_cleaned[['model_year']]
analyze_feature("Price vs Model Year", X_model_year, y)```
```

1. Price vs Odometer:

Price vs Odometer
 - Mean Squared Error (MSE): 883302247.9873015
 - R-squared (R²): 0.0004469943499377793
 - Coefficient for Price vs Odometer: 0.009921914499807838
   
   - MSE (883,302,247.99): This high value suggests that the model's predictions deviate significantly from the actual prices on average. MSE is sensitive to the scale of the target variable.
   - R² (0.0004): This indicates that only 0.04% of the variation in price is explained by the odometer reading. Essentially, odometer has a negligible effect on the price in this dataset.
   - Coefficient (0.0099): For every 1-unit increase in odometer, the price increases by 0.0099 units. This is unusual because we typically expect odometer to negatively affect price (as cars with higher mileage usually have lower prices). This could be due to outliers, data quality issues, or the need for feature scaling.

2. Price vs Condition:

Price vs Condition
 - Mean Squared Error (MSE): 883293109.4070103
 - R-squared (R²): 0.0004573356520438665
 - Coefficient for Price vs Condition: 576.0638776428258

   - MSE (883,293,109.41): Similar to odometer, the predictions have a large deviation from the actual prices.
   - R² (0.00046): Only 0.046% of the price variation is explained by the car's condition. This suggests condition has a very weak relationship with price in this dataset.
   - Coefficient (576.06): For each unit increase in the condition_encoded feature, the price increases by $576. This suggests that better condition cars (if encoded as higher numbers) are slightly associated with higher prices, but the effect is very weak.

3. Price vs Model Year

Price vs Model Year
 - Mean Squared Error (MSE): 820532723.6300294
 - R-squared (R²): 0.071477569532441
 - Coefficient for Price vs Model Year: -12.722549154304126

   - MSE (820,532,723.63): This MSE is slightly lower than for the other features, meaning the model fits the data marginally better.
   - R² (0.0715): About 7.15% of the price variation is explained by the car's model year. While still weak, model year appears to be more predictive of price compared to odometer or condition.
   - Coefficient (-12.72): For each additional year in the car's model year, the price decreases by $12.72. This negative relationship could imply that older model years are associated with higher prices, which might seem counterintuitive but could make sense if the dataset includes vintage or collectible cars.

4. Relation between multivariate analysis:

```
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# Selecting features for multivariate analysis
X_multivariate = data_cleaned[['odometer', 'condition_encoded', 'model_year']]  # Add other features as needed
y = data_cleaned['price']

# Fit the model
model = LinearRegression()
model.fit(X_multivariate, y)

# Predictions
y_pred = model.predict(X_multivariate)

# Metrics
mse = mean_squared_error(y, y_pred)
r2 = r2_score(y, y_pred)
coefficients = model.coef_
intercept = model.intercept_

print("Multivariate Linear Regression Results:")
print(f"Mean Squared Error (MSE): {mse}")
print(f"R-squared (R²): {r2}")
print(f"Intercept: {intercept}")
print("Coefficients:")
for feature, coef in zip(X_multivariate.columns, coefficients):
    print(f"  {feature}: {coef}")
```
Multivariate Linear Regression Results:
Mean Squared Error (MSE): 808287861.6904154
R-squared (R²): 0.0853339687246768
Intercept: 49497.36478735225
Coefficients:
  odometer: -0.06252319819456688
  condition_encoded: 32.041516321995246
  model_year: -15.68573442815044


MSE (808,287,861.69): This MSE indicates the average squared deviation between the predicted and actual prices when considering all features. The MSE is slightly lower compared to univariate models, suggesting that combining multiple features improves the model's fit marginally.

R² (0.0853): About 8.53% of the variation in price is explained by the combined effects of odometer, condition, and model year. While this is an improvement over individual feature R² values, it still indicates that a significant portion of the price variability is not captured by these features, suggesting the need for additional or alternative predictors.

Intercept (49,497.36): When all features (odometer, condition_encoded, model_year) are 0, the baseline price of the car is approximately $49,497. While this value might not be meaningful practically, it helps anchor the regression line.

Coefficients:

 - Odometer (-0.0625): For each additional mile, the price decreases by $0.0625, holding all other features constant. This reflects the expected trend where higher mileage leads to lower car prices.
 - Condition Encoded (32.04): For each unit increase in the condition's encoded value, the price increases by $32.04, assuming other features are constant. While the impact is positive, the effect is relatively small, indicating condition alone has limited influence on price.
 - Model Year (-15.69): For each additional year in the car's model year, the price decreases by $15.69, holding other factors constant. This suggests that older cars (potentially vintage or collectible) might have higher prices, which could explain the negative relationship.
  

# 5. Conclusion
This regression model reveals that model year and condition are significant predictors of car price, with condition having a positive effect and model year a negative effect. Odometer, however, is not statistically significant in this model. After investigating, the resons on odometer might cause by luxury brand which may affect between odometer, brand, and price.
   - For optimal pricing, newer models (2015+) in excellent condition should be priced at the high end of the market range, while those in fair or poor condition should be discounted.
   - Mid-aged models (2005-2014) in good or excellent condition can be moderately priced.
   - For older or high-mileage cars in fair or poor condition, focus on budget pricing to attract buyers seeking affordable optionss. 
