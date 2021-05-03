# Consumer-Finance-Complaints-Text-classification-with-PostgreSQL

Recently I came across [The Consumer Complaint Database](https://catalog.data.gov/dataset/consumer-complaint-database) Published by the Bureau of Consumer Financial Protection, which is a U.S. government agency, “ The Database is a collection of complaints about consumer financial products and services that they sent to companies for response. Complaints are published after the company responds, confirming a commercial relationship with the consumer, or after 15 days, whichever comes first. Complaints referred to other regulators, such as complaints about depository institutions with less than $10 billion in assets, are not published in the Consumer Complaint Database ”.

## problem Statement
Classifying Consumer Finance Complaints into one of eleven product categories, The problem is a Text classification, also known as text tagging or text categorization. Text classifiers can automatically analyze text and then assign a set of pre-defined tags or categories based on its content. In this problem, I have taken 'consumer_complaint_narrative'  as “text” and to classify each consumer_complaint_narrative / “text”  into one of eleven pre-defined categories of product.

<b>Predictors/features/independent = 'consumer_complaint_narrative'</b>
<b>Response/target/dependent variable = 'product' </b>
