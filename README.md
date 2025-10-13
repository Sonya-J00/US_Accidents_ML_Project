# US Car Accidents - Predicting Clearance Time Categories

<img src="Assets/Pictures/Title_pic.jpg" width="400" height="400">

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

- The population of the incident area will impact clearance times
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
- Analyse count distribution of boolean variables via stacked bar chart; assess statistical significance between clearance class using chi-squared
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
- Outline dashboard design Balsamiq wireframes
- Main Dashboard - cards to predict total clearance times and median clearance times, slicers to investigate main important features, summary bar chart of clearance times
- Map View - bubble map of accidents, highlighting clearance times, with slicers to investigate main most important features
- Main Features - more in-depth view of the main features themselves, along with slicers to investigate
- Overall, the complex dataset will be simplified by only incorporating the top important features (also those of particular interest). Summary cards will be used to clearly display the main predictive outcomes of clearance times, and charts to visualise clearance categories and median clearance times by selected features. Slicers for the most important features gives the user the ability to drill down and narrow predictions.

## The rationale to map the business requirements to the Data Visualisations
* Preductive modeling: confusion Matrix (train/test), classification Report (precision, recall, F1-score) and feature Importance plot
    - These visuals evaluate model performance and ensure it generalises well
* Insight Generation: - box plots of Clearance_Time(hr) by each variable, stacked bar charts showing clearance class distribution by categorical variables, correlation heatmap across all variables. together with statistical testing
    - These visuals reveal relationships between independent variables and clearance duration
* Dashboard Visualisation: interactive Power BI visuals including stacked bar charts and map that display the most important features
    - The interactive visuals enable stakeholders to explore how clearance times vary by geography, time, and environmental conditions. This supports pattern recognition and trend analysis without requiring technical analysis, enhancing usability for traffic management teams
* Decision Support: feature importance plot, summary statistics and clearance category distributions
    - These visuals communicate the basis of the model, showing why certain predictions are made. Clear interpretability helps decision-makers trust the model and take actionable steps e.g., allocating additional resources in high-risk areas

## Analysis techniques used

**Data Analysis Methods**
The project combined exploratory, statistical, and predictive modeling approaches to understand and predict accident clearance times:
- Exploratory Data Analysis (EDA): Used descriptive statistics, histograms, KDE plots, box plots, and scatter plots to assess distributions and relationships between variables.
- Statistical Testing: Employed Chi-squared, Kruskal–Wallis, and Mann–Whitney U tests to assess significance between clearance categories and predictors (e.g., weather, severity, population).
- Feature Engineering: Simplified, aggregated and derived new features (Clearance_Class, the target), encoded categorical features, and scaled numerical values for modeling.
- Predictive Modeling: Trained and evaluated classification models to predict clearance time categories, with emphasis on interpretability (feature importance, confusion matrix, classification report).

**Limitations and Alternatives**
Several constraints shaped the analytical strategy:
- Data imbalance: Fewer *Very Long* clearance cases may have led to a skewed model performance, however, *Very Long* was still the best performing class. class weighting using Random Forest Classification was tried but did not result in improvement. Alternatives such as SMOTE could improve balance.
- Non-normal distributions: Clearance times and many numeric variables were heavily skewed, limiting the applicability of parametric tests. Non-parametric alternatives (Kruskal–Wallis, Mann–Whitney U) were therefore used.
- Missing or incomplete variables: Some potential predictors (e.g., staffing levels, traffic volume, casualties) were unavailable, reducing explanatory power. This was mitigated by feature engineering and focusing on interpretable features.

**Structuring of Analysis**
The analysis followed a progressive, hypothesis-driven structure:
- Data cleaning
- Target and feature engineering
- EDA to understand distributions and correlations
- Statistical validation to confirm significant patterns
- Feature selection based on EDA insights
- Preprocessing and model training. Evaluation guided by business priorities (accurate prediction of *Very Long* categories).

**Role of Generative AI Tools**
Generative AI tools (such as ChatGPT) were used for:
- Ideation: Refining hypotheses and selecting appropriate statistical tests.
- Design Thinking: Structuring visualisations and analysis flow to align with business requirements.
- Code Optimisation: Debugging and improving Python scripts (e.g., automating subplots, tuning model evaluation code, refining plotting logic).
All outputs were reviewed manually to ensure methodological validity and reproducibility.

## Modele Overview
The best performing model is a Gradient Boosting Classifier, optimised through hyperparameter tuning using grid search and cross-validation.

|           |   Accuracy    |    Macro f1   |
|-----------|---------------|---------------|
|  train    |     0.76      |     0.77      |
|  test     |     0.61      |     0.62      |


