While completing the **Google Advanced Data Professional Certificate**, I completed the Automatidata project:

# Project Report: Predicting Taxi Fares for NYC Taxi and Limousine Commission (TLC)

> ## Project Background:
> The data consulting firm Automatidata has recently hired you as the newest member of their data analytics team. Their newest client, the NYC Taxi and Limousine Commission (New York City TLC), wants the Automatidata team to build a multiple linear regression model to predict taxi fares using existing data that was collected over the course of a year. The team is getting closer to completing the project, having completed an initial plan of action, initial Python coding work, EDA, and A/B testing.
The Automatidata team has reviewed the results of the A/B testing. Now it’s time to work on predicting the taxi fare amounts. You’ve impressed your Automatidata colleagues with your hard work and attention to detail. The data team believes that you are ready to build the regression model and update the client New York City TLC about your progress.

> ### Client: NYC Taxi and Limousine Commission (TLC)
> ### Consulting Firm: Automatidata
> ### Team Member: S N M Azizul Hoque

### 1. Introduction
Automatidata, a leading data consulting firm, tasked our team with building a multiple linear regression model for the NYC Taxi and Limousine Commission (TLC). The objective was to use a year’s worth of collected data to predict taxi fares. As a newly onboarded member of the team, I was involved in various stages of the project, including planning, data exploration, statistical analysis, and model building. This report outlines the key steps and findings from the project.

### 2. Project Objective
The primary goal was to develop a regression model to accurately predict taxi fares in New York City based on various factors such as trip distance, time of day, and passenger count. The challenge was to turn the collected raw data into meaningful insights, leveraging machine learning techniques to create a model that could provide accurate fare predictions.

### 3. Step 1: Translating Data into Insights[^1]
The initial step involved transforming the raw dataset into actionable insights. The dataset included numerous features such as pickup/dropoff locations, distance traveled, time duration, passenger count, and fare amounts.

#### - 3.1 Data Cleaning and Preprocessing
- Before conducting any analysis, it was crucial to clean and prepare the data. Key tasks included:
- Handling missing values: Missing data for key variables such as fare amounts and trip distances was either imputed or removed based on the context.
- Outlier detection: Outliers (such as unusually long trips with low fares) were identified and addressed to prevent skewed model results.
- Date and time manipulation: Trip timestamps were converted into features representing time of day, day of the week, and month, which are crucial for fare prediction.

#### - 3.2 Exploratory Data Analysis (EDA)[^2]
After cleaning, I conducted an exploratory data analysis (EDA) to better understand the data distribution and relationships between variables. Key insights from the EDA included:
- Fare patterns: There was a clear relationship between trip distance and fare, with longer trips generally leading to higher fares.
- Time-related trends: Rush hour trips (morning and evening) tended to be more expensive, likely due to traffic conditions.
- Passenger count: The number of passengers appeared to have a minor influence on fare amounts, although larger groups sometimes resulted in higher fares.
          
#### - 3.3 Correlation Analysis
I performed a correlation analysis to determine which variables had the strongest linear relationships with fare amounts. Unsurprisingly, trip distance had the highest positive correlation with fare. Other factors, such as trip duration and the time of day, also showed moderate correlations, indicating their potential usefulness in the predictive model.

```Python
# 1. Generate random points on a 2D plane from a normal distribution
test = np.round(np.random.normal(10, 5, (3000, 2)), 1)
midway = int(len(test)/2)  # Calculate midpoint of the array of coordinates
start = test[:midway]      # Isolate first half of array ("pick-up locations")
end = test[midway:]        # Isolate second half of array ("drop-off locations")

# 2. Calculate Euclidean distances between points in first half and second half of array
distances = (start - end)**2           
distances = distances.sum(axis=-1)
distances = np.sqrt(distances)

# 3. Group the coordinates by "drop-off location", compute mean distance
test_df = pd.DataFrame({'start': [tuple(x) for x in start.tolist()],
                   'end': [tuple(x) for x in end.tolist()],
                   'distance': distances})
data = test_df[['end', 'distance']].groupby('end').mean()
data = data.sort_values(by='distance')

# 4. Plot the mean distance between each endpoint ("drop-off location") and all points it connected to
plt.figure(figsize=(14,6))
ax = sns.barplot(x=data.index,
                 y=data['distance'],
                 order=data.index)
ax.set_xticklabels([])
ax.set_xticks([])
ax.set_xlabel('Endpoint')
ax.set_ylabel('Mean distance to all other points')
ax.set_title('Mean distance between points taken randomly from normal distribution');
````
![Fig 1:](https://github.com/snmhoque123/python.github.io/blob/main/py1.png)


```Python
plt.figure(figsize=(16,4))
# DOLocationID column is numeric, so sort in ascending order
sorted_dropoffs = df['DOLocationID'].sort_values()
# Convert to string
sorted_dropoffs = sorted_dropoffs.astype('str')
# Plot
sns.histplot(sorted_dropoffs, bins=range(0, df['DOLocationID'].max()+1, 1))
plt.xticks([])
plt.xlabel('Drop-off locations')
plt.title('Histogram of rides by drop-off location', fontsize=16);
```
![Fig2: ](https://github.com/snmhoque123/python.github.io/blob/main/py2.png)

### 4. A/B Testing Results
To verify the effectiveness of different model features and strategies, the team conducted A/B testing. Different versions of the dataset and model were tested to determine which version led to the most accurate fare predictions. These tests confirmed that including time-related features (e.g., rush hours and weekdays) significantly improved the model’s performance.

### 5. Regression Model Development [^3]
After completing the A/B testing, the next step was to build a multiple linear regression model. Using Python libraries such as Pandas, Numpy, and Scikit-Learn, I developed the model to predict taxi fares based on a variety of factors:
**Independent variables:** Trip distance, trip duration, pickup/dropoff location, time of day, day of the week, and passenger count.
**Dependent variable:** Fare amount.
The model was trained using a subset of the dataset and evaluated using performance metrics such as Mean Squared Error (MSE) and R-squared. The model performed well, with trip distance and time of day emerging as the most important predictors of fare amounts.

```Python
# Create correlation heatmap

