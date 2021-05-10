# Consumer-Finance-Complaints-Text-classification-with-PostgreSQL

Recently I came across [The Consumer Complaint Database](https://catalog.data.gov/dataset/consumer-complaint-database) made available by the Bureau of Consumer Financial Protection, which is a U.S. government agency, “ The Database is a collection of complaints about consumer financial products and services that they sent to companies for response. Complaints are published after the company responds, confirming a commercial relationship with the consumer, or after 15 days, whichever comes first. Complaints referred to other regulators, such as complaints about depository institutions with less than $10 billion in assets, are not published in the Consumer Complaint Database ”.

## problem Statement
Classifying Consumer Finance Complaints into one of eleven product categories, The problem is a Text classification, also known as text tagging or text categorization. Text classifiers can automatically analyze text and then assign a set of pre-defined tags or categories based on its content. In this problem, I have taken 'consumer_complaint_narrative'  as “text” and to classify each consumer_complaint_narrative / “text”  into one of eleven pre-defined categories of product.

<b>Predictors/features/independent = 'consumer_complaint_narrative'</b> 

<b>Response/target/dependent variable = 'product' </b>

## Dataset
Dataset can be downloaded from Bureau of Consumer Financial Protection US [website](https://catalog.data.gov/dataset/consumer-complaint-database)
The dataset consists of the following,
| Field Name |  Data Type |
| :----------|-----------:|
|complaint_id	 | text|
|date_received	| Date|
|product |text |
|sub_product	 | text|
|issue	 |text |
|sub_issue	 |text |
|sub_issue	 |text |
|company_public_response	|text |
|company	 |text |
|state	 |text |
|zip_code |text |
|tags	 |text |
|consumer_consent_provided	 |text |
|submitted_via	 |text |
|date_sent	 |Date |
|company_response_to_consumer	 |text |
|timely_response |text |
|consumer_disputed	 |text |

## PostgreSQL  Database 
This dataset provides the perfect opportunity for experimenting with the Database system therefore I ended up setting the PostgreSQL database for this dataset. Here are the following things I have done with it

### Accessing PostgreSQL Database Server from Python with ODBC
To establish a connection to my PostgreSQL Database Server I'm gonna be using odbc (Open Database Connectivity) 
that's the standard that says if you write to through this protocol this standard API then you can work with ODBC it accomplishes DBMS independence by using an ODBC driver as a translation layer between the application and the DBMS. The application uses ODBC functions through an ODBC driver manager with which it is linked, and the driver passes the query to the DBMS. An ODBC driver can be thought of as analogous to a printer driver or other driver, providing a standard set of functions for the application to use, and implementing DBMS-specific functionality. An application that can use ODBC is referred to as "ODBC-compliant". Any ODBC-compliant application can access any DBMS for which a driver is installed. Drivers exist for all major DBMSs and it's a plug-and-play kind of thing so there are drivers for ODBC for databases like Oracle, PostgreSQL, MySQL, and  Microsoft SQL Server  but you've to manually install DBMS of your choice and configure its driver before using it in your python code here I've already done it for PostgreSQL hence, I can use my PostgreSQL DRIVER named {PostgreSQL Unicode} in my code to establish the connection to PostgreSQL Database

### Created tables for the data
As mentioned earlier for this classification I'm going to use  'consumer_complaint_narrative'  as predictors/features/independent variables and 'product' as Response/target/dependent variable which has a total of eleven classes/categories/labels therefore I'm going to create a total of eleven tables in my Database

following are the tables being created in the database
- Mortgage'
- CreditReporting'
- StudentLoan
- DebtCollection 5CreditCard
- BankAccountORservice
- ConsumerLoan         
- MoneyTransfers
- PaydayLoan
- PrepaidCard
- therFinancialService
### Loaded Data into tables from data frames 
After creating my tables now I've to load data from data frames into tables 
### Using views
I have to deal with the missing values in my features. but in this problem of text classification, the only field I will use as features is 'consumer_complaint_narrative', which I will later convert from text into the numeric form, but before that, I have to deal with missing values in this 'consumer_complaint_narrative' Text field and in the case of text classification I can't use imputation to set my missing values to some value, therefore, I have to consider only those columns that have 'consumer_complaint_narrative' not equal to NULL and the way I'm going to deal with missing values is by using PostgreSQL Views and UNION operation by first creating twenty-two Views, eleven for complaints where 'consumer_complaint_narrative' is not null for all eleven original tables and eleven where'consumer_complaint_narrative' is null for all eleven original tables and then finally taking UNION of eleven Views from twenty-two Views where 'consumer_complaint_narrative' is not null
![ERD](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/DBMS_ER_diagram_(UMLnotation).jpeg)
### Union
Finally taking UNION of eleven views and created a new view with the name "with_complaints" and loaded it into a pandas data frame
![ERD](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/DBMS_ER_diagram_(UMLnotation)(1).jpeg)

## Exploratory Data Analysis using PostgreSQL
EDA by using SQL queries can be more flexible instead of doing it on Pandas Dataframes but be cautious it can act as a double-edged since it's not very efficient and faster nevertheless the flexible queries you can pose for EDA is phenomenal 

### most notorious Credit card companies by number of complaints
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/EDA.png)

### Class Imbalance check
To do classification, I need to know if my data is suffering from the Class Imbalance Problem where class distributions are highly imbalanced that will be reflected by poor precision. recall and F1 score hence now I'm gonna do a Class Imbalance check
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/PIE.jpg)
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/IMBALANCE.png)

