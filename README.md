# Final-Project-Exoplanet-Classification-with-XGBoost

## Project/Goals
The goal of the projects tackle a real world dataset and train a machine learning model that can solve a unique problem. 

## Process
### Step 1 - Planning/Strategizing
This step consisted of reviewing the project outline, and mapping out an approach prior to starting the work. This allowed for given steps to be followed to ensure that project requirements were not missed, and that time management was done effectively.  

Early planning allowed the project to stay on task, and complete the main components and the presentation with a day to spare. As a result, we can take some time to implement some of the next steps outlined in the presentation. 
### Step 2 - Experimenting/Testing
The data was explored and manipulated in order to better understand it's structure, and also identify any issues. 

This consisted of identifying potential outliers, and handling null values. Although we were able to use various methods to detect possible outliers, without domain knowledge, it was safer to assume that these measurements were correct, and should not be removed. 

Once a good understanding of the data was established, the process of data cleaning began. This included a number of standard practices such as removing NULL/NaN values and duplicates, as well as performing encoding on the target variables. 

The data for nearly all the variables was heavily skewed, and had a very high variance. This created some challenges during this stage, but was expected as it was measurements on an astronomical scale.

### Step 3 - Analyzing
Analyzing the data consisted of 2 separate parts: EDA, which involved investigating the descriptive statistics of the data as well as data visualization, and then additional preprocessing steps with evaluation metrics. 

6 Initial models were ran without any parameters or further processing: KNN, DTC, RFC, LogReg, SVM, XGBoost.

Our XGBoost model performed the best, and continued to do so even when some basic preprocessing was applied like scaling. As a result, this was chosen as the base for our final model. 

Additional preprocessing steps were trialed to determine if we could further improve our model performance. Our main evaulation metric was the f1-score due to the nature of our data. 
- Feature selection
- Logarithmic scaling
- potential outlier removal
- SMOTE to address class imbalance
- PCA

Despite our best efforts, we were able to gain very little improvements on the performance of the model. The best results came from simply removing the error columns that accompanied most of the variables 2 to 1. Even this only resulted in a couple percent improvement over the untuned XGBoost model.

This challenge was partly due to a lack of domain knowledge, but we were able to demonstrate that an accurate model can still be generated with a data driven approach to evaluation. 

Cross validation was also computed to ensure generalization of our model. 

### Step 4 - Interpretation and Results
Lastly we will want to interpret the results of our model. 

Below is a classifcation report of our final model: 

| Classification Report |       |         |         |        |
|-----------------------|-------|---------|---------|--------|
| Class                 | Precision | Recall  | F1-Score | Support |
|-----------------------|-------|---------|---------|--------|
| 0                     | 0.85  | 0.77    | 0.81    | 557    |
| 1                     | 0.80  | 0.87    | 0.84    | 573    |
| 2                     | 0.99  | 0.99    | 0.99    | 1122   |
|                       |       |         |         |        |
| accuracy              |       |         | 0.91    | 2252   |
| macro avg             | 0.88  | 0.88    | 0.88    | 2252   |
| weighted avg          | 0.91  | 0.91    | 0.91    | 2252   |

The most important class would be our class 1, or the "CONFIRMED" class as it is labelled in the original dataset. The macro average can be viewed to measure our overall performance across all classes. We see here, and within the notebooks in this repository, that our model does an excellent job of predicting our majority class. From the confusion matrix included in our repo, we can see that the inaccuracies come from determining if a sample is either a CANDIDATE or a CONFIRMED class. 

This is expected however. The FALSE POSITIVES we know have failed at least one test that is required to verify the sample as an Exoplanet. The CANDIDATES however have already passed some test to be verified, but testing is not yet complete. So essentially they are closer to a CONFIRMED planet than they are to a FALSE POSITIVE. 

As a next step, we could retrain our model on only the false positives and confirmed classes, and use that model to predict the outcome of all the candidate classes. This is another application of nearly the exact same problem. 

## Results
Overall, we have a strong performing model, that is able to predict exoplanets based on the input variables. We seem to have plateaued in terms of performance when applying the XGBoost model, however, we may be able to realize performance gains if we expand our domain knowledge. 

## Challenges 
There were a few core challenges during this project. Most of which revolved around our lack of domain knowledge. 

The different challenges are outlined below:
- Understanding the different variables
  - Despite descriptions being provided by NASA, it was still difficult to understand the expected scale, or variance of a given variable. 
  - Many of the units appeared to be in units like "flux" or use abnormal scaling such as relative to our Earth or Sun
- The error values were a bit of a red herring
  - Much time was spent, and potentially wasted in an attempt to draw information from the error values, however, without further domain knowledge it was difficult to use them for detectin outliers or such
  - It is possible they hold valuable information, but more knowledge of the variables themselves, or more time would be required in order to extract useful information from them for the sake of our model
- Lack of data
  - Our dataset was not small, but it was also not large. After removing null values, we were left with 9007 datapoints. 
  - Although we were able to get a model trained, we seemed unable to breach the 90% macro average point with what we had
- Small gains provided little feedback
  - As the performance gains were small, negative, or negligible, there was little to not feedback in terms of what worked with the data and what didn't
  - Again, with further domain knowledge we may know what to try in terms of feature engineering, scaling, or model tuning in order to optimize performance

## Future Goals
With more time, a number of changes would be made that would a) improve the model performance b) improve the usability of the model

- As for improving model performance, there seems to be two remaining strategies
    1. Improve domain knowledge, which will let us optimize our preprocessing and possible squeeze more information out of our data
    2. Implement deep learning such as neural networks in order to improve performance. 
        - XGBoost is already one of the best performing classifiers, without NN's involved. As such, we may not be able to get much higher performance without more advanced techniques
- For the usability, right now all of the work is in jupyter notebooks, and the model needs to be saved using pickle or otherwise. 
  - A flask app or custom API that would allow users to access the model would greatly improve it's usability
  - The Kepler telescope has an API that could also be used for futureu predictions, or further training of the model. 
  - Implementing a proper pipeline would allow the processing of new data to happen seamlessly instead of repurposing the existing notebooks
  - Converting the code into proper scripts and functions would also help with the transferability of the code, and potential generalization to other applications 
