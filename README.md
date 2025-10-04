### Project Title
Data Analysis of California Wildfires from 1992 - 2020

**Author**
Stephen Scheiman

#### Executive summary
* Overview and Goals
This study examines the relationship between rising global temperatures and the frequency and destructiveness of wildfires in California between 1992 and 2020. The findings highlight the growing environmental, economic, and social risks posed by climate-driven wildfire activity, with implications for land management agencies, insurers, policymakers, and residents.
* Findings and Results
Results based on Exploratory Data Analysis (EDA) and modeling indicate that California's wildfires, while no more frequent during the study period, are significantly more severe in the amount of acreage burned per year. Additionally, I was able to determine basic characteristics of California Wildfires such as the counties that experience the most wildfire and most acreage burned, the most prevalent and damaging months for wildfire starts as well as the leading causes of wildfire in the state over the course of the study period. 

We were able to correlate rising average high temperatures to wildfire severity, with a correlation of almost sixty percent. While forest mismanagement certainly plays a role in the rising danger of catastrophic wildfire, these large scale conflagrations are highly correlated with rising average high temperatures. Rising temperatures don’t just correlate with wildfire activity in California—they are a primary driver. The hotter the atmosphere, the drier the vegetation and soils, and the more explosive the fires become. Human land use and fire suppression policies amplify this climate signal, but the underlying physics of heat and dryness are the dominant force.

#### Rationale
Preliminary review of the data and established reasoning suggest that both the incidence and severity of wildfires in California have risen over the last ten years. An in-depth understanding of these trends is critical from multiple standpoints:

1.	Environmental impacts encompass ecosystem disruption, loss of biodiversity, and exacerbation of climate feedback mechanisms. These matters require careful consideration by land management agencies when developing mitigation strategies.

2.	The human and economic consequences are significant for insurers and other stakeholders with financial interests in the region. Notably, losses from the 2018 California wildfires alone exceeded $148 billion. Therefore, enhanced insight into the temporal and spatial patterns of wildfire events can empower agencies, businesses, and individuals to make more informed land use and risk mitigation decisions.


#### Research Question
How have shifts in global temperature trends influenced the frequency and severity of wildfires in California from 1992 - 2020?

#### Data Sources
Wildfire data was sourced from the “Fire Program Analysis Fire-Occurrence Database (FPA FOD)”: Short, Karen C. 2022. Spatial wildfire occurrence data for the United States, 1992-2020 [FPA_FOD_20221014]. 6th Edition. Fort Collins, CO: Forest Service Research Data Archive. https://doi.org/10.2737/RDS-2013 06<br>https://www.fs.usda.gov/rds/archive/products/RDS-2013-0009.6/RDS-2013-0009.6_Data_Format4_SQLITE.zip This data source is peer reviewed and a comprehensive analysis of wildfire in the United States from 1992 – 2020. It will provide the data I need with respect to the frequency and severity (measured in acres burned) of wildfires in California. I adopt the timeframe of this dataset as my study period.

Geospatial data is sourced from Natural Earth. Natural Earth provides shape files for the basemap used in our geospatial analysis. This data source proved to be the easiest to implement while learning basic geospatial analytics on-the-fly. Dataset here: https://www.naturalearthdata.com/http//www.naturalearthdata.com/download/110m/cultural/ne_110m_admin_1_states_provinces.zip

