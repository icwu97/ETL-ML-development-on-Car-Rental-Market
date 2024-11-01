
# 1.	Business Goals for the Pricing Analysis
Optimize Car Retail Pricing Strategy

Goal: Identify the optimal price range for different car models and conditions and help sellers price their vehicles competitively to increase sells volumes/revenues while maximizing profitability.

Analysis: Use SQL and Python to analyze car transaction data


# 2.	Technical highlights

(1)	Price vs. Odometer Correlation:
Calculate the average price by odometer ranges to see how price drops with mileage.

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

(2)	Average Price by Condition:
Analyze how different conditions affect average pricing.

SELECT 
    condition, 
    AVG(price) AS avg_price, 
    COUNT(*) AS total_cars
FROM car_example
WHERE price IS NOT NULL
GROUP BY condition
ORDER BY avg_price DESC;

(3)	Price Trends by Model Year:
Examine the average price for each model year to identify how prices change over time.

SELECT 
    model_year, 
    AVG(price) AS avg_price, 
    COUNT(*) AS total_listings
FROM car_example
WHERE price IS NOT NULL AND model_year > 0
GROUP BY model_year
ORDER BY model_year DESC;

These three factors—odometer reading, condition, and model year—are the primary drivers influencing car prices. In a competitive pricing strategy, sellers can focus on setting prices relative to these key factors to attract buyers while maximizing profitability.
