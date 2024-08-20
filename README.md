# San-Francisco-AirBnb-rental-price-prediction-Databricks

## Overview

This project analyzes a San Francisco Airbnb rental dataset using Linear Regression on Spark. The goal is to predict listing prices based on various features such as the number of bedrooms, host attributes, and more. The dataset was provided by the [Inside Airbnb project](http://insideairbnb.com/), which offers data and insights about Airbnb's impact on residential communities.

## Dataset

- **Source**: [Inside Airbnb](http://insideairbnb.com/)
- **Date Scraped**: March 6th, 2019
- **Description**: The dataset contains Airbnb listing information in San Francisco, including features such as super host status, cancellation policy, instant bookable option, and more.

## Project Structure

### 1. Data Loading

The original dataset has been downloaded, cleaned, and stored in a Delta table. To load the Delta table into a Spark DataFrame, I used the following code:

```python
filePath = "dbfs:/data/sf-listings-2019-03-06-clean.delta/"
airbnbDF = spark.read.format("delta").load(filePath)
```

### 2. Data Exploration

We begin by building a simple linear regression model to predict the listing price based solely on the number of bedrooms. During this phase, we explore the dataset using Spark's visualization tools and summary functions, noting any outliers that may affect the model's accuracy.

### 3. Preliminary Modeling

- **Data Split**: The dataset is split into training (80%) and testing (20%) sets. seed = 42 for reproducibility. 
- **Model**: A simple linear regression model is built using the number of bedrooms as the predictor.
- **Metrics**: The model's performance is evaluated using RMSE and R^2 on the testing data.

### 4. Advanced Modeling

In this step, we create a more complex linear regression model using all available variables. The steps include:

- **Data Preparation**:
  - Convert categorical variables into numerical ones using StringIndexer.
  - Apply One Hot Encoding to create "dummy" variables.
- **Model**: A linear regression model is built using a Spark ML Pipeline.
- **Metrics**: The model's performance is again evaluated using RMSE and R^2, and results are compared with the preliminary model.

### 5. Model Persistence

To avoid repeated executions of the model training codes, the pipeline model can be saved and can be reloaded for future use:

```python
# Save the pipeline model
pipelinePath = "dbfs:/models/your_pipeline_model"
pipelineModel.write().overwrite().save(pipelinePath)

# Load the pipeline model
from pyspark.ml import PipelineModel
savedPipelineModel = PipelineModel.load(pipelinePath)
```

## Results

The project demonstrates how different modeling approaches can be used to predict Airbnb listing prices in San Francisco. The advanced model provides improved accuracy by utilizing a broader set of features and leveraging Spark's ML Pipeline for efficient model training and evaluation.

The results for the 2nd model are much better than the results of the 1st model. The 2nd model gives us an RMSE of 133.46 and r-sqaured value of 0.44 as compared to the RMSE of 149.55 and r-squared of 0.30 of the preliminary model. Which is understandable because we used more information (predictors) in the 2nd model which resulted in the model to make more accurate and reliable predictions.

## Tools and Technologies

- **Apache Spark**: For large-scale data processing and machine learning.
- **Python**: For scripting and implementation of the models.
- **Databricks**: For data storage and model management.

## Conclusion

This project showcases the application of Linear Regression using Spark on a real-world dataset. By predicting Airbnb listing prices, we provide valuable insights for hosts to optimize their property listings.
