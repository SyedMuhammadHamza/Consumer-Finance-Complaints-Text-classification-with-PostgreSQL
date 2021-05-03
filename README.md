# Consumer-Finance-Complaints-Text-classification-with-PostgreSQL

Recently I came across [The Consumer Complaint Database](https://catalog.data.gov/dataset/consumer-complaint-database) Published by the Bureau of Consumer Financial Protection, which is a U.S. government agency, “ The Database is a collection of complaints about consumer financial products and services that they sent to companies for response. Complaints are published after the company responds, confirming a commercial relationship with the consumer, or after 15 days, whichever comes first. Complaints referred to other regulators, such as complaints about depository institutions with less than $10 billion in assets, are not published in the Consumer Complaint Database ”.

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

PostgreSQL
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
