🍔 Food Delivery Time Prediction
📌 About the Project
This project is about predicting how long a food order will take to be
delivered.
The idea is simple: delivery time is affected by many things, such as
traffic, road distance, preparation time, weather, vehicle type, and
delivery distance. Instead of only looking at past orders, I used
machine learning to learn the relationship between these factors and the
actual delivery time.
The project includes both the data analysis part and a small
Streamlit web application where a user can enter delivery details
and get an estimated delivery time.
---
🎯 Project Goal
The main goal is to build a regression model that can predict:
> *`Time\_taken\_min` --- the total delivery time in minutes*
The model can be useful for estimating delivery time for a new order
based on the information available before or during delivery.
---
📂 Project Files
The project contains:
``` text
Food-Delivery-Time-Prediction/
│
├── Final_app.ipynb
├── Food\\_Delivery_Time_Prediction.csv
├── app.py
└── README.md
```
Files description
`Final\\\\\\\\\\\\\\\_app.ipynb`  
The main Jupyter/Google Colab notebook. It contains the data
loading, data checking, exploration, preprocessing, model training,
evaluation, and Streamlit app code.
`Food\\\\\\\\\\\\\\\_Delivery\\\\\\\\\\\\\\\_Time\\\\\\\\\\\\\\\_Prediction.csv`  
The dataset used for the project.
`app.py`  
The Streamlit application generated from the notebook. It provides
the interactive prediction interface.
`README.md`  
This file explains the project and how to run it.
---
📊 Dataset
The dataset contains 50,000 food delivery orders and 24 columns.
The target variable is:
``` text
Time\\\\\\\\\\\\\\\_taken\\\\\\\\\\\\\\\_min
```
which represents the delivery time in minutes.
The original dataset has no missing values and no duplicated rows.
Dataset Columns
---
Column                              Description
---
`Order_ID`                          Unique ID for each order
`Order\_Date`                        Date when the order was placed
`Order\_Hour`                        Hour when the order was placed
`Day_of_Week`                       Day of the week
`Is_Weekend`                        Indicates whether the order was
placed on a weekend
`Is_Festival`                       Indicates whether the order was
placed during a festival
`Weather`                           Weather condition during delivery
`Pickup_Zone`                       Zone where the restaurant/order was
picked up
`Dropoff_Zone`                      Destination zone
`Vehicle_Type`                      Vehicle used for the delivery
`Rider_Experience_Years`            Rider's experience in years
`Rider_Rating`                      Rider rating
`Restaurant_Rating`                 Restaurant rating
`Cuisine_Type`                      Type of food/cuisine
`Order_Items`                       Number of items in the order
`Restaurant_Load`                   Restaurant workload level
`Preparation_Time\_Min`              Time needed to prepare the order
`Roa_Distance_km`                  Road distance in kilometers
`Delivery_Distance_Category`        Short, Medium, or Long delivery
`Traffic_Level`                     Low, Moderate, High, or Severe
`Number_of_Signals`                 Number of traffic signals on the
route
`Average_Speed_kmph`                Average delivery speed
`Delivery\_Priority`                 Normal, Priority, or VIP
`Time\_taken_min`                    Target: actual delivery time in
minutes
---
🔎 Data Preparation
I started by loading the CSV file using Pandas and checking the dataset
before building the model.
The following checks were performed:
Data types
Missing values
Duplicate rows
Descriptive statistics
Numerical feature distributions
Correlations
Delivery time patterns
The dataset originally contains 50,000 rows. In the application
pipeline, I kept only logically valid delivery records where:
``` text
Time\\\\\\\\\\\\\\\_taken\\\\\\\\\\\\\\\_min > Preparation\\\\\\\\\\\\\\\_Time\\\\\\\\\\\\\\\_Min
```
This left 49,996 records for the application/model pipeline.
The date column is also converted to a datetime format, and year, month,
and day are extracted in the application code.
---
📈 Exploratory Data Analysis
I explored the data to understand which factors are related to delivery
time.
Some of the analysis included:
Traffic Level
One of the clearest patterns is the difference in average delivery time
between traffic levels.
Approximate average delivery times in the cleaned application data are:
Traffic Level     Average Delivery Time
---
Low                           76.45 min
Moderate                      92.30 min
High                         111.73 min
Severe                       134.40 min
This shows a clear increase in delivery time as traffic becomes heavier.
Other analysis
The application also includes:
Numerical feature distributions
Correlation heatmap
Delivery time by traffic level
Delivery time by order hour
Dataset preview
These visualizations help understand the data before using the machine
learning model.
---
🤖 Machine Learning Approach
This is a regression problem because the target is a continuous
numerical value: delivery time in minutes.
Features Used
For the final prediction model, I used these 8 features:
``` text
Preparation_Time_Min
Road_Distance_km
Average_Speed_kmph
Traffic_Level
Delivery_Distance_Category
Vehicle_Type
Weather
Pickup_Zone
```
Target
``` text
Time_taken_min
```
The dataset is divided into:
80% training data
20% testing data
using `train\_test\_split` with:
``` python
test_size=0.2
random_state=42
```
---
## 📊 Power BI Dashboard

