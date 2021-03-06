**Genpact Machine Learning Hackathon**

***Score Summary:***

Root of Mean Squared Logarithmic Error : 0.61

Private Leader Board Rank : Top 6% worldwide

***Libraries used (Python):***
pandas, numpy, catboost, scikit learn, matplotlib, seaborn


***Problem Statement:***
Your client is a meal delivery company which operates in multiple cities. They have various fulfillment centers in these cities for dispatching meal orders to their customers. The client wants you to help these centers with demand forecasting for upcoming weeks so that these centers will plan the stock of raw materials accordingly.

The replenishment of majority of raw materials is done on weekly basis and since the raw material is perishable, the procurement planning is of utmost importance. Secondly, staffing of the centers is also one area wherein accurate demand forecasts are really helpful. Given the following information, the task is to predict the demand for the next 10 weeks (Weeks: 146-155) for the center-meal combinations in the test set:

Historical data of demand for a product-center combination (Weeks: 1 to 145)
Product(Meal) features such as category, sub-category, current price and discount
Information for fulfillment center like center area, city information etc.


***Data Dictionary:***
Weekly Demand data (train.csv): Contains the historical demand data for all centers, test.csv contains all the following features except the target variable. <br/>
`Variable`               ---- `Definition`
1. id	                   ---- Unique ID
2. week                  ---- Week No                                                 
3. center_id	           ---- Unique ID for fulfillment center                        
4. meal_id	             ---- Unique ID for Meal                                      
5. checkout_price	       ---- Final price including discount, taxes & delivery charges
6. base_price	           ---- Base price of the meal                                  
7. emailer_for_promotion ---- Emailer sent for promotion of meal 
8. homepage_featured     ---- Meal featured at homepage                               
9. num_orders	(Target)   ---- Orders Count                                            

fulfilment_center_info.csv: Contains information for each fulfilment center <br/>
`Variable` ---- `Definition`                    
1. center_id   ---- Unique ID for fulfillment center
2. city_code   ---- Unique code for city            
3. region_code ---- Unique code for region          
4. center_type ---- Anonymized center type          
5. op_area	   ---- Area of operation (in km^2)     

meal_info.csv: Contains information for each meal being served <br/>
`Variable`  ---- `Definition`                           
1. meal_id  ---- Unique ID for the meal                 
2. category	---- Type of meal (beverages/snacks/soups….)
3. cuisine	---- Meal cuisine (Indian/Italian/…)        

***Evaluation Metric:***
The evaluation metric for this competition is 100*RMSLE where RMSLE is Root of Mean Squared Logarithmic Error across all entries in the test set.

Test data is further randomly divided into Public (30%) and Private (70%) data.

***Solution:***
Converted this time series problem to regression problem.


***Data transformation:***
Here number of orders placed (target variable) is highly right skewd so that Log transformation is applied.
Log transformation of base_price, checkout_price, and num_orders.

***Feature engineering:***
A major focus of my project was to do very effective feature engineering. I explored the relationships between different features.
1. Features on different meals, different centers, and their combination
2. Temporal Features


***Cross validation strategy:***
Last 10 weeks (136 - 145) of every center-meal pair data is used as a Validation dataset from train dataset.


***Model:***
One single CatBoost model which has RMSLE of 0.61.
High regularization so it does not overfit because of new features made using target variable.


***What didn't work:***
1. Just using original data as it is and using catboost regressor gave RMSLE of 1.58
2. Only using difference between base_price and checkout_price, difference between base_price and checkout_price as a features and not using any lag and exponentially
3. Rolling mean and median over last 26, 52, 104 weeks as features didn't work that well, feature importance was low.
4. Geographical features had low feature importance, So didn't use them in final model.
5. Other ensemble algorithms like XGboost, LightGBM gave lower scores


***TODO / Improvements:***
1. Extensive hyper parameter tuning and feature selection.
2. Create more features based on Categorical Encoding methods like mean encoding, freqvency encoding, hash encoding etc.
3. Try ARIMA , Prophet etc.
4. Try ensemble of different models.