### Visualizing the TF-IDF weighted Vectors
Visualizing embeddings give us a better understanding of how successful our embeddings are going to work as classifiers by separating different classes from higher dimensional feature space to some lower two-dimensional feature space but this requires dimensionality reduction before we can visualize it.
The various methods used for dimensionality reduction include:
* Principal Component Analysis (PCA)
* Linear Discriminant Analysis (LDA)
* Generalized Discriminant Analysis (GDA)
* Singular Value Decomposition (SVD) 


For this problem, we've used Truncated-SVD slightly modified version of SVD the difference between Truncated-SVD and PCA is that truncated-SVD doesn't center the data before computing values First, first, we've converted our higher dimensional TF-IDF Embedding/Bag of words Embedding into two-dimensional using Truncated-SVD then visualize our 2D embeddings using a scatter plot we've done it for each class so we can understand how each class separates itself from others.

![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/EMBEDDING.png)

## Model Building and Evaluation

### OneVsRest Support Vector Machine Classifier
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/REPORT1.jpg)
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/SCORE1.png)

### RandomForestClassifier
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/REPORT2.jpg)
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/SCORE2.png)


### GradientBoostingClassifier
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/REPORT3.jpg)
![pic](https://github.com/SyedMuhammadHamza/Consumer-Finance-Complaints-Text-classification-with-PostgreSQL/blob/main/Data/SCORE3.png)


## Model performance
| Algorithm        | Accuracy           |  Recall |  Precision |  F1  | 
| ---------------- |:------------------:| -------:|-----------:|-----:|
|OneVsRest Support Vector Machine Classifier|  85% | 0.85| 0.86 | 0.85|
|RandomForestClassifier| 82% |0.82 | 0.82| 0.81|
|GradientBoostingClassifier|  83% | 0.83 |0.83 | 0.82|


## Conclusion
The classification reports summarize the result and I concluded that The performance of my OneVsRest Support Vector Machine Classifier outperformed both ensembling methods namely RandomForestClassifier and GradientBoostingClassifier


## Technologies 
* Python
* Scikit-learn
* Matplotlib & Seaborn for data visualization
* NLTK
* TensorFlow
* SMOTE
* Sklearn for model building

©SyedMuhammadHamza Licensed under [MIT License]() 