An interactive Power BI dashboard was built to explore delivery performance and identify the key factors affecting delivery time. The dashboard consists of two pages, navigable through a built-in **Navigator** panel, with dynamic filters for **Traffic Level**, **Vehicle Type**, and **Pickup Zone**.

### Page 1: Overview
High-level KPIs and performance summary:
- **Total Orders:** 50K
- **Avg. Delivery Time:** 83.87 min
- **Avg. Preparation Time:** 24.03 min
- **Avg. Rider Rating:** 3.95

**Visuals:**
- *When Does Delivery Slow Down?* — average delivery time by day of week
- *Which Ride Gets There Faster?* — average delivery time by vehicle type
- *Traffic Is Costing Us Time* — delivery time breakdown by traffic severity
- *The Rush Hour Effect* — delivery time trend across hours of the day
- *Where Are Deliveries Taking Longer?* — average delivery time by zone type

### Page 2: Analysis
Deeper breakdown of delivery bottlenecks and contributing factors:
- **Late Orders:** 35.04%
- **Avg. Distance:** 26.20 km
- **Bad Weather Time:** 102.16 min
- **Max Delivery Time:** 180 min

**Visuals:**
- *How Weather Conditions Delay Deliveries* — delivery time share by weather condition
- *Impact of Traffic Signals on Delivery Time* — delivery time vs. number of traffic signals
- *Avg Preparation Time by Cuisine Type* — preparation time ranked by cuisine
- *Delivery Time vs Distance Groups* — delivery time by distance range
- *Priority & Traffic Level Impact on Time* — delivery time by order priority and traffic level