The model demonstrates moderate performance, with some class imbalance effects; it more effectively distinguishes *Short* (0.68) and *Very Long* (0.72) classes compared to *Moderate* (0.46) and *Long* (0.61), that likely represents overlapping patterns in the underlying features. However, the most important criteria for model performance was overall accuracy of predicting *Very Long* incidents, which what this model was optimised for.

The model attempts to balance bias and variance, but there is some overfitting and room for improvement via additional data features (e.g. persons injured, ambulence/ fire service needed), feature engineering (extraction of more data from location and datetime) or ensemble learning technicques such as model stacking. 

**Feature Importance**
- The top 10 most important features were found to be: Distance(mi), State_Other, Start_Lat, Start_Lng, County_Other, Pressure(in), Population, Temperature(F), Humidity(%), Severity
- Demonstrates that Size of the accident area is most important feature, followed by location and then environmental conditions
- Illustrates a need to better understanding how Severity is reported 

**Model Pipeline**
The project uses a preprocessing and model pipeline that:

- Encodes categorical variables using OneHotEncoder
- Scales numerical variables using StandardScaler
- Trains the model using cross-validation (accuracy)
- Evaluates performance using confusion matrix and classification report


## Ethical considerations
Data is anonymouse and not traceable to individuals, therefore, there are no ethical considerations that need to be taken into account.

## Unfixed Bugs
* No unfixed bugs

## Development Roadmap

**Challenges and Strategies**
Throughout the project, several technical and analytical challenges were encountered:
- Data quality and missing values: the raw dataset contained missing and inconsistent entries across key variables (e.g., weather, wind direction). These were addressed through data cleaning, mapping to simplified categories, and domain-informed imputations to maintain interpretability without introducing bias.
- Imbalanced clearance categories: the *Very Long* clearance classes was underrepresented, which could have impacting model generalisation. To mitigate this, macro-averaged metrics were prioritised over accuracy for fairer performance evaluation.
- Non-normal distributions and extreme outliers: many numerical variables showed heavy skewness, which limited the use of parametric tests. Non-parametric methods (Kruskal–Wallis, Mann–Whitney U) and robust visualisations, together with transformation of clearance times (boxplots with log scaling) were used instead. A large proportion of the data were statistical outliers. However, these were kept as is, as they fell within expected and valid ranges.
- Model interpretability: ensuring the predictive model provided interpretable outputs for decision-makers required careful model tuning. Tree-based models were chosen for their balance between performance and explainability, and feature importance plots were used to highlight key predictors of clearance time.
- Multiple visualisation requirements:creating clear, meaningful plots across many variables required iterative refinement. Seaborn and Matplotlib were used in combination, with automation of subplot generation and consistent labeling to improve readability and reproducibility.

**Next Steps and Skill Development**
Based on insights gained during this project, several areas for further growth were identified:

Advanced Model Optimisation:
Learn and apply Bayesian optimisation and ensemble stacking techniques to improve model robustness and predictive power.

Model Explainability and SHAP Analysis:
Incorporate SHAP or LIME methods for deeper understanding of individual prediction drivers and to enhance model transparency for stakeholders.

Dashboard Deployment:
Expand technical skills in Plotly Dash or Streamlit for building an interactive dashboard integrating predictions, key insights, and historical trends.

MLOps and Deployment:
Learn MLflow or Docker for model tracking and deployment, ensuring reproducibility and scalability in real-world traffic management systems.
* What challenges did you face, and what strategies were used to overcome these challenges?
* What new skills or tools do you plan to learn next based on your project experience? 


## Main Data Analysis Libraries
* Here you should list the libraries you used in the project and provide an example(s) of how you used these libraries.


## Credits 
- For the dataset: Moosavi, Sobhan, Mohammad Hossein Samavatian, Srinivasan Parthasarathy, Radu Teodorescu, and Rajiv Ramnath. "Accident Risk Prediction based on Heterogeneous Sparse Data: New Dataset and Insights." In proceedings of the 27th ACM SIGSPATIAL International Conference on Advances in Geographic Information Systems, ACM, 2019.
- ChatGPT: for ideation, design thinking and code optimisation
- Code Institute Data Analytics with AI learning management system

### Content 
- Kaggle for the US Accidents Dataset (https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents/)
- US city and county population data from the United States Census Bureau (https://www.census.gov/programs-surveys/acs/)

### Media
- The photos used on the home and sign-up page are from This Open-Source site
- The images used for the gallery page were taken from this other open-source site


## Acknowledgements (optional)
* I would like to thank Neil and Vasi from Code Institute for their help and support