
# 1.	Business Goals for the Pricing Analysis
Optimize Car Retail Pricing Strategy

Goal: Identify the optimal price range for different car models and conditions and help sellers price their vehicles competitively to increase sells volumes/revenues while maximizing profitability.

Analysis: Use SQL and Python to analyze car transaction data


# 2.	Technical highlights

(1)	Odometer and Price:
Calculate the average price by odometer ranges to see how price drops with mileage.

![image](https://github.com/user-attachments/assets/69d168d5-700c-4233-950f-756a36ba3d0e)


(2)	Condition and Price:
Analyze how different conditions affect average pricing. The condition variable ranges from 0 to 4, likely representing different conditions from "poor" to "excellent".

![image](https://github.com/user-attachments/assets/e6243595-b881-4132-a3e5-edba6019dec7)


(3)	Model Year and Price:
Examine the average price for each model year to identify how prices change over time.

![image](https://github.com/user-attachments/assets/70ed8e80-b8cb-4476-8be1-5b6ec2f5ff78)


Basically, these three factors 'odometer reading, condition, and model year' are the primary drivers influencing car prices. In a competitive pricing strategy, sellers can focus on setting prices relative to these key factors to attract buyers while maximizing profitability.

# 3.	Summary

A brief summary and strategy of the outcomes based on the data:

1. Average Price by Odometer Range:
   ![image](https://github.com/user-attachments/assets/fd2159df-7e0d-4d3e-b6a5-b918f45b8ba7)

   - (1) Cars with 0-50K miles have the highest average prices.
   - (2) Low-mileage cars can be priced at a premium, as buyers are willing to pay more for less mileage.
   - ** Fun facts: The finding that cars with 150K+ miles have a higher average price than those with <50K miles is unusual. To investigate further, high-mileage vehicles in the data might be luxury models that retain value despite higher mileage. For example, some buyers are willing to pay more for a high-mileage premium or well-maintained vehicle like Mercedes compared to a lower-mileage and economy car.

2. Average Price by Condition:
   ![image](https://github.com/user-attachments/assets/ffa43cca-da04-40ae-ba33-1ee0cd19941c)

   - (1) Cars in better conditions (like "like new" or "excellent") command higher prices.
   - (2) Vehicles in "salvage" or "fair" condition show significantly lower prices, indicate that condition is a key factor in determining price and value.
   - (3) Sellers can increase profitability by good maintaining vehicle condition.
   - (4) For cars in poor condition, sellers should adopt a discount strategy or focus on niche buyers to attract specific customers like students .

3. Price Trends by Model Year:
   ![image](https://github.com/user-attachments/assets/752d59b8-439c-4e82-a70c-20692ba4471d)

   - (1) Newer model years generally have higher prices, it shows with a clear decline as model years get older.
   - (2) Sellers should highlight the features and reliability of newer models to justify higher prices. For older cars, sellers can emphasize good maintenance history to sustain competitive pricing.

# Regression statistics:
1. Average Price by Odometer Range:
   ![image](https://github.com/user-attachments/assets/3689a50b-d919-4fb5-af9e-86ac4a512906)
   
   - R Square (0.0017): This value shows that only 0.17% of the variation in car prices can be explained by odometer readings. This suggests that odometer alone is not a strong predictor of car prices.
   - Standard Error (24372.12): Indicated that the predictions are, on average off around $24,372.
   - P-value for Odometer (0.6541): The odometer coefficient is not that statistically significant (p > 0.05). This means that odometer does not significantly impact the price in this analysis.

3. Average price by Condition:
   ![image](https://github.com/user-attachments/assets/2cbdf0ea-b50a-4cd6-a56c-172e3952c95b)

   - R Square (0.0525): This value suggests that approximately 5.25% of the variation in car prices can be explained by the condition variable.
   - P-value for Condition (0.0118): The condition coefficient is statistically significant (p < 0.05). This is confirmed that car condition has a meaningful impact on price.

4. Average Trends by Model Year:
   ![image](https://github.com/user-attachments/assets/29d2fb39-243e-4a79-9b60-b1cb5e230e4b)

   - R Square (0.0693): This value indicates that approximately 6.93% of the variation in car prices can be explained by the model year. 
   - Standard Error (23,532.89): This suggests that the model's predictions deviate from the actual car prices by an average of $23,532.
   - P-value for Model Year (0.0037): The model year coefficient is statistically significant (p < 0.05). This is confirmed that model year has a meaningful impact on car price in this dataset.

5. Relation between multivariate analysis:
   ![image](https://github.com/user-attachments/assets/751a574b-cb47-4b48-8ec4-f960607ce99f)

   - R Square (0.1124): About 11.24% of the variation in car prices can be explained by the combined effects of odometer, model year, and condition.
   - Odometer: The p-value (0.5537) is above 0.05, so odometer is not statistically significant in this model, suggesting that it does not have a strong influence on car price when model year and condition are included.
   - Model Year: The p-value (0.0061) is below 0.05, indicating that model year has a statistically significant impact on car price. This aligns with expectations that older cars generally have lower prices.
   - Condition: The p-value (0.0424) is below 0.05, making condition statistically significant. This confirms that cars in better condition command higher prices.
  

# 5. Conclusion
This regression model reveals that model year and condition are significant predictors of car price, with condition having a positive effect and model year a negative effect. Odometer, however, is not statistically significant in this model. After investigating, the resons on odometer might cause by luxury brand which may affect between odometer, brand, and price.