Historic temperature data was sourced from Extremeweatherwatch.com (https://www.extremeweatherwatch.com/states/california/average-temperature-by-year). This data source was chosen as it had the data for the requisite study period. 

#### Methodology
Following data cleaning—which includes handling missing values and outliers—I will perform feature engineering as appropriate. Subsequent exploratory data analysis will employ methods such as geospatial analysis and descriptive statistics. To identify patterns and gain insights, I plan to implement regression, classification, and advanced geospatial analytical techniques.

#### Data Preprocessing/Preparation
* Wildfire Data
Based on the size of the original SQLite database, the first step in preprocessing was to create our initial DataFrame from a query that extracted only the California data needed for this study. Leveraging basic tools such as .info(), I was able to understand which columns would be needed and which could be discarded due to missing data. However, a required column containing the California county name, was missing a fair amount of data. Rather than lose this valuable information, I came up with a couple of solutions to impute this data, leveraging the longitude and latitude data in each record. The first solution attempted to leverage the Nominatim API for OpenStreetMap. However, this would have taken somewhere in the vicinity of thirty-six hours to complete. The second and final solution was to leverage a Random Forest Classifier, trained on the records which had both county name and latitude/longitude, to classify the missing county name. Our model proved to be ninety-nine percent accurate at predicting county name from latitude and longitude, the best performing model of this study! The cleaned and imputed dataset was then written to csv file and used as the study dataset.
Several columns were datetime encoded after splitting of data strings, and I leveraged an Ordinal Encoder and Standard Scaler on our DataFrame as well. 
* Geospatial Data
We created a geometry array of datapoints by zipping the latitude and longitude data from our wildfire dataset and created a GeoDataFrame from this data. I then leveraged this GeoDataFrame to anchor and display the California state basemap for our geospatial analyses. 
* Historic Temperature Data
California historic temperature data presented an opportunity to use Pandas to read an html table and create a DataFrame from what is essentially a screen scrape. The data was datetime encoded and “cropped” to the study period timeframe.
Data was grouped by various parameters during the Exploratory Data Analysis (EDA) stage for analysis. Data used for modeling was split into Train and Test sets using the “train_test_split” library from Scikit Learn Model Selection, using default parameters.

#### Models Deployed
* Linear Regression:
A simple model to predict wildfire severity based solely on the calendar year. Ridge and Lasso regression were also tested in this capacity.
* Tensorflow/Keras
A more complex time-series model with which to predict wildfire severity in the future.
* DBSCAN
Leverage DBSCAN to understand any geographic clusters or patterns in the data.
* Random Forest Classifier (RFC)
Used to predict the causes of wildfire given certain features as well as predicting the Fire Size Class from similar features.
* Nearest Neighbors
Used to build the K-Distance Graph that informed our clustering hyperparameter selection used in DBSCAN.
* Support Vector Machine
An alternative to RFC to test performance against.

#### Model Outcomes
Multiple supervised and unsupervised machine learning models were used in this study to perform both regression and classification tasks, including Linear Regression for simple predictions, Random Forest Classification for predicting wildfire causes and DBSCAN for our clustering exercise.

Review of the data and established reasoning suggest that both the incidence and severity of wildfires in California have risen over the study period. However, this study found that the number of wildfires starts per year during the study period did not show an upward trend. That said, the study also found that the severity of wildfires, i.e., the number of acres burned by wildfire fire was steadily increasing. 

The models also showed that severe wildfires tend to occur in specific parts of the state and that the destructiveness of wildfires varies by county and by location in general. I was also able to ascertain the leading causes of wildfire in the state as well as the months when wildfire was most prevalent and destructive. While Riverside County had the most wildfire incidents during the study period, San Diego County experienced the most damage. The question as to why that is presents an opportunity for further study, perhaps leveraging satellite image data and Convolutional Neural Networks.

Our models showed a high correlation between warming temperatures and wildfire severity, an alarming trend for anyone living in the state. This fact is not lost on Insurers in the state, most of whom won’t insure homes in high wildfire risk areas, forcing property owners to purchase insurance from the CA-FAIR Plan, a state-backed insurer whose rates are higher than most people’s mortgage payments.

Lastly, our analysis uncovered that the most destructive cause of wildfire in the state is “Natural” a/k/a Lightning. This makes sense in that lightning-started fires tend to be in remote locations, making containment more difficult for wildland fire fighters.

For the regression models I leveraged Mean Squared Error to evaluate the accuracy of each model. As suspected, these simple models based solely on calendar year performed quite poorly. While it was possible to see an upward trend in the regression models, the ability to predict future wildfire size was woefully inaccurate. The same can be said for our Keras Time-Series Model. Perhaps due to the wide variance in our dataset, we were unable to create a model that accurately represented our data and therefor our model’s predictions were extremely inaccurate. The Random Forest Classifiers used for our classification exercises proved much more adept at predicting both the causes of fire and the fire size given a small set of features. 



#### Next steps


#### Outline of project

- https://github.com/stephen-scheiman/Capstone


##### Contact and Further Information
Do Not Contact