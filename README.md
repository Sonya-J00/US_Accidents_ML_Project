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
- Insight Generation: identify key factors that influence prolonged clearance times
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
|  train    |     0.79      |     0.79      |
|  test     |     0.61      |     0.62      |


The model demonstrates moderate performance, with some class imbalance effects; it effectively distinguishes *Short* (0.69) and *Very Long* (0.73) classes compared to *Moderate* (0.43) and *Long* (0.61). This likely represents overlapping patterns in the underlying features.

The model attempts to balance bias and variance, but there is some overfitting and room for improvement via additional data features (e.g. persons injured, ambulence/ fire service needed), feature engineering (extraction of more data from location and datetime) or ensemble methods. 

**Model Pipeline**

The project uses a preprocessing and model pipeline that:

- Encodes categorical variables using OneHotEncoder
- Scales numerical variables using StandardScaler
- Trains the model using cross-validation (accuracy)
- Evaluates performance using confusion matrix and classification report


## Ethical considerations
* Were there any data privacy, bias or fairness issues with the data?
* How did you overcome any legal or societal issues?

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

## Deployment
### Heroku

* The App live link is: https://YOUR_APP_NAME.herokuapp.com/ 
* Set the runtime.txt Python version to a [Heroku-20](https://devcenter.heroku.com/articles/python-support#supported-runtimes) stack currently supported version.
* The project was deployed to Heroku using the following steps.

1. Log in to Heroku and create an App
2. From the Deploy tab, select GitHub as the deployment method.
3. Select your repository name and click Search. Once it is found, click Connect.
4. Select the branch you want to deploy, then click Deploy Branch.
5. The deployment process should happen smoothly if all deployment files are fully functional. Click now the button Open App on the top of the page to access your App.
6. If the slug size is too large then add large files not required for the app to the .slugignore file.


## Main Data Analysis Libraries
* Here you should list the libraries you used in the project and provide an example(s) of how you used these libraries.


## Credits 

* In this section, you need to reference where you got your content, media and extra help from. It is common practice to use code from other repositories and tutorials, however, it is important to be very specific about these sources to avoid plagiarism. 
* You can break the credits section up into Content and Media, depending on what you have included in your project. 

### Content 

- US city and county population data from the United States Census Bureau (https://www.census.gov/programs-surveys/acs/)

### Media

- The photos used on the home and sign-up page are from This Open-Source site
- The images used for the gallery page were taken from this other open-source site



## Acknowledgements (optional)
* Thank the people who provided support through this project.