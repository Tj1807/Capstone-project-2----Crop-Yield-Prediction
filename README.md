# Capstone-project-2----Crop-Yield-Prediction

## Overview
This project focuses on predicting crop yield based on multiple environmental and agricultural factors such as rainfall, temperature, and pesticide usage. The goal is to build a machine learning model that can help improve agricultural decision-making and productivity.

## Project Goal
The goal of this project is to build a model that can determine the maximum crop yield that can be obtained given a set of parameters. This is a widely discussed topic within the Agriculture industry as improving crop yield reduces costs and helps provide sustanence for an ever growing population. This project focuses on trying to identify the best scenario to maximize crop yields while enabling a more sustainable form of Agriculture while reducing the environmental impacts.

## Files Used
* [pesticides](Data/pesticides.csv): Contains the information about pesticide usage in tonnes for each country per year.
* [rainfall](Data/rainfall.csv): Provides the average amount of rainfall in each country in mm per year.
* [temp](Data/temp.csv): Provides the average tempearture for each country in degrees Celsius.
* [yield](Data/yield.csv): Provides the yield for each country by crop and year. Results are measured in hectagrams per hectare.
* [df_yield](Data/df_yield.csv): This file is the combined data from all the other datasets with only relevant fields.

* The data for pesticides and yield were collected from the [FAO](https://www.fao.org/home/en/) website.
* The data for rainfall and temperature were collected from the [World Bank](https://data.worldbank.org/) website.


## Getting Started
* Begin by creating a folder to house your information.
* This folder should contain all data that was found and will eventually be where you store the notebook.
* For organizational purposes, all data has been stored in the Data folder.
* After getting the data and performing cleaning operations, create a virtual environment within your repository.
* Type `python -m venv .venv` in your terminal to setup a virtual environment.
* To activate the virtual environment type `.venv\Scripts\activate` in the terminal.
* After creating the virtual environment use `pip install -r requirements.txt` in the terminal.
* Once there, type `jupyter lab` in the terminal. If a browser does not automatically pop up, use `CTRL + Click` on the link provided.

## Requirements
1. Git
2. Python 3.7+ (3.11+ preferred)
3. VS Code Editor
4. VS Code Extension: Python (by Microsoft)
5. Jupyter Notebook

## Step-by-Step Workflow
### 1. Load the datasets
The first step is to import the required libraries and load all four datasets into pandas dataframes. The notebook uses the yield, temperature, pesticide, and rainfall files as the raw data sources.

### 2. Clean the yield dataset
The yield dataset originally includes many columns, but only the useful fields are kept for modeling. The notebook reduces it to:

- `Area`
- `Item`
- `Year`
- `Value` 
These fields are later used to create the final merged dataset.

### 3. Clean the temperature dataset
The temperature file already contains useful information, but its column names do not match the yield dataset. To make merging easier, `year` is renamed to `Year` and `country` is renamed to `Area`. This makes the temperature data consistent with the other files. 

### 4. Clean the rainfall dataset
The rainfall dataset contains the fields needed for the project, mainly:
- `Area`
- `Year`
- `average rainfall mm per year` 
The column name is adjusted during the merge so it can be used easily in the final dataframe.

### 5. Clean the pesticide dataset
The pesticide file contains several columns, but only the important ones are kept for prediction. The notebook reduces it to:
- `Area`
- `Year`
- `Value` 
This keeps only the pesticide usage amount needed for modeling.

### 6. Merge all datasets
After cleaning, the datasets are merged into one final dataframe using `Area` and `Year` as the common keys. The final merged dataframe includes:
- `Area`
- `Item`
- `Year`
- `Yield`
- `avgtemp`
- `Pesticides`
- `avgprecipitation`

This final table is the main dataset used for machine learning.

### 7. Check the cleaned dataset
The notebook verifies the merged dataset by checking its shape and structure. The final dataframe contains 28,248 rows and 7 columns, which confirms that the data is ready for exploration and modeling.

### 8. Explore the data
Before training models, the notebook performs exploratory data analysis to better understand the structure and patterns in the dataset. This helps confirm that the merged data looks usable and that the target variable and feature columns are in the expected format.

The next step was to begin using visualizations to understand the data better. A copy of the data was created so that a correlation matrix could be used. From the matrix, a few weak negative correlations were discovered. There was nothing discovered in the data though that would suggest a linear relationship.

From the images we have obtained, we can see that for our average yield, we have a right tail indicating that high yields are less common than average yields. The average precipitation seems to have a fairly normal skew while pesticides values tend to be on the lower end. Looking at the scatter plots, we see variability between all of our crops and yields. Average temperature shows that some crops have a wider variability such as wheat and rice whereas cassava and sweet potatoes are more clustered. This indicates that temperature ranges are variable depending on the crop but no significant linear relationship exists. Pesticide usuage shows that most of our yield comes from lower pesticide usage. Finally, with precipitation we still do not have a distinct linear relationship. It is noted that on precipitation, we have a larger cluster towarsd the bottom. This can indicate that crops have a threshold of precipitation when it comes to determining the yield.

To provide a more clear image, the different countries were split up into 5 groups. Each of these groups were then plotted to show the yield by average precipitation and yield by pesticide useage. The logic for this step was to see if there was a theme with precipitation and to see if pesticide use really increased the crop yield. There was not a clear relationship between a higher yield and either lower or higher precipitation. It is inferred that this has to do with countries producing the crops that fit best for that region. Again, a clear connection to higher pesticide use and higher yields was not present.

The key insights gained in this part of the project suggests that there is not a linear relationship between the features in the data. The data all falls within a normal distribution indicating that no further transformation was needed. The outliers in the data were left in as they all fall within a normal range. The importance of the EDA helped decide the direction that was taken in the model building process.


### 9. Define features and target
The project separates the data into input features and the target variable:
- Features: `Item`, `Area`, `Pesticides`, `avgtemp`, `avgprecipitation`
- Target: `Yield` 
This is the setup used for training all the machine learning models.

### 10. Encode categorical variables
Since `Item` and `Area` are categorical variables, the notebook uses one-hot encoding to convert them into a numeric format that machine learning models can understand. Numerical features are passed through directly, and some experiments also use scaling or polynomial interaction features.

### 11. Split the data
The dataset is split into training and testing sets using an 80/20 split. This allows the project to train models on one portion of the data and evaluate them on unseen data afterward. 

### 12. Train multiple models
The notebook trains and compares several regression models, including:
- Linear Regression
- Random Forest
- Gradient Boosting
- Decision Tree
- K-Nearest Neighbors
- Neural Network 

Each model is evaluated using:
- MAE
- MSE
- RMSE
- R-squared


### 13. Tune and compare models
The project also tests tuned versions of some models using hyperparameter settings. A grid search block is included in the notebook for more detailed optimization, although it is commented out because it would take significant time and resources to run. 

### 14. Select the best model
Based on the test-set results, Gradient Boosting performs the best overall. It has one of the highest test R-squared values and one of the lowest error values among the models compared. Random Forest is also very strong and close behind.

## Best Performing Model
Gradient Boosting is the best model in this project because it achieves the strongest balance between accuracy and generalization. Its test R-squared is very high, and its error values are lower than the other models tested.

## Final Output
The final result of the project is a trained crop yield prediction pipeline that can estimate yield based on crop type, temperature, pesticide use, and rainfall. The notebook also saves the cleaned combined dataframe for later use.

## How To Run the Project
1. Open `crop_yield1.ipynb` in Jupyter Notebook or JupyterLab. 
2. Place the CSV files in the correct working directory. 
3. Run the data cleaning cells to prepare each dataset.
4. Run the merge cells to create the final dataframe.
5. Run the preprocessing and model training cells.
6. Compare the evaluation metrics for each model.
7. Use Gradient Boosting as the final model if you want the best performance.

## Technologies Used
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Scikit-learn
