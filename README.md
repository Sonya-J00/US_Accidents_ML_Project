# US Car Accidents - Predicting Clearance Time Categories

This project develops a data-driven tool to predict US car accident clearance times, categorised as either: 

- short (< 1 hr) 
- moderate (1 - 6 hr) 
- long (6 - 24 hr) or 
- very long (> 24 hr) 

Through exploratory data analysis (EDA), key factors such as location, distance, weather conditions and month, are examined to uncover patterns influencing the time taken to clear an accident. 

A Gradient Boosting classifier model was trained, and evaluated using a confusion matrix and performance metrics such as accuracy and f1-score, to classify new incidents into a clearance time category. The results, including feature importance, are visualised through an interactive Power BI dashboard that displaysreal-time predictions, feature importance and historical trends.

These tools were developed to enable traffic management authorities anticipate road clearance durations to allocate resources efficiently, manage congestion and communicate accurate travel updates. This data solution integrates data analysis, predictive modeling and intuitive visualisations to support informed, data-driven decision making in traffic management.

# ![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)


## Dataset Content

The dataset used for this project is a subsample (500 K) of a larger dataset (~7.7 M) of car accidents that covers 49 states of the USA. The accident data were collected from January 2016 to March 2023, using multiple APIs that provide streaming traffic incident (or event) data. These APIs broadcast traffic data captured by various entities, including the US and state departments of transportation, law enforcement agencies, traffic cameras, and traffic sensors within the road networks. 

The data includes detailed records of:
- Accident details: ID, location (latitude/ longitude, timezone, state, county, city, street, zipcode), affected distance, severity level and description
- Temporal features: start and end time
- Environmental conditions: weather condition report,  temperature, pressure, precipitation wind speed and direction etc.
- Features of accident: at amenities, traffic signal, station, railway, crossing etc.


## Business Requirements

The project is designed to meet the following business needs:

- Predictive Modeling: develop a model that predicts the clearance time category of new or ongoing accidents
    - It is more important that *Long* and *Very Long* clearance times be predicted accuracy compared to *Short* and *Moderate*, as they will require more planning and resources 
- Insight Generation: identify key factors that influence prolonged clearance times, and also if the model could benefit from more detailed variables
- Dashboard Visualisation: create an interactive dashboard displaying predicted clearance categories, contributing factors and historical trends
- Decision Support: provide interpretable outputs to support operational decision making for traffic management teams 

## Hypothesis and how to validate?
- Adverse weather increases clearance times
    - Visualise changes in clearance class (stacked bar chart) and clearance times (box plot) by weather conditions. Use chi-squared and kruskal-Wallis to test for statistical significance 

- Accidents with higher severity level take longer to clear
    - Visualise changes in clearance class (stacked bar chart) and clearance times (box plot) by weather conditions. Use chi-squared and kruskal-Wallis to test for statistical significance 

- The population of the incident area will inpact clearance times
    - Visualise differences in clearance class distribution (histogram) and clearance times (scatter plot) and test for statistical significance.
    - As neither of the above visualisations gave a clear picture, population was binned, a contingency table by clearance category made, and chi-squared used to test for statistical significance

## Project Plan

**Data Cleaning**

- Import and explore dataset 
- Handle missing values
- Check for unique incident IDs
- Handdle repeated rows

**Subsampling**

- Use "Start_Time" and "End_Time" to obtain "Clearance_Time(hr)" => "Clearance_Class" (the target)
- Use "Clearance_Class" to subsample the data (500 K rows) into a smaller, 10 K row dataset with classes as equal as possible 

**Feature Engineering**

- Use "Start_Time" to extract month = > "Month"
- Use "City" and "County" to merge US Census population data => "Population"
- Aggregate States and Counties that appear less than 5 time => "State_Other" and "County_Other"
- Aggregate and simplify "Weather_Condition" => "Weather_Simplified
- Use "Street" to obtain the type of road the accident happened on => "Road_Type" 
- Align symbols and words in "Wind_Direction

**Exploratory Data Analysis (EDA)**

- Analyse distribution of numerical variables via histograms and KDE plots; check for normality
- Analyse count distribution of categorical variables via stacked bar chart; assess statistical significance between clearance class using chi-squared
- Analyse count distributio of boolean variables via stacked bar chart; assess statistical significance between clearance class using chi-squared
- Analyse clearance times by categoric variable via box plot; assess statistical significance using Kruskal-Wallis test
- Analyse clearance times by boolean variable via box plot; assess statistical significance using Mann-Whitney U test
- Analyse clearance times by numerical variables via scatter plot
- Produce a correlation matrix comparing relationships between all variables (numerical and categorical)
- Note: transformation of Clearance_Time(hr) added to aid visualisations
- Note: binning of "Population" to assess changes in clearance classes; contingency table and stacked bar chart to visualise differences; assessed statistical significance by chi-squared test