plt.figure(figsize=(6,4))
sns.heatmap(df2.corr(method='pearson'), annot=True, cmap='Reds')
plt.title('Correlation heatmap',
          fontsize=18)
plt.show()
```
![Fig3: Correlation Heatmap](https://github.com/snmhoque123/python.github.io/blob/main/py3.png)

```Python
fig, ax = plt.subplots(figsize=(6, 6))
sns.set(style='whitegrid')
sns.scatterplot(x='actual',
                y='predicted',
                data=results,
                s=20,
                alpha=0.5,
                ax=ax
)
plt.plot([0,60], [0,60], c='red', linewidth=2)
plt.title('Actual vs. predicted');
```
![Fig4: Visualization Model Result](https://github.com/snmhoque123/python.github.io/blob/main/py4.png)

### 6. Results & Insights
The final regression model demonstrated strong predictive power, with the following insights:

Key fare determinants: The model confirmed that trip distance and time of day are the most significant factors influencing fare amounts.
Rush hour premiums: Fares were consistently higher during peak times, highlighting the importance of traffic and demand fluctuations in pricing.
Passenger count: While this variable had some influence, it played a less significant role in fare prediction compared to other factors.

### 7. Next Steps
The predictive model was shared with the NYC Taxi and Limousine Commission (TLC) for feedback and further refinement. The model will be integrated into their systems to provide real-time fare predictions, helping both drivers and passengers estimate fares more accurately.

### 8. Conclusion
This project showcased the power of data analysis and machine learning in deriving actionable insights from raw data. By leveraging regression techniques and thorough exploratory data analysis, I successfully contributed to the development of a model that can predict taxi fares with high accuracy. The experience enhanced my skills in data cleaning, statistical analysis, and machine learning model development.

## Key Tools & Skills Used:

- [ ] Python (Pandas, Numpy, Scipy, Scikit-Learn)
- [ ] Multiple Linear Regression
- [ ] Data Cleaning and Preprocessing
- [ ] Exploratory Data Analysis (EDA)
- [ ] A/B Testing

## References: 
[^1]: ###  [Step 1: Translate Data into Insights](https://github.com/snmhoque123/google_eda_python/blob/main/Activity_Course%203%20Automatidata%20project%20lab.ipynb)
[^2]: ###  [Step 2: Statistical Analysis](https://github.com/snmhoque123/google_stat_python/blob/main/Activity_%20Course%204%20Automatidata%20project%20lab.ipynb)
[^3]: ###  [Step 3: Regression Analysis: Simplify complex data relationships](https://github.com/snmhoque123/google_regression_repository/blob/main/Activity_%20Course%205%20Automatidata%20project%20lab.ipynb)


Besides, I completed IBM Data Analysis with Python. Please check the IBM Python Project:
### [Python Project Link:](https://github.com/snmhoque123/python_project/blob/main/Home%20Sales%20in%20King%20Count%20Usa.ipynb)

### Applied the following 🔑 Key Skills:
- [ ] Using Pandas, Numpy and Scipy libraries for data manipulation
- [ ] Using Scikit-Learn to build smart models and make predictions
- [ ] Building machine learning regression models
- [ ] Building data pipelines

#Python #DataScience #MachineLearning #Coursera #IBMBadge #DataAnalysis #DataPipelines
