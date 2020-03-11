# <center>Predicting user who unsubscribe using PySpark
## Project Overview
Sparkify is a popular digital music service where many of users stream their favorite songs everyday using either free or paid services. This project is to develop a model that predict which users are at risk cancelling their service. If we can accurately identify those users before they leave, the business can offer them incentives and discounts potentially saving millions in revenues.
The full details of the analysis and code can be found at [here](Sparkify.ipynb).

## Data
### Data Processing
The underlying data used to develop the model is a two-month subeset of the whole behavioral data. It contains a total of 286,500 rows of user events and has 18 columns. 278,154 out of the 286,500 are valid records and the rest are with missing UserId. Those null UserID records are related to unregistered users and hence are removed from the dataset as they are irrelevant for analysing unsubscription activity.

**Columns:** <br>
 - artist: string (nullable = true)<br>
 - auth: string (nullable = true)<br>
 - firstName: string (nullable = true)<br>
 - gender: string (nullable = true)<br>
 - itemInSession: long (nullable = true)<br>
 - lastName: string (nullable = true)<br>
 - length: double (nullable = true)<br>
 - level: string (nullable = true)<br>
 - location: string (nullable = true)<br>
 - method: string (nullable = true)<br>
 - page: string (nullable = true)<br>
 - registration: long (nullable = true)<br>
 - sessionId: long (nullable = true)<br>
 - song: string (nullable = true)<br>
 - status: long (nullable = true)<br>
 - ts: long (nullable = true)<br>
 - userAgent: string (nullable = true)<br>
 - userId: string (nullable = true)

Graphs below show distributions of churn/don't churn users by some key categories:

<img src="Image/Churn by Gender.png">

<img src="Image/Churn by Gender.png">

<img src="Image/Churn by Page.png">

### Feature Engineering
As the data provided is the behaviour data, each user could have multiple records. In order to predict the possibility of churning, we need to create a modelling dataset of user characteristics with one line for each UserId. A list of features which look promising have been considered and the selection is an iterative process based on the model performance. 

The selected features are based in part on user characteristics, including gender, free or paid user and registration time. The other part is based on user behaviour, such as average number of songs per session, average number of days logged in per month, average number of activities of thumbsdown, thumbsup, rollad etc. and average time per session.

**Engineered Training Data**
 - userId: string (nullable = true)
 - churn: string (nullable = true)
 - gender_Male: string (nullable = true)
 - gender_Female: string (nullable = true)
 - free_flag: string (nullable = true)
 - paid_flag: string (nullable = true)
 - free_paid_flag: string (nullable = true)
 - avg_songs_persession: string (nullable = true)
 - num_session: string (nullable = true)
 - avg_days_login_permonth: string (nullable = true)
 - avg_thumbsdown_permonth: string (nullable = true)
 - avg_thumbsup_permonth: string (nullable = true)
 - avg_playlist_permonth: string (nullable = true)
 - avg_addfriend_permonth: string (nullable = true)
 - avg_rollad_permonth: string (nullable = true)
 - avg_help_permonth: string (nullable = true)
 - avg_error_permonth: string (nullable = true)
 - regDay: string (nullable = true)
 - avg_session_hour: string (nullable = true)
 
 ## Modelling
To build our model, the modelling dataset was firstly transformed in a way that each row is a single feature vector and the data in each column was normalised. The datast was then split into training and validation data in a ratio of 80% to 20%.

The following three supervised learning algorithms are then considered:
1. Logistic Regression (LR)
2. Decision Tree Classifier (DTC)
3. Gradient Boosted Tree Classifier (GBT)

In order to find the best hyper-parameters for each of the models, grid search and a 3-fold cross validation were performed on the dataset.

## Conclusion
The model metrics are as followed:

Logistic Regression:
- Accuracy:  0.93(Train) &nbsp; &nbsp; 0.73(Test) <br>
- F1: &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;0.93(Train) &nbsp; &nbsp; 0.68(Test)<br>

Decision Tree:
- Accuracy:  0.99(Train) &nbsp; &nbsp; 0.75(Test) <br>
- F1: &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;0.99(Train) &nbsp; &nbsp; 0.75(Test)<br>

Gradient Boosted Tree Classifier:
- Accuracy:  0.96(Train) &nbsp; &nbsp; 0.75(Test) <br>
- F1: &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;0.96(Train) &nbsp; &nbsp; 0.75(Test)<br>

All of the three machine learning models performed well in predicting those customers' who will most probably end in unsubscribing. By comparing the model accuray and F1 scores on both train and test datasets, the Decision Tree Classifier model appears to be the best one. In terms of the feature importance, the average hours per session is proved to be the most important feature in predicting unsubscribing activity which overwhelmingly contributes 70%.
All of the three machine learning models performed well in predicting those customers' who will most probably end in unsubscribing. By comparing the model accuray and F1 scores on both train and test datasets, the Decision Tree Classifier model appears to be the best one. In terms of the feature importance, the average hours per session is proved to be the most important feature in predicting unsubscribing activity which overwhelmingly contributes 70%.

<img src="Image/Feature Importance.png">

## Summary 
In this project, a model was built to predict user churn from Sparkify - a song streaming service. Feature engineering was firstly performmed following familiarising and understanding the dataset mainly based on the activities of the users. The data was fed into three supervised machine learning models. Out of those, Decision Tree classifier provided the best results. It was also found that the average hours per session is the particularly most important feature that helps in predicting user churn.
