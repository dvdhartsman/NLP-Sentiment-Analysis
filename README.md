# NLP Text Classification
Analyzing Twitter Posts Regarding Apple and Google

David Hartsman (dvdhartsman@gmail.com)
Reyn Chagami (rchagami17@gmail.com)
Andrew Hendricks (andrewhhendricks@gmail.com)

Find our non-technical presentation [here](https://docs.google.com/presentation/d/12J7u8S0OZltTBgUNQsBwZ3RKaHuetXo6/edit?usp=sharing&ouid=106491021188736703963&rtpof=true&sd=true).

Instructions for navigating the repo:
- [Lemmatization_binary-modeling.ipynb](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/blob/main/EDA%20Notebooks/Lemmatization_binary-modeling.ipynb) - contains code for our preprocessing and binary models
- [One_vs_rest.ipynb](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/blob/main/EDA%20Notebooks/One_vs_rest.ipynb) - contains code for our positive classification models and negative classification models
- [Multiclass_analysis.ipynb](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/blob/main/Multiclass_analysis.ipynb) - contains code for our mutli-class classification model
- [Text_Classification_Final_Notebook.ipynb](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/blob/main/Text_Classification_Final_Notebook.ipynb) - contains a condensed version of the project to highlight most successful models and overall process

## Overview
Social media provides organizations with more data than ever about how the public is engaging with their products and brands. Using text classification to process all of that data into useable insights gives organizations the opportunity to incorporate public perceptions into their operations in new and unprecedented ways.

In this project, we take on the role of a consulting firm creating an NLP text classification prototype that we are pitching to the heads of the analytics and operations teams for SXSW 2024. We use Twitter data from the 2013 SXSW conference to create a model that classifies positive, negative, and neutral Tweet sentiment. We conclude by sharing ideas for how the SXSW operations team could use the results of our model to enhance future conferences and ideas for next steps.

## Business Problem and Data
The stakeholders for this project are the SXSW 2024 operations team and analytics team, and the business problem is developing a model that classifies Tweets based on positive, negative, or neutral sentiment. Based on the business context, we develop three separate models: a binary model to classify positive Tweets, a binary model to classify negative Tweets, and a multi-class model to classify positive, negative, and neutral Tweets. We train these models on 8,936 labeled Tweets from the 2013 SXSW conference. The Tweets reference products from Apple and Google, and 60% of them are labeled neutral, 33% are positive, and 6% are negative. 

Only about a third of the records original records include brand labels. While we do not use the brand labels in our models, we impute the missing brands using keywords from the Tweet text to allow us to disaggregate the data by brands in future analyses.

Before modeling, we create a preprocessing function to prepare the Tweets for classification. The function uses a regex pattern and tokenizer to tokenize each Tweet. After tokenization, the function lower cases each token and removes stopwards and punctuation from the token list. The function then applies part of speech tags to each of the tokens before using the WordNetLemmatizer to lemmatize them. 

<img width="506" alt="Screenshot 2023-10-25 142043" src="https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/7037a72f-e7cf-429f-ae70-8e8196e29584">


## Modeling

Our project includes three models that are each intended to serve different use cases. To develop each of the models, we use an iterative approach that includes Multinomial Naive Bayes, Random Forest, logistic regression, ADA Boost, and XG Boost models. For each model type, we try separate models using count vectorization and TF-IDF vectorization. We also try models that use SMOTE to address the imbalance in the data. We create a class to handle the model iterations more efficiently.

The purpose of our first model is to detect positive Tweets. The operations team could use this to share positive public response with participating companies, which they could repost on their social media accounts, or to publicize positive reactions to the event to attract new companies to present at SXSW in the future.  For positive Tweet classification, we use a one-versus-rest binary classification model and maximize precision because we wanted to make sure that if the operations team is sharing positive social media posts or statistics with companies or publicing them on their own social media accounts that the Tweets actually are positive. 

Here is the ROC-AUC curve for our top 5 models:

![precision_top_5](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/8a6cb247-9dee-4995-a862-82e181aa64ba)

Our strongest model for this purpose is a Mutlinomial Naive Bayes classifier that uses TF-IDF vectorization. The model precision for train data is 68% and the precision for test data is 73%. Of the Tweets that it classified as positive, 163 actually were positive and 59 were false positives.

![conf_mat_precision_winner](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/af39d80e-042c-473a-ba53-c329e2a4b4ef)

Our second model detects negative Tweets to support the operations team in detecting operational issues that people are complaining about publicly on social media and responding efficiently. Once again, we use a one-versus-rest binary classification model, but for this model, we focus on recall score to optimize the percentage of actually negative Tweets that the model detects. We use this metric because we want to minimize the negative Tweets that the model does not detect.

Here is the ROC-AUC curve for our top 5 models:

![download](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/d602ce08-5af8-48c3-b414-b11c5d6b2a0f)

Our strongest model for this purpose is a Mutlinomial Naive Bayes classifier that uses TF-IDF vectorization and SMOTE. The model recall for train data is 98% and the recall for test data is 63%. Of the Tweets labeled as negative, it correctly classifies 90 as negative and incorrectly classifies 52.

![download](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/1f1b5c65-4460-4614-a756-c7bbe7b234f5)

Finally, our third model is a multi-class classifier that distinguishes between positive, negative, and neutral Tweets. The operations team could use this model to gain insights about public reaction to the products and presentations that companies share throughout the conference. The team could share these insights both with companies to help them quantify the impact of presenting at SXSW on social media response and internally to determine which companies to prioritize inviting to future conferences and which companies not to invite back. Given the purpose of the model, our key metric is accuracy score to make sure that the model's classifications are as correct as possible. 

Our strongest model for this purpose is a logistic regression classifier that uses TF-IDF vectorization. The model accuracy for train data is 69% and the accuracy for test data is 71%. Of the Tweets labeled as negative, it correctly classifies 90 as negative and incorrectly classifies 52. The confusion matrix below illustrates its specific classifications:

![download](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/434ee536-e86a-4afb-be19-a5b5fe2a6013)


## Evaluation and Conclusion
While our models substantially outperform dummy classifiers, they do not achieve the level of precision, recall, and accuracy that we would want to offer a client for real world use. We believe that the main cause source of the issue is the limited amount of data used to train the model. If we were to continue the project, our next steps would be to use a larger data set to train the model, to find data with date and time markers to allow us to visualize how the emotion classifications change over time, to explore relationships between the social media data and related data such as sales or attendance, and to create critical thresholds that identify the need for an immediate response. We would also want to pursue methods like Latent Dirilichlet Allocation and Sentiment Analysis to get more sophisticated insights than simple classification.