**Transformation of Numerical Variables**
- Find out which transformers work best for each numberical variable, except Start_Lat and Start_Lng, which are not technically numeric

**Preprocessing and Modeling**
- Scale numerical data and One-Hot encode categorical data
- To model with and without transformation of numerical variables
- To model with and without scaled Start_Lat and Start_Lng
- To model with and without variables that did not reach statistical significance
- To compare multiple models using GridsearchCV, including tree, logistic regression and neural network based algorithms
- To perform hyperparameter optimisation for 2 best candidates before selecting the model with overall best accuracy performance
- To extract the top 20 most important features from the model 

**Dashboard**

- 

## The rationale to map the business requirements to the Data Visualisations
* List your business requirements and a rationale to map them to the Data Visualisations

## Analysis techniques used
* List the data analysis methods used and explain limitations or alternative approaches.
* How did you structure the data analysis techniques. Justify your response.
* Did the data limit you, and did you use an alternative approach to meet these challenges?
* How did you use generative AI tools to help with ideation, design thinking and code optimisation?

## Modele Overview

The best performing model is a Gradient Boosting Classifier, optimised through hyperparameter tuning using grid search and cross-validation.

|           |   Accuracy    |    Macro f1   |
|-----------|---------------|---------------|
|  train    |     0.76      |     0.77      |
|  test     |     0.61      |     0.62      |


The model demonstrates moderate performance, with some class imbalance effects; it more effectively distinguishes *Short* (0.68) and *Very Long* (0.72) classes compared to *Moderate* (0.46) and *Long* (0.61), that likely represents overlapping patterns in the underlying features. However, the most important criteria for model performance was overall accuracy of predicting *Very Long* incidents, which what this model was optimised for.

The model attempts to balance bias and variance, but there is some overfitting and room for improvement via additional data features (e.g. persons injured, ambulence/ fire service needed), feature engineering (extraction of more data from location and datetime) or ensemble learning technicques such as model stacking. 

**Model Pipeline**

The project uses a preprocessing and model pipeline that:

- Encodes categorical variables using OneHotEncoder
- Scales numerical variables using StandardScaler
- Trains the model using cross-validation (accuracy)
- Evaluates performance using confusion matrix and classification report


## Ethical considerations
Data is anonymouse and not traceable to individuals, therefore, there are no ethical considerations that need to be taken into account.

## Dashboard Design
* List all dashboard pages and their content, either blocks of information or widgets, like buttons, checkboxes, images, or any other item that your dashboard library supports.
* Later, during the project development, you may revisit your dashboard plan to update a given feature (for example, at the beginning of the project you were confident you would use a given plot to display an insight but subsequently you used another plot type).
* How were data insights communicated to technical and non-technical audiences?
* Explain how the dashboard was designed to communicate complex data insights to different audiences. 

## Unfixed Bugs
* Please mention unfixed bugs and why they were not fixed. This section should include shortcomings of the frameworks or technologies used. Although time can be a significant variable to consider, paucity of time and difficulty understanding implementation are not valid reasons to leave bugs unfixed.
* Did you recognise gaps in your knowledge, and how did you address them?
* If applicable, include evidence of feedback received (from peers or instructors) and how it improved your approach or understanding.

## Development Roadmap
* What challenges did you face, and what strategies were used to overcome these challenges?
* What new skills or tools do you plan to learn next based on your project experience? 


## Main Data Analysis Libraries
* Here you should list the libraries you used in the project and provide an example(s) of how you used these libraries.


## Credits 

- For the dataset: Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, Radu Teodorescu, and Rajiv Ramnath. "Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights." In proceedings of the 27th ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, ACM, 2019.
- ChatGPT: for 
* In this section, you need to reference where you got your content, media and extra help from. It is common practice to use code from other repositories and tutorials, however, it is important to be very specific about these sources to avoid plagiarism. 
* You can break the credits section up into Content and Media, depending on what you have included in your project. 

### Content 

- US city and county population data from the United States Census Bureau (https://www.census.gov/programs-surveys/acs/)

### Media

- The photos used on the home and sign-up page are from This Open-Source site
- The images used for the gallery page were taken from this other open-source site


## Acknowledgements (optional)
* I would like to thank Neil and Vasi for their help and support