### Purpose
The dashboard translates the raw dataset into actionable insights — helping identify which factors (weather, traffic, distance, vehicle type, preparation time) most affect delivery time, and supporting the business questions defined earlier in the project.
------------
🔄 Feature Encoding
The model cannot directly work with text categories, so categorical
features were converted into numerical representations.
Ordinal Encoding
These two features have a natural order:
``` text
Traffic\\\\\\\\\\\\\\\_Level
Low → Moderate → High → Severe

Delivery\\\\\\\\\\\\\\\_Distance\\\\\\\\\\\\\\\_Category
Short → Medium → Long
```
They were encoded using `OrdinalEncoder`.
One-Hot Encoding
These features do not have a natural order:
``` text
Vehicle\\\\\\\\\\\\\\\_Type
Weather
Pickup\\\\\\\\\\\\\\\_Zone
```
So they were converted using `OneHotEncoder`.
The preprocessing is handled using `ColumnTransformer`.
---
🌲 Model
The main model used in the final application is:
Random Forest Regressor
Random Forest was selected because it can capture non-linear
relationships between different delivery factors.
The final application uses:
``` python
RandomForestRegressor(
    n\\\\\\\\\\\\\\\_estimators=300,
    max\\\\\\\\\\\\\\\_depth=None,
    min\\\\\\\\\\\\\\\_samples\\\\\\\\\\\\\\\_leaf=5,
    random\\\\\\\\\\\\\\\_state=42
)
```
The model is trained on the encoded training data and then evaluated on
the test data.
---
📏 Model Evaluation
The model was evaluated using common regression metrics:
MAE --- Mean Absolute Error
This tells us the average absolute difference between the actual
delivery time and the predicted delivery time.
MSE --- Mean Squared Error
This gives more weight to larger prediction errors because the errors
are squared.
RMSE --- Root Mean Squared Error
This is the square root of MSE and is expressed in the same unit as the
target, which is minutes.
R² --- R-squared
This shows how much of the variation in delivery time is explained by
the model.
For the evaluated notebook model, the results were:
Metric        Result
---
MAE         5.69 min
MSE            53.92
RMSE        7.34 min
Test R²        0.958
The notebook also showed:
``` text
Train R²: 0.9588
Test R²: 0.9579
```
The close training and testing R² values suggest that the model
performed similarly on both sets.
---
🖥️ Streamlit Application
The project also includes an interactive Streamlit application.
The app has three main sections:
🏠 Overview
This page gives a quick summary of the project and shows some KPIs,
including:
Total orders
Average delivery time
Average distance
Average speed
It also shows delivery time patterns by traffic level and order hour.
📊 Data Insights
This page allows the user to explore:
A preview of the dataset
Numerical feature distributions
Correlation heatmap
Delivery time by traffic level
🤖 Prediction
This is the main prediction page.
The user enters:
Preparation time
Road distance
Average speed
Traffic level
Delivery distance category
Vehicle type
Weather
Pickup zone
After clicking Predict Delivery Time, the model returns an estimated
delivery time in minutes.
The app also displays the model's R² score.
---
🚀 How to Run the Project
1. Clone the repository
``` bash
git clone <your-repository-link>
cd Food-Delivery-Time-Prediction
```
2. Install the required libraries
``` bash
pip install pandas numpy matplotlib seaborn scikit-learn streamlit
```
3. Make sure the files are in the same folder
``` text
Final_app.ipynb
Food_Delivery_Time_Prediction.csv
app.py
README.md
```
The CSV file needs to be in the same directory as `app.py` because the
application loads it using:
``` python
pd.read_csv("Food_Delivery_Time_Prediction.csv")
```
4. Run the Streamlit app
``` bash
streamlit run app.py
```
Then open the local URL shown by Streamlit in your browser.
---
🧪 Example Prediction
For example, a user could enter:
``` text
Preparation Time: 15 minutes
Road Distance: 5 km
Average Speed: 30 km/h
Traffic Level: Moderate
Distance Category: Medium
Vehicle Type: Bike
Weather: Clear
Pickup Zone: CBD
```
The model will use these values together and return an estimated
delivery time.
The prediction is not based on only one factor. The Random Forest model
considers the combination of all selected features.
---
🧠 What I Learned From the Project
This project helped me practice the complete machine learning workflow:
Loading a real dataset
Understanding the columns
Checking data quality
Exploring patterns using visualizations
Selecting useful features
Splitting the data into training and testing sets
Encoding categorical variables
Training a regression model
Evaluating the model using different metrics
Connecting the model to a simple web application
It also helped me understand that building an ML project is not only
about training a model. The data preparation, feature selection,
evaluation, and final user interface are all important parts of the
project.
---
⚠️ Limitations
There are some limitations to keep in mind:
The prediction depends on the quality and range of the dataset.
The dataset represents historical delivery records, so real-world
conditions can be different.
Some information that could affect delivery time may not be
available in the dataset.
The model should not be considered a guarantee of the exact delivery
time.
The Streamlit application uses the trained model generated from the
project pipeline.
---
🔮 Future Improvements
Some possible improvements for the project are:
Try and compare more regression algorithms.
Perform hyperparameter tuning more systematically.
Add more useful features if additional delivery information is
available.
Improve the user interface of the Streamlit app.
Add prediction confidence or an expected prediction range.
Save the trained model so it does not need to be trained every time
the application starts.
Deploy the application online so it can be accessed without running
it locally.
---
🛠️ Technologies Used
Python
Pandas --- data loading and manipulation
NumPy --- numerical operations
Matplotlib --- visualization
Seaborn --- correlation and data visualization
Scikit-learn --- preprocessing, Random Forest, and evaluation
Jupyter Notebook / Google Colab --- development and analysis
Streamlit --- interactive web application
---
👩‍💻 Project Structure
The general workflow of the project is:
``` text
Dataset
   ↓
Data Cleaning & Checking
   ↓
Exploratory Data Analysis
   ↓
Feature Selection
   ↓
Train / Test Split
   ↓
Categorical Encoding
   ↓
Random Forest Regression
   ↓
Model Evaluation
   ↓
Streamlit Prediction App
```
---
Finally
This project was built as a practical machine learning project to
understand how delivery-related information can be used to estimate food
delivery time.
The main idea is to take information about an order, process it
correctly, and use a trained regression model to give a useful estimate
instead of relying only on a simple guess.